# Домашнее задание к занятию «Защита сети» - `Костюк Денис`

Подготовка к выполнению заданий  
Подготовка защищаемой системы:  
установите Suricata,  
установите Fail2Ban.  
Подготовка системы злоумышленника: установите nmap и thc-hydra либо скачайте и установите Kali linux.  
Обе системы должны находится в одной подсети.  

## Задание 1
Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:  

sudo nmap -sA < ip-адрес >  
sudo nmap -sT < ip-адрес >  
sudo nmap -sS < ip-адрес >  
sudo nmap -sV < ip-адрес >  

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.  

В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.  

### Ответ 1

На входе имеем 2 хоста:  
- атакующий Kali Linux = 192.168.0.147  
- Debian = 192.168.0.182  

![image](https://github.com/denniskostyuk/netprotection/blob/main/task-11.png)

sudo nmap -sA 192.168.0.182  
Fail2Ban = нет логов  
Suricata = нет логов  

sudo nmap -sT 192.168.0.182
Fail2Ban = нет логов  
Suricata сигнализирует о потенциально опасном траффике:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-12.png)  

sudo nmap -sS 192.168.0.182  
Fail2Ban = нет логов  
Suricata сигнализирует о потенциально опасном траффике:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-13.png)  

sudo nmap -sV 192.168.0.182  
Fail2Ban = нет логов  
Suricata сигнализирует о потенциально опасном траффике:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-14.png)  



## Задание 2
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

### Ответ 2

#### Fail2Ban включен:  

hydra не может подобрать пароль:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-21.png)   

Лог Fail2Ban = в какой-то момент (полсе нескольких безуспешных попыток) IP атакующего хоста блокируется:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-22.png)   

Лог Suricata фиксирует попытки подключения по SSH:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-23.png)   

#### Fail2Ban выключен:  

hydra благополучно подбирает пароль:  
![image](https://github.com/denniskostyuk/netprotection/blob/main/task-24.png)   
