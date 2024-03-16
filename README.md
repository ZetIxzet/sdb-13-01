# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Воронин Алексей`



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

### Решение

```
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login?
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
```

OpenSSH 2.3 < 7.7 - Username Enumeration https://www.exploit-db.com/exploits/45233
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit) https://www.exploit-db.com/exploits/42084
PostgreSQL 8.3.6 - Low Cost Function Information Disclosure https://www.exploit-db.com/exploits/32847

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*


### Решение

SYN: отправляются SYN пакеты на все сканируемые порты. Открытые порты отвечают пакетом SYN ACK, закрытые порты отвечают RST ACK
![sS](https://github.com/ZetIxzet/sdb-13-01/blob/main/172427.png)

FIN: отправляются FIN пакеты на все сканируемые порты. Закрытые порты отвечают RST. Остальные молчат.
![sF](https://github.com/ZetIxzet/sdb-13-01/blob/main/172445.png)

Xmas: отправляются пакеты c флагами FIN, PSH и URG на все сканируемые порты. Закрытые порты отвечают RST ACK. Остальные молчат.
![sX](https://github.com/ZetIxzet/sdb-13-01/blob/main/172513.png)

UDP: отправляются UDP пакеты. Для большинства портов пустые, но для некоторых популярных отправляется специфический для протокола payload. Дальше состояние порта определяется по наличию/отсутствию ответа UDP или ICNP.
![sU](https://github.com/ZetIxzet/sdb-13-01/blob/main/172638.png)

