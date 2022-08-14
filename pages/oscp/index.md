---
layer: article
title: OSCP 备考
tags: oscp
---

以下内容为个人备考OSCP 2022整理

#### 学习资料

[Kali Linux](https://www.iihack.com/pages/tools/Kali-Linux.html)

《PWK OSCP V2.0.1》

《Kali-Linux-Revealed-2021-edition》

### 备考

[Linux Command Line Fun](https://iihack.com/pages/oscp/Linux-Command-Line-Fun.html)

[Windows Command Line Fun](https://iihack.com/pages/oscp/Windows-Command-Line-Fun.html)

Practical Tools

Bash Scripting

Passive Information Gathering

Active Information Gathering

Vulnerability Scanning

Web Application Attacks

Introduction to Buffer Overflows

Windows Buffer Overflows

Locating Public Exploits

Fixing Exploits

File Transfers

Antivirus Evasion

[Privilege Escalation](https://iihack.com/pages/oscp/Privilege-Escalation.html)

Password Attacks

Port Redirection and Tunneling

Active Directory Attacks

The Metasploit Framework

PowerShell Empire

Assembling the Pieces: Penetration Test Breakdown

[Common Linux Vulnerabilities](https://iihack.com/pages/oscp/Common-Linux-Vulnerabilities.html)

### 推荐

- [NetSecFocus Trophy Room](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8)

其中Vulnhub部分（内容见下表），除去失效的[Alpha 1: https://www.vulnhub.com/entry/alfa-1,655/](https://www.vulnhub.com/entry/alfa-1,655/) 无法打开和[metasploitable3](https://github.com/rapid7/metasploitable3)大家可以自建，其他的已经按尾部编号上传至[Pan](https://pan.iihack.com/Vulnhub) 下载速度快慢就看阿里云了

|名称 |  [vulnhub地址](https://www.vulnhub.com/)                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **List of PWK/OSCP boxes from the previous versions  of the course** |                                                              |
| Kioptrix:   Level 1 (#1):                                    | <https://www.vulnhub.com/entry/kioptrix-level-1-1,22/>       |
| Kioptrix:   Level 1.1 (#2):                                  | <https://www.vulnhub.com/entry/kioptrix-level-11-2,23/>      |
| Kioptrix:   Level 1.2 (#3):                                  | <https://www.vulnhub.com/entry/kioptrix-level-12-3,24/>      |
| Kioptrix:   Level 1.3 (#4):                                  | <https://www.vulnhub.com/entry/kioptrix-level-13-4,25>       |
| Kioptrix:   2014:                                            | <https://www.vulnhub.com/entry/kioptrix-2014-5,62/>          |
| FristiLeaks 1.3:                                             | <https://www.vulnhub.com/entry/fristileaks-13,133/>          |
| Stapler   1:                                                 | <https://www.vulnhub.com/entry/stapler-1,150/>               |
| VulnOS   2:                                                  | <https://www.vulnhub.com/entry/vulnos-2,147/>                |
| SickOs   1.2:                                                | <https://www.vulnhub.com/entry/sickos-12,144/>               |
| HackLAB:   Vulnix:                                           | <https://www.vulnhub.com/entry/hacklab-vulnix,48/>           |
| /dev/random: scream:                                         | <https://www.vulnhub.com/entry/devrandom-scream,47/>         |
| pWnOS   2.0:                                                 | <https://www.vulnhub.com/entry/pwnos-20-pre-release,34/>     |
| SkyTower   1:                                                | <https://www.vulnhub.com/entry/skytower-1,96/>               |
| Mr-Robot   1:                                                | <https://www.vulnhub.com/entry/mr-robot-1,151/>              |
| PwnLab:                                                      | <https://www.vulnhub.com/entry/pwnlab-init,158/>             |
| Lin.Security:                                                | <https://www.vulnhub.com/entry/linsecurity-1,244/>           |
| Temple   of Doom:                                            | <https://www.vulnhub.com/entry/temple-of-doom-1,243/>        |
| Pinkys   Palace v2:                                          | <https://www.vulnhub.com/entry/pinkys-palace-v2,229/>        |
| Zico2:                                                       | <https://www.vulnhub.com/entry/zico2-1,210/>                 |
| Wintermute:                                                  | <https://www.vulnhub.com/entry/wintermute-1,239/>            |
| Tr0ll 1:                                                     | <https://www.vulnhub.com/entry/tr0ll-1,100/>                 |
| Tr0ll 2:                                                     | <https://www.vulnhub.com/entry/tr0ll-2,107/>                 |
| Web   Developer 1:                                           | <https://www.vulnhub.com/entry/web-developer-1,288/>         |
| SolidState:                                                  | <https://www.vulnhub.com/entry/solidstate-1,261/>            |
| Hackme   1:                                                  | <https://www.vulnhub.com/entry/hackme-1,330/>                |
| Escalate_Linux: 1:                                           | <https://www.vulnhub.com/entry/escalate_linux-1,323/>        |
| DC 6:                                                        | <https://www.vulnhub.com/entry/dc-6,315/>                    |
| **Current   Systems that are Simliar to the current PWK/OSCP course** |                                                              |
| DC 9:                                                        | <https://www.vulnhub.com/entry/dc-9,412/>                    |
| Digitalworld.local (Bravery):                                | <https://www.vulnhub.com/entry/digitalworldlocal-bravery,281/> |
| Digitalworld.local (Development):                            | <https://www.vulnhub.com/entry/digitalworldlocal-development,280/> |
| Digitalworld.local (Mercy v2):                               | <https://www.vulnhub.com/entry/digitalworldlocal-mercy-v2,263/> |
| Digitalworld.local (JOY):                                    | <https://www.vulnhub.com/entry/digitalworldlocal-joy,298/>   |
| Digitalword.local (FALL):                                    | <https://www.vulnhub.com/entry/digitalworldlocal-fall,726/>  |
| Prime 1:                                                     | <https://www.vulnhub.com/entry/prime-1,358/>                 |
| Misdirection 1:                                              | <https://www.vulnhub.com/entry/misdirection-1,371/>          |
| Sar 1:                                                       | <https://www.vulnhub.com/entry/sar-1,425/>                   |
| Djinn 1:                                                     | <https://www.vulnhub.com/entry/djinn-1,397/>                 |
| EVM 1:                                                       | <https://www.vulnhub.com/entry/evm-1,391/>                   |
| DerpNStink 1:                                                | <https://www.vulnhub.com/entry/derpnstink-1,221/>            |
| RickdiculouslyEasy 1:                                        | <https://www.vulnhub.com/entry/rickdiculouslyeasy-1,207/>    |
| Tommy Boy 1:                                                 | <https://www.vulnhub.com/entry/tommy-boy-1,157/>             |
| Breach 1:                                                    | <https://www.vulnhub.com/entry/breach-1,152/>                |
| Breach 2.1:                                                  | <https://www.vulnhub.com/entry/breach-21,159/>               |
| Breach 3.0.1:                                                | <https://www.vulnhub.com/entry/breach-301,177/>              |
| NullByte:                                                    | <https://www.vulnhub.com/entry/nullbyte-1,126/>              |
| Bob 1.0.1:                                                   | <https://www.vulnhub.com/entry/bob-101,226/>                 |
| Toppo 1:                                                     | <https://www.vulnhub.com/entry/toppo-1,245/>                 |
| W34kn3ss 1:                                                  | <https://www.vulnhub.com/entry/w34kn3ss-1,270/>              |
| GoldenEye 1:                                                 | <https://www.vulnhub.com/entry/goldeneye-1,240/>             |
| Infosec Prep OSCP Box:                                       | <https://www.vulnhub.com/entry/infosec-prep-oscp,508/>       |
| LemonSqueezy:                                                | <https://www.vulnhub.com/entry/lemonsqueezy-1,473/>          |
| Brainpan 1:                                                  | <https://www.vulnhub.com/entry/brainpan-1,51/>               |
| Lord of the root 1.0.1:                                      | <https://www.vulnhub.com/entry/lord-of-the-root-101,129/>    |
| Tiki-1:                                                      | <https://www.vulnhub.com/entry/tiki-1,525/>                  |
| Healthcare 1:                                                | <https://www.vulnhub.com/entry/healthcare-1,522/>            |
| Photographer 1:                                              | <https://www.vulnhub.com/entry/photographer-1,519/>          |
| Glasglow 1.1:                                                | <https://www.vulnhub.com/entry/glasgow-smile-11,491/>        |
| DevGuru 1:                                                   | <https://www.vulnhub.com/entry/devguru-1,620/>               |
| Alpha 1:                                                     | <https://www.vulnhub.com/entry/alfa-1,655/>                  |
| Hack Me Please:                                              | <https://www.vulnhub.com/entry/hack-me-please-1,731/>        |
| **Other   Vm's to check out!**                               |                                                              |
| IMF:                                                         | <https://www.vulnhub.com/entry/imf-1,162/>                   |
| Tommy Boy:                                                   | <https://www.vulnhub.com/entry/tommy-boy-1,157/>             |
| Billy Madison:                                               | <https://www.vulnhub.com/entry/billy-madison-11,161/>        |
| Tr0ll1:                                                      | <https://www.vulnhub.com/entry/tr0ll-1,100/>                 |
| Tr0ll2:                                                      | <https://www.vulnhub.com/entry/tr0ll-2,107/>                 |
| Wallaby's   Nightmare:                                       | <https://www.vulnhub.com/entry/wallabys-nightmare-v102,176/> |
| Moria:                                                       | <https://www.vulnhub.com/entry/moria-1,187/>                 |
| BSides Vancouver 2018:                                       | <https://www.vulnhub.com/entry/bsides-vancouver-2018-workshop,231/> |
| DEFCON Toronto Galahad:                                      | <https://www.vulnhub.com/entry/defcon-toronto-galahad,194/>  |
| Spydersec:                                                   | <https://www.vulnhub.com/entry/spydersec-challenge,128/>     |
| Pinkys   Palace v3:                                          | <https://www.vulnhub.com/entry/pinkys-palace-v3,237/>        |
| Pinkys   Palace v4:                                          | <https://www.vulnhub.com/entry/pinkys-palace-v4,265/>        |
| Vulnerable Docker 1:                                         | <https://www.vulnhub.com/entry/vulnerable-docker-1,208/>     |
| Node 1:                                                      | <https://www.vulnhub.com/entry/node-1,252/>                  |
| Troll 3:                                                     | <https://www.vulnhub.com/entry/tr0ll-3,340/>                 |
| Readme 1:                                                    | <https://www.vulnhub.com/entry/readme-1,336/>                |
| OZ:                                                          | <https://www.vulnhub.com/entry/oz-1,317/>                    |
| Metasploitable 3:                                            | <https://github.com/rapid7/metasploitable3>                  |
| Election 1:                                                  | <https://www.vulnhub.com/entry/election-1,503/>              |
| Pinkys Palace v1:                                            | <https://www.vulnhub.com/entry/pinkys-palace-v1,225/>        |
| Hacker Kid: 1.0.1:                                           | <https://www.vulnhub.com/entry/hacker-kid-101,719/>          |
