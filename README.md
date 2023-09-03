Задание 1
Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

sudo nmap -sA < ip-адрес >

sudo nmap -sT < ip-адрес >

sudo nmap -sS < ip-адрес >

sudo nmap -sV < ip-адрес >

![Снимок49](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/8adac2d5-8350-4cbf-8ad1-a8112a0d77f1)

Suricata:

sudo nmap -sA < ip-адрес > - логов не было, видимо вид атаки не предусмотрен  в правилах по умолчанию .

В остальных логах информация о потенциально опасном  трафике и возможной утечки информации. При этом порт назначения и отправителя постоянно изменяется.
 
![Снимок50](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/eb361619-7114-4b7d-a985-913e171803c8)

![Снимок52](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/87318a5f-b853-44a0-98fa-bdee171f1202)

Fail2Ban показал активность по SSH с атакуемого хоста.

![Снимок51](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/807805c6-0765-493d-ae5b-3f6308c138e7)

***


Задание 2
---
Проведите атаку на подбор пароля для службы SSH:

hydra -L users.txt -P pass.txt < ip-адрес > ssh

Настройка hydra:

создайте два файла: users.txt и pass.txt;

в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по hydra: https://kali.tools/?p=1847.

Включение защиты SSH для Fail2Ban:

открыть файл /etc/fail2ban/jail.conf,

найти секцию ssh,

установить enabled в true.

Дополнительная информация по Fail2Ban:https://putty.org.ru/articles/fail2ban-ssh.html.

В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.

**Ответ**

С выключеной секцией SSH etc/fail2ban/jail.conf пароль был подобран

![Снимок53](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/49365026-4864-488e-830d-584d3d601f99)

Suricata показала, что к хосту пытаются подключится по SSH, при этом порт отправителя меняется.

![Снимок55](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/8f9c8936-cfd2-4c97-b896-e422c45e5fc6)


Fail2Ban отобразил перебор пароля 

![Снимок54](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/933c0527-2478-4102-bf88-04705e0b9321)


После включения  секции SSH etc/fail2ban/jail.conf

![Снимок56](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/79394c21-34d2-421a-8ffe-00957075fb97)

Suricata как же показывает, что 22 порт сканируется

![Снимок57](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/f3a3ec5f-183f-4a8a-80e4-09d769709a73)

Fail2Ban заблокировал атаку из за привышения попыток подключения.

![Снимок 57](https://github.com/AlexanderSchelokov/Network-protection-hw/assets/121572590/b25aee26-afd7-4add-aed1-0566426140a9)





