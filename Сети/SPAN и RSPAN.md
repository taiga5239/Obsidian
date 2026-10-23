[[Сети и системы передачи информации (L2-3)]]

**Зеркалирование трафика** — это технология копирования сетевых пакетов с одного или нескольких портов коммутатора или VLAN и отправки их на специальный порт мониторинга для анализа.
Работа протокола SPAN:
[root@host] *ovs-vsctl set bridge ovs0 mirrors=@m -- --id=@mirror0 get port ens20 -- --id=@ens19 get port ens19 -- --id=@m create mirror name=mymirror select-dst-port=@ens19 select-src-port=@ens19 output-port=@mirror0*

Настройка RSPAN:
[root@host] *ovs-vsctl set bridge ovs0 mirrors=@m -- --id=@m create mirror name=mymirror select-all=true select-vlan=10 output-vlan=15*

SPAN и RSPAN различаются масштабом и способом передачи данных. SPAN работает в пределах одного коммутатора, а RSPAN работает между разными коммутаторами.


