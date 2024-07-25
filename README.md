
**Задание 1**

Компания ОАО «XXX – век» планирует увеличить пропускную способность канала связи. К вам поступила заявка: настроить статическую агрегацию для сети.
Необходимо настроить и проверить доступность компьютеров, проверить настройки через пространство команд show.


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/ea869eabda38a5c6f0fefa80165a6cded1d3c59a/IMG/1.png)

Пришлите pkt с полученным проектом


**Решение 1**

 Настроим свитч SW0
 
 
 ```
SW0(config)#int ran fa0/1-4
SW0(config-if-range)#sh
SW0(config-if-range)#channel-group 1 mode on
SW0(config-if-range)#exi
SW0(config)#int po1
SW0(config-if)#switchport mode trunk
SW0(config-if)#exi
SW0(config)#exi
SW0#wr
SW0#sh etherchannel summary
```

Проверим конфигурацию


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/4.jpg)




Настроим SW1

```

Switch(config)#hostname SW1
SW1(config)#int ran fa0/1-4
SW1(config-if-range)#channel-group 1 mode on
SW1(config-if-range)#exi
SW1(config)#int po1
SW1(config-if)#switchport mode trunk
SW1(config-if)#exi
SW1(config)#exi
SW1#wr

```


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/5.jpg)


Линки пошли - теперь проверим коннект между рабочими станциями 


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/69201cb70fab2915c3582d083324d3718daeedf4/IMG/6.jpg)


[PKT](https://github.com/mezhibo/loadbalancer-protocols/blob/873f66dfec178259b244d8e8c3a2101f2c7627f1/IMG/balanc1.pkt)





**Задание 2**

Компания ОАО «XXX – век» после модернизации и расширения сети задумалась над резервированием существующего агрегированного канала.
К вам поступила заявка: настроить динамическую агрегацию каналов для сети, так как есть риск разрыва соединения на основных интерфейсах.

Необходимо:

 - настроить сетевое оборудование и проверить доступность компьютеров,

 - проверить настройки через пространство команд show,

 - проверить работу после отключения основных интерфейсов.

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/e5b147208c4a263c5e8f424bc9a0c5c3fe0159e0/IMG/2.png)

Пришлите pkt с полученным проектом


**Решение 2**

Настроим SW0

```
SW0(config)#int range fa0/1-8
SW0(config-if-range)#sh

SW0(config-if-range)#int range fa0/1-4
SW0(config-if-range)#channel-group 1 mode activ
SW0(config-if-range)#exi
SW0(config)#int po1
SW0(config-if)#switchport mode trunk

SW0(config-if)#int range fa0/5-8
SW0(config-if-range)#channel-group 2 mode activ
SW0(config-if-range)#exi
SW0(config)#int po2
SW0(config-if)#switchport mode trunk

SW0(config-if)#exi
SW0(config)#exi
SW0#wr
```

Проверяем

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/7.jpg)


Настроим SW1

```
SW1(config)#int range fa0/1-4
SW1(config-if-range)#channel-group 1 mode activ
SW1(config-if-range)#exi
SW1(config)#int po1
SW1(config-if)#switchport mode trunk

SW1(config-if)#int range fa0/5-8
SW1(config-if-range)#channel-group 3 mode activ
SW1(config-if-range)#exi
SW1(config)#int po3
SW1(config-if)#switchport mode trunk

SW1(config-if)#exi
SW1(config)#exi
SW1#wr
```

Проверим


![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/8.jpg)


Настроим SW2

```
SW2(config)#int range fa0/1-4
SW2(config-if-range)#channel-group 2 mode activ
SW2(config-if-range)#exi
SW2(config)#int po2
SW2(config-if)#switchport mode trunk

SW2(config-if)#int range fa0/5-8
SW2(config-if-range)#channel-group 4 mode activ
SW2(config-if-range)#exi
SW2(config)#int po4
SW2(config-if)#switchport mode trunk

SW2(config-if)#exi
SW2(config)#exi
SW2#wr
```

Проверим

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/9.jpg)


И настроим SW3

```
SW3(config)#int range fa0/1-8
SW3(config-if-range)#sh

SW3(config-if-range)#int range fa0/1-4
SW3(config-if-range)#channel-group 3 mode activ
SW3(config-if-range)#exi
SW3(config)#int po3
SW3(config-if)#switchport mode trunk

SW3(config-if)#int range fa0/5-8
SW3(config-if-range)#channel-group 4 mode activ
SW3(config-if-range)#exi
SW3(config)#int po4
SW3(config-if)#switchport mode trunk

SW3(config-if)#exi
SW3(config)#exi
SW3#wr
```

Проверяем 

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/10.jpg)


Запустим порты на всех свичах

```
SW..(config)#int range fa0/1-8
SW..(config-if-range)#no sh
```

Линки горят зеленым. Теперь проверим пинг между компьютерами

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/11.jpg)


А теперь возьмем порвем весь коннект между SW2 и SW3, и еще оторвем везде по одному каналу, ипроверим как работает наше резервирование каналов.

И вилим что раннее заблокированные порты на SW3 запустили линк благодаря STP.

Теперь проверим есть ли пинг, и видим что наша отказоустойчивость работает.

![alt text](https://github.com/mezhibo/loadbalancer-protocols/blob/4927195eecc91e631b2b909097b6398b86af8986/IMG/12.jpg)


[PKT](https://github.com/mezhibo/loadbalancer-protocols/blob/fa051d0ca624dddc8e826e65214328590e592695/IMG/balanc2.pkt)







