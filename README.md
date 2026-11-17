# Домашнее задание к занятию "`Защита сети`" - `Olga Dunaevskaya`


### Задание 1

Сканирование sudo nmap -sA:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%201_%20%D0%B0%D1%82%D0%B0%D0%BA%D1%83%D1%8E%D1%89%D0%B0%D1%8F.png)

Лог Сурикаты:  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%97%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%201_%20%D0%B7%D0%B0%D1%89%D0%B8%D1%89%D0%B0%D0%B5%D0%BC%D0%B0%D1%8F.png)

Сканирование sudo nmap -sT:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_2.png)

Лог Сурикаты:  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_2_%D0%B7%D0%B0%D1%88%D0%B8%D1%89%D0%B0.png)

Сканирование sudo nmap -sS:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_3.png)

Лог Сурикаты:  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0_3.png)

Сканирование sudo nmap -sV:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_4.png)

Лог Сурикаты:  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_4_%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0.png)
 
 Fail2ban: ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D1%81%D1%82%D0%B0%D1%82%D1%83%D1%81.png)
 

### Выводы по заданию 1: 
Suricata видит сканирование Nmap. Она записала его в eve.json, а не в fast.log.
Важные поля из  журнала:
"in_iface":"enp0s8",
"event_type":"flow",
"src_ip":"192.168.56.10",
"dest_ip":"192.168.56.20",
"proto":"TCP"
Что это означает:
•	event_type: flow — Suricata зарегистрировала сетевой поток.
•	alerted: false — правило обнаружения не сработало, поэтому тревожное событие не сформировано.
То есть трафик зафиксирован. 
Сканирование -sT прошло, Suricata видит обращения с атакующей машины и записывает их в eve.json. Отсутствие тревог в fast.log не означает, что сканирование не обнаружено на уровне сетевых событий.
Третий вид сканирования:
•	src_ip: 192.168.56.10 — атакующая машина.
•	dest_ip: 192.168.56.20 — защищённая машина.
•	tcp_flags_ts: 02 — SYN-пакет от сканирующей машины.
•	tcp_flags_tc: 14 — ответ RST/ACK от защищённой машины для закрытых портов.
•	state: closed — TCP-соединение закрыто.
•	alerted: false — отдельная тревога не сформирована.
Suricata регистрирует сетевые события SYN-сканирования, но текущие правила не создают отдельные предупреждения о сканировании.
Четвертое скаинрование:
•	22/tcp open — открыт TCP-порт 22.
•	SERVICE: ssh — на порту работает SSH.
•	VERSION: OpenSSH 8.9p1 Ubuntu 3ubuntu0.18 — обнаружена версия SSH-сервера.
•	OS: Linux — Nmap определил операционную систему как Linux.
•	999 closed ports — остальные проверенные порты закрыты.
Сканирование -sV позволило определить не только открытый порт, но и конкретную службу с её версией. Это полезно для инвентаризации сервисов и оценки потенциальных уязвимостей.
Не проверяли Fail2Ban после первых четырёх сканирований Nmap, потому что Suricata и Fail2Ban выполняют разные задачи и реагируют на разные типы событий.Suricata, которая анализирует именно сетевой трафик. Fail2Ban не предназначен для обнаружения самого факта сканирования портов.



   
### Задание 2

Подготовка к заданию 2:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20_2_%20%D0%BF%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.png)

Запуск Гидры (атака):  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B0%D1%82%D0%B0%D0%BA%D0%B0_2_2.png)

Fail2ban после атаки:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0_2_2.png)

Логи после атки:  ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D1%81%D1%82%D0%B0%D1%82%D1%83%D1%81_%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D0%B0.png)

Снятие блокировки:   ![alt text](https://github.com/OlgaStudent1/Suricata/blob/main/img/%D1%81%D0%BD%D1%8F%D1%82%D0%B8%D0%B5%20%D0%B1%D0%BB%D0%BE%D0%BA%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B8.png)

Fail2Ban отслеживал SSH-аутентификацию: его задача — обнаружить пять неудачных попыток входа за десять минут, что собственно было сделано.
 
