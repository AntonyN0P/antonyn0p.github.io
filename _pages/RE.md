---
permalink: /RE/
title: "Malware analysis notes"
toc: true
author_profile: true
---

## Песочницы и сканеры для динамического анализа вредоносного ПО

- [any.run](https://app.any.run/) — интерактивная онлайн-песочница.
- [BoomBox](https://github.com/nbeede/BoomBox) — автоматическое развертывание лаборатории для исследования вредоносных программ Cuckoo Sandbox с помощью Packer и Vagrant.
- [Cuckoo Sandbox](https://cuckoosandbox.org/) — автономная песочница с открытым исходным кодом и автоматизированная система анализа.
- [cuckoo-modified](https://github.com/brad-sp/cuckoo-modified) — модифицированная версия Cuckoo Sandbox, выпущенная под лицензией GPL.
- [cuckoo-modified-api](https://github.com/keithjjones/cuckoo-modified-api) — API Python, используемый для управления песочницей, модифицированной cuckoo.
- [detux](https://github.com/detuxsandbox/detux/) — песочница, разработанная для анализа трафика вредоносных программ Linux и захвата IOC.
- [DRAKVUF](https://github.com/tklengyel/drakvuf) — Система динамического анализа вредоносных программ.
- [firmware.re](http://firmware.re/) — Распаковывает, сканирует и анализирует практически любую версию прошивки.
- [HaboMalHunter](https://github.com/Tencent/HaboMalHunter) — инструмент автоматизированного анализа вредоносных программ для файлов ELF Linux.
- [Jotti](https://virusscan.jotti.org/en) — бесплатный мульти-AV-сканер онлайн.
- [Limon](https://github.com/monnappa22/Limon) — песочница для анализа вредоносного ПО для Linux.
- [Malheur](https://github.com/rieck/malheur) — Автоматический изолированный анализ поведения вредоносных программ.
- [malice.io](https://github.com/maliceio/malice) — Масштабируемый фреймворк для анализа вредоносных программ.
- [malsub](https://github.com/diogo-fernan/malsub) — API-фреймворк для онлайн-сервисов анализа вредоносных программ и URL-адресов.
- [MetaDefender Cloud](https://metadefender.opswat.com/) — бесплатно сканируйте файл, хэш, IP-адрес, URL-адрес или доменный адрес на наличие вредоносных программ.
- [Noriben](https://github.com/Rurik/Noriben) — использует Sysinternals Procmon для сбора информации о вредоносных программах в изолированной среде.
- [PacketTotal](https://lab.dynamite.ai/) — PacketTotal — это онлайн-движок для анализа файлов .pcap и визуализации сетевого трафика внутри них.
- [sandboxapi](https://github.com/InQuest/sandboxapi) — библиотека Python для создания интеграций с несколькими изолированными программными средами с открытым исходным кодом и коммерческими вредоносными программами.
- [SEE](https://github.com/WithSecureOpenSource/see) — Sandboxed Execution Environment (SEE) — это платформа для построения автоматизации тестирования в защищенных средах.
- [Hybrid Analysis](https://www.hybrid-analysis.com/) — онлайн-инструмент для анализа вредоносного ПО на базе VxSandbox (имеет API).
- [VirusTotal](https://www.virustotal.com/) — бесплатный онлайн-анализ образцов вредоносных программ и URL-адресов
- [InQuest Deep File Inspection](https://labs.inquest.net/dfi) — загружайте распространенные вредоносные программы для глубокой проверки файлов и эвристического анализа.
- [Visualize_Logs](https://github.com/keithjjones/visualize_logs) — библиотека визуализации с открытым исходным кодом и инструменты командной строки для журналов. (Cuckoo, Procmon, и.т.д)

- [Список Зельцера](https://zeltser.com/automated-malware-analysis/) — бесплатные автоматизированные песочницы и сервисы, составленные Ленни Зельцером.
- [ProcDot](http://www.procdot.com/) — набор графических инструментов для анализа вредоносных программ.

## Инструменты анализа подозрительных документов на наличие вредоносного кода (Shellcode, VBA, JS и.т.д)
- [PDF Tools](https://blog.didierstevens.com/programs/pdf-tools/) — pdfid, pdf-parser и многое другое от Didier Stevens.
- [PDF X-Ray Lite](https://github.com/9b/pdfxray_lite) — инструмент для анализа PDF, бесплатная версия PDF X-RAY.
- [peepdf](http://eternal-todo.com/tools/peepdf-pdf-analysis-tool) — инструмент на Python для изучения потенциально вредоносных PDF-файлов (парсинг объектов, потоков, декодирование декомпрессирование и.т.д).
- [AnalyzePDF](https://github.com/hiddenillusion/AnalyzePDF) — инструмент для анализа PDF-файлов и попытки определить, являются ли они вредоносными.
- [Pdf-parser.py](https://github.com/DidierStevens/DidierStevensSuite/blob/master/pdf-parser.py) — аналогичная peepdf утилита, для анализа pdf;
- [malpdfobj](https://github.com/9b/malpdfobj) — отобразить вредоносные PDF-файлы в представление JSON.
- [Origami PDF](https://code.google.com/archive/p/origami-pdf) — инструмент для анализа вредоносных PDF-файлов и многого другого.
- [PDF Examiner](http://www.pdfexaminer.com/) — Анализ подозрительных файлов PDF.
- [olevba](http://www.decalage.info/python/olevba) — скрипт для разбора документов OLE и OpenXML и извлечения полезной информации.
- [OfficeMalScanner](http://www.reconstructer.org/code.html) — Сканирует вредоносные следы в документах MS Office.
- [box-js](https://github.com/CapacitorSet/box-js) — инструмент для изучения вредоносных программ JavaScript с поддержкой JScript/WScript и эмуляцией ActiveX.
- [JS Beautifier](http://jsbeautifier.org/) — распаковка и деобфускация JavaScript.
- [Spidermonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey) — движок JavaScript от Mozilla для отладки вредоносного JS.
- [box-js](https://github.com/CapacitorSet/box-js) — инструмент для изучения вредоносных программ JavaScript с поддержкой JScript/WScript и эмуляцией ActiveX.
- [diStorm](http://www.ragestorm.net/distorm/) — дизассемблер для анализа вредоносного шеллкода.
- [libemu](http://libemu.carnivore.it/) — библиотека и инструменты для эмуляции шеллкода x86.

## Инструменты анализа вредоносной активности в сетевом трафике

- [FakeNet-NG](https://github.com/fireeye/flare-fakenet-ng) — инструмент динамического сетевого анализа следующего поколения (хорошее описание в книге «Вскрытие покажет»).
- [INetSim](http://www.inetsim.org/) — эмуляция сетевых служб, полезная при создании лаборатории вредоносных программ (хорошее описание в книге «Вскрытие покажет»).
- [ApateDNS](https://fireeye.market/apps/211380) — подделывает DNS-ответы для IP-адресов, которые относятся к определенному пользователю. Позволяет отслеживать запросы вредоносного ПО(хорошее описание в книге «Вскрытие покажет»).
- [Fiddler](https://www.telerik.com/fiddler) — перехватывающий веб-прокси, предназначенный для «веб-отладки».
- [Bro](https://www.bro.org/) — анализатор протоколов, работающий в невероятных масштабах; как файловые, так и сетевые протоколы.
- [BroYara](https://github.com/hempnall/broyara) — Используйте правила Yara от Bro.
- [Chopshop](https://github.com/MITRECND/chopshop) — анализ структуры и декодирование протоколов.
- [HTTPReplay](https://github.com/hatching/httpreplay) — библиотека для разбора и чтения файлов PCAP, включая сессии TLS с использованием ключей TLS (используется в песочнице Cuckoo).
- [Malcolm](https://github.com/idaholab/Malcolm) — мощный, легко развертываемый набор инструментов для анализа сетевого трафика с целью получения артефактов (файлы PCAP) и журналов Zeek.
- [mitmproxy](https://mitmproxy.org/) — позволяет перехватывать сетевой трафик «на лету».
- [NetworkMiner](http://www.netresec.com/?page=NetworkMiner) — инструмент для судебного анализа сети с бесплатной версией (широко используется в DFIR).
- [ngrep](https://github.com/jpr5/ngrep) — поиск по сетевому трафику, аналог grep в linux.
- [Tcpdump](https://www.tcpdump.org/) — сбор сетевого трафика.
- [tcpxtract](https://tcpxtract.sourceforge.net/) — Извлечение файлов из сетевого трафика.
- [Wireshark](https://www.wireshark.org/) — инструмент для анализа сетевого трафика.




