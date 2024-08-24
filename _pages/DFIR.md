---
permalink: /RE/
title: "Malware analysis notes"
toc: true
author_profile: true
---

## Образы ОС для расследования инцидентов и анализа вредоносного ПО с уже предустановленными утилитами (Free)
- [SIFT](https://www.sans.org/tools/sift-workstation/) – от института SANS, на базе ОС Ubuntu (версии ОС обновляются и поддерживаются).
- [FLARE VM by Mandiant](https://github.com/mandiant/flare-vm) – на базе ОС Windows 7 & Windows 10+, в проекте поставляется скрипт который позволяет подготовить ОС, загрузить полезные утилиты, для исследования вредоносного ПО, проведения расследования на базе Windows.
- [TSURUGI](https://tsurugi-linux.org/tsurugi_linux.php) - ОС на базе Ubuntu, собранная энтузиастами-профессионалами DFIR, содержит на своём борту все необходимые утилиты для триажа, проведения расследований.
- [REMnux](https://remnux.org/) – образ от комьюнити экспертов по анализу вредоносного ПО на базе ОС Ubuntu (версии ОС обновляются и поддерживаются), идеально подходит для анализа вредоносного ПО, уже предустановлены все необходимые утилиты.
- [Kali](https://www.kali.org/) – известная многим ОС Kali не только годна для пентеста, но также имеет Forensics Mode.

## Исследование сетевого трафика на наличие артефактов

- [NetworkMiner](https://www.netresec.com/?page=Networkminer) – есть триалка, очень удобная утилита по анализу дампа трафика.
- [WireShark](https://www.wireshark.org/) - бесплатная и, пожалуй, лучший софт для анализа в оффлайн/онлайн режиме.
- [naft-gfe.py](http://blog.didierstevens.com/2012/03/12/naft-release/) – позволяет извлекать дамп трафика в формате .PCAP из дампов оперативной памяти для дальнейшего анализа.
- [Ethscan (by Jamaal)](https://github.com/byt3bl33d3r/jamaal-re-tools/blob/master/volplugins/ethscan.py) – плагин для Volatility, позволяет извлекать сетевой трафик в формате pcap и анализировать его.
- [netstat.exe](https://github.com/byt3bl33d3r/jamaal-re-tools/blob/master/volplugins/ethscan.py) – нативная утилита для динамического анализа активности.
- [TCPView.exe](https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview) - для динамического анализа активности.

## Работа с дампами оперативной памяти

- [FTK Imager](https://www.exterro.com/forensic-toolkit) - софт для сбора и анализа артефактов в Windows (Есть Demo)
- [BelkaSoft RAM Capturer](https://belkasoft.com/ru/get) – снятие дампа оперативной памяти (Есть Demo)
- [Winpmem](https://winpmem.velocidex.com/) – снятие дампов оперативной памяти (для Rekall)
- [DumpIt](https://github.com/thimbleweed/All-In-USB/tree/master/utilities/DumpIt) – снятие дампа оперативной памяти (free)

## Анализ оперативной памяти

- [Volatility](https://www.volatilityfoundation.org/) – одно из лучших средств для анализа дампа оперативной памяти (скомпилированный исполняемый файл).
- [Vol.py](https://github.com/volatilityfoundation/volatility) - одно из лучших средств для анализа дампа оперативной памяти, реализация на Python2.
- [Rekall](http://www.rekall-forensic.com/) – аналог Volatility.
- [WindowsSCOPE](http://www.rekall-forensic.com/) – есть триал версия.
- [Redline (by Mandiant - free)](https://www.fireeye.com/services/freeware/redline.html)

## Анализ файлов реестра Windows

- [Registry Viewer 2.0.0 by Access Data](https://accessdata.com/product-download/registry-viewer-2-0-0)
- [Registry Explorer/RECmd](https://ericzimmerman.github.io/) 
- [Registry Recon](https://arsenalrecon.com/products/)
- [RegRipper3.0](https://github.com/keydet89/RegRipper3.0)

## Исследование файловой системы Windows
- [MFT-Parser](http://az4n6.blogspot.com/2015/09/whos-your-master-mft-parsers-reviewed.html) - Сравнение результатов парсинга MFT
- [MFTEcmd](https://binaryforay.blogspot.com/2018/06/introducing-mftecmd.html) - MFT Parser by Eric Zimmerman
- [MFTExtractor](https://github.com/aarsakian/MFTExtractor) - написанный на go parser MFT
- [NTFS journal parser](http://strozfriedberg.github.io/ntfs-linker/)
- [NTFS USN Journal parser](https://github.com/PoorBillionaire/USN-Journal-Parser)
- [RecuperaBit](https://github.com/Lazza/RecuperaBit)
- [Python-ntfs](https://github.com/williballenthin/python-ntfs) - библиотека для анализа NTFS на python

## Централизованный сбор артефактов (Triage) c ОС Linux

- [CatScale](https://labs.withsecure.com/tools/cat-scale-linux-incident-response-collection) - проект впервые представлен в 2019 году на SANS DFIR Summit, в текущее время активно поддерживается и развивается. Инструментарий позволяет собрать большинство необходимых для расследований артефактов в *nix.
- [Velociraptor](https://docs.velociraptor.app/) - ещё один фреймворк для сбора артефактов с разных ОС (в тч Windows), кроме возможности удаленно собирать артефакты, имеет удобный веб-интерфейс и даже public API.
- [Google Rapid Response](https://grr-doc.readthedocs.io/en/latest/) - позволяет развернуть клиент серверную архитектуру для централизованного сбора артефактов с хостов и проведения расследования.
- [TuxResponse](https://github.com/la3ar0v/) - более простая реализация инструмента для траижа.
- [UAC](https://github.com/tclahr/uac) - Bash-скрипт для сбора артефактов.
- [Unix_Collector](https://github.com/op7ic/unix_collector) - ещё один bash-скрипт для сбора базовых артефактов.

## Централизованный сбор артефактов (Triage) c ОС Windows

- [CyLR](https://github.com/orlikoski/CyLR) — Live Response Collection tool by Alan Orlikoski and Jason Yegge, довольно быстрый сбор необходимых артефактов без использования Win API, на выходе zip файл.
- [KAPE](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape) - Live Response Collection tool by Eric Zimmerman and team, содержит различные шаблоны для триажа. Является одной из самых популярных утилит для сбора артефактов с Windows систем.
- [EDD](https://www.magnetforensics.com/resources/encrypted-disk-detector/) - Encrypted Disk Detector by Magnet Forensics, бесплатная утилита для проверки шифрования диска.
- [Velociraptor](https://docs.velociraptor.app/) - ещё один фреймворк для сбора артефактов.

## Полезная литература
The Art of Memory Forensics by Michael Hale Ligh, Andrew Case, Jamie Levy, Aaron Walters;
File System Forensics Analysis by Brian Carrier;
Windows Registry Forensics Advanced Digital Forensic Analysis of the Windows Registry by Harlan Carvey;
Windows Forensics Analysis by Harlan Carvey;
Digital Forensics and Incident Response by Gerard Johansen;
INCIDENT RESPONSE & COMPUTER FORENSICS, SECOND EDITION by Kevin Mandia, Chris Prosise & Matt Pepe;
FORENSIC ANALYSIS OF UNALLOCATED SPACE IN WINDOWS REGISTRY HIVE FILES by Jolanta Thomassen (диссертация);
