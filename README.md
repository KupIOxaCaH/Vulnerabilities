# Домашнее задание к занятию «Уязвимости и атаки на информационные системы» - Бойко Кирилл

------

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
  
*Приведите ответ в свободной форме.*  

---

#### Решение 1

nmap показал множество открытых портов. Среди них такие:

21/tcp – FTP

22/tcp – SSH

23/tcp - Telnet

25/tcp – SMTP

53/tcp,udp – DNS

80/tcp – HTTP

111/tcp,udp – RPC

139/tcp, 445/tcp – SMB

3306/tcp – MySQL

5900/tcp – VNC

8180/tcp – Apache Tomcat

6667/tcp – IRC (UnrealIRCd)

**Уязвимости**

vsftpd 2.3.4 backdoor (FTP, порт 21)

https://www.exploit-db.com/exploits/17491

UnrealIRCd backdoor (IRC, порт 6667)

https://www.exploit-db.com/exploits/16922

Apache Tomcat Manager уязвимая сборка (порт 8180/tcp)

https://www.exploit-db.com/exploits/31433

---

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*

---

#### Решение 2

**SYN сканирование:**

[syn_scan](https://github.com/KupIOxaCaH/Vulnerabilities/blob/main/materials/syn_scan.pcapng)

Отправляет SYN пакет

Открытый порт: получает SYN-ACK

Закрытый порт: получает RST

**FIN сканирование:**

[fin_scan](https://github.com/KupIOxaCaH/Vulnerabilities/blob/main/materials/fin_scan.pcapng)

Отправляет пакет с флагом FIN

Открытый порт: нет ответа

Закрытый порт: отправляет RST

**Xmas сканирование:**

[xmas_scan](https://github.com/KupIOxaCaH/Vulnerabilities/blob/main/materials/xmas_scan.pcapng)

Отправляет пакет с FIN+PSH+URG

Работает как FIN сканирование

Открытый: нет ответа, закрытый: RST

**UDP сканирование:**

[udp_scan](https://github.com/KupIOxaCaH/Vulnerabilities/blob/main/materials/udp_scan.pcapng)

Отправляет UDP пакеты

Открытый: может не отвечать

Закрытый: ICMP Port Unreachable

**Как отвечает сервер:**

При SYN сканировании сервер отвечает SYN-ACK на открытые порты и RST на закрытые

​При FIN/Xmas сканировании сервер отправляет RST только на закрытые порты, на открытые - молчит

​При UDP сканировании сервер отправляет ICMP ошибку для закрытых портов

---
