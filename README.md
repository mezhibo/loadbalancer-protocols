![alt text](https://github.com/mezhibo/Vvedenie-terraform/blob/3e854dd0d95703c0b43a8573959d44d12580f173/IMG/4.jpg)



**Задание 1**

Компания ОАО «XXX – век» планирует увеличить пропускную способность канала связи. К вам поступила заявка: настроить статическую агрегацию для сети.
Необходимо настроить и проверить доступность компьютеров, проверить настройки через пространство команд show.


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/ea869eabda38a5c6f0fefa80165a6cded1d3c59a/IMG/1.png)

Пришлите pkt с полученным проектом


**Решение 1**

 Настроим свитч SW0
 
   '''

SW0(config)#int ran fa0/1-4
SW0(config-if-range)#sh
SW0(config-if-range)#channel-group 1 mode on
SW0(config-if-range)#exi
SW0(config)#int po1
SW0(config-if)#switchport mode trunk
SW0(config-if)#exi
SW0(config)#exi
SW0#wr
SW0#show etherchannel summary

'''

Проверим конфигурацию


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/4.jpg)




Настроим SW1

'''

Switch(config)#hostname SW1
SW1(config)#int ran fa0/1-4
SW1(config-if-range)#channel-group 1 mode on
SW1(config-if-range)#exi
SW1(config)#int po1
SW1(config-if)#switchport mode trunk
SW1(config-if)#exi
SW1(config)#exi
SW1#wr

'''


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/5.jpg)


Вернемся и запустим порты на SW0

'''
SW0(config)#int ran fa0/1-4
SW0(config-if-range)#no sh

'''


Линки пошли - теперь проверим коннект между рабочими станциями 


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/6.jpg)


[PKT](https://github.com/mezhibo/loadbalancer-protocols/blob/873f66dfec178259b244d8e8c3a2101f2c7627f1/IMG/balanc1.pkt)

