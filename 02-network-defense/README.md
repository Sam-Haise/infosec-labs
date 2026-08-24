# 🛡️ Раздел 02 — Защита сети (Cisco / EVE-NG)

Восемь практических работ по защите сетевой инфраструктуры. Схема каждой работы —
**атака → наблюдение → защита**: сначала атака воспроизводится на стенде, затем
настраивается и проверяется механизм противодействия. Это даёт понимание защиты
не на уровне определений, а на уровне пакетов и конфигов.

**Стенд:** эмуляция сети **EVE-NG**, коммутаторы и маршрутизаторы **Cisco IOL**,
узлы **Kali Linux** и **Windows**.
**Инструменты:** Cisco IOS (Port Security, DAI, DHCP snooping), nping, macof,
Yersinia, arpspoof, Scapy, Nmap, Metasploit/Armitage, Wireshark, tcpdump.

---

## Работы

| # | Тема | Атака / что изучено | Защита / вывод |
|:-:|------|---------------------|----------------|
| 1 | [Таблица CAM + Port Security](reports/lab1-cam-port-security.docx) | Повреждение и **переполнение таблицы CAM** (`nping`, `macof`) → коммутатор работает как хаб | Настроен **Port Security** (protect, 1 MAC/порт) — атака заблокирована |
| 2 | [Подмена DHCP-сервера](reports/lab2-dhcp-spoofing.docx) | **DHCP starvation** (Yersinia) — истощён весь пул адресов; попытка rogue-сервера | Понимание, зачем нужен **DHCP snooping**; честно описан баг Yersinia 0.8.2, оборвавший rogue-этап |
| 3 | [ARP: DoS и MitM](reports/lab3-arp-mitm.docx) | Повреждение ARP-таблицы (`arping`) → **DoS**; включение `ip_forward` → **MitM** «человек посередине» | ARP не имеет аутентификации — отсюда необходимость DAI (работа 4) |
| 4 | [Dynamic ARP Inspection](reports/lab4-dynamic-arp-inspection.docx) | 4 типа аномальных ARP-сообщений (`arping`, `arpspoof`, **Scapy**) | **DAI + ARP ACL + validate** отфильтровали все аномалии; проверено по счётчикам коммутатора |
| 5 | [Разведка сети Nmap](reports/lab5-nmap-scanning.docx) | Обнаружение узлов, сканирование TCP/UDP (**SYN/FIN/XMAS/NULL/UDP**), определение ОС и сервисов | Сравнение методов: SYN — самый точный; понимание, что «видит» атакующий |
| 6 | [Пентест уязвимого сервера](reports/lab6-metasploit-pentest.docx) | **Metasploit / Armitage** на Metasploitable 2: эксплойт `vsftpd 2.3.4 backdoor` (CVE-2011-2523) → root | Найдено **6 уязвимостей** (vsftpd, Samba CVE-2007-2447, UnrealIRCd, telnet/r-сервисы, bindshell, слабый Tomcat) + **меры по устранению каждой** |
| 7 | [HTTP vs HTTPS в Wireshark](reports/lab7-http-https-wireshark.docx) | Перехват формы логина: **HTTP POST — логин/пароль открытым текстом**; HTTPS — то же в TLS | Наглядно, почему HTTPS обязателен; пассивный перехват TLS бесполезен |
| 8 | [Telnet vs SSH в Wireshark](reports/lab8-telnet-ssh-wireshark.docx) | Follow TCP Stream: **Telnet отдаёт пароль в открытом виде**, SSH — только рукопожатие | Обоснование повсеместного перехода на SSH для управления |

📎 Полные отчёты со скриншотами — в папке [`reports/`](reports).

---

## Что демонстрирует раздел

- Практическое понимание **канальных атак** (CAM, ARP, DHCP) и штатных средств
  защиты Cisco против них.
- Умение **настроить и проверить** Port Security и Dynamic ARP Inspection, а не
  просто назвать их.
- Полный мини-пентест: **разведка (Nmap) → эксплуатация (Metasploit) → отчёт с
  мерами устранения**.
- Чтение трафика на уровне пакетов и доказательное сравнение защищённых и
  незащищённых протоколов (HTTP/HTTPS, Telnet/SSH).
- Инженерная честность: там, где инструмент дал сбой (Yersinia, PMKID),
  результат описан как есть, с разбором причины.

> 🎓 Все узлы, адреса (`192.168.1.0/24`, `10.0.0.0/24`) и цели (Metasploitable,
> `demo.testfire.net`) — учебные, внутри изолированного стенда EVE-NG.
