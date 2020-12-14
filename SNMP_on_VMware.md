## Настройка SNMP на хостах VMWare ESXi
#### На узле EXSi в консоли  
##### Выполняем команды  
```
esxcli system snmp set --targets=10.1.32.8@161/public
esxcli system snmp set --communities public
```
Местонахождение и контакт (поправить если требуется)  
```
esxcli system snmp set --syslocation "Помещение 325 сервер 26"
esxcli system snmp set --syscontact admin@elavt.spb.ru
```
Старт/Стоп  
```
esxcli system snmp set --enable true
esxcli system snmp set --enable false
```
Тестирование настроек  
```
esxcli system snmp test
```
Текущие параметры  
```
esxcli system snmp get
```
Должно выдать примерно следующее  
```
esxcli system snmp get
Authentication:
Communities: public
Enable: true
Engineid: 00000063000000a100000000
Hwsrc: indications
Largestorage: true
Loglevel: info
Notraps:
Port: 161
Privacy:
Remoteusers:
Syscontact: admin@elavt.spb.ru
Syslocation: Помещение 325 сервер 23
Targets: 10.1.32.8@161 public
Users:
V3targets:
```
На узле zabbix проверяем доступность smnp  
```
snmpwalk -v 2c -c public -O e a.b.c.d
```
### В zabbix настраиваем хост  
Добавить интерфейс для хоста (НазваниеХоста\Узел сети\добавить\  
##### Добавить community (НазваниеХоста\Макросы\добавить)  
```
{$SNMP_COMMUNITY}
public
```
Добавить шаблоны(НазваниеХоста\Шаблоны\добавить)  
Template OS Linux SNMPv2
Template VM VMware Guest
Template VM VMware Hypervisor

