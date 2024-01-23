# Сети в Linux

Настройка сетей в Linux на виртуальных машинах.

## Часть 1. Инструмент ipcalc

### Часть 1.1 Инструмент ipcalc

- Адрес сети *192.167.38.54/13* \
  ![part1](./images/part1/network.png) 
- Перевод маски *11111111.11111111.11111111.11110000* - */28*, обычная *255.255.255.240*

### Часть 1.2 localhost

- Адреса *194.34.23.100*, *128.0.0.1* - не localhost, а *127.0.0.2*, *127.1.0.1* - localhost

### Часть 1.3 Диапазоны и сегменты сетей

- Адреса *10.0.0.45*, *192.168.4.2*, *172.20.250.4*, *172.16.255.255*, *10.10.10.10* - частные, а
*134.43.0.2*, *172.20.250.4*, *172.20.250.4*, *172.0.2.1*, *192.169.168.1* - публичные. \
  ![part1](./images/part1/private.png) 

- Подходит всё что между HostMin и HostMax \
  ![part1](./images/part1/hostminmax.png)

- *10.10.0.2*, *10.10.10.10*, *10.10.1.255* - подходят, а *10.0.0.1* и *10.10.100.1* не подходят

## Часть 2. Статическая маршрутизация между двумя машинами

- Текущий интерфейс у машины ws1 \
  ![part2](./images/part2/ipa_output_ws1.png)

- Текущий интерфейс у машины ws2 \
  ![part2](./images/part2/ipa_output_ws2.png)

- Новый сетевой интерфейс для машины ws1 \
  ![part2](./images/part2/newcfg_ws1.png)

- Новый сетевой интерфейс для машины ws2 \
  ![part2](./images/part2/newcfg_ws2.png)

- Добавление статического маршрута вручную и пинги ws1 \
  ![part2](./images/part2/addroute_and_check_ws1.png)

- Добавление статического маршрута вручную и пинги ws2 \
  ![part2](./images/part2/addroute_and_check_ws2.png)

- Конфиг для добавления статического машрута вручную и пинги ws1\
  ![part2](./images/part2/permcfg_ws1.png) \
  ![part2](./images/part2/addpermroute_and_check_ws1.png)

- Конфиг для добавления статического машрута вручную и пинги ws2 \
  ![part2](./images/part2/permcfg_ws2.png) \
  ![part2](./images/part2/addpermroute_and_check_ws2.png)

## Часть 3. Утилита **iperf3**

- Запуск программы iperf3 на ws1 \
  ![part3](./images/part3/setup_iperf3.png)

- Подключение с ws2 на w1 и замер скорости. \
  8 Mbps = 1 MB/s. 100 MB/s = 800000 Kbps. 1 Gbps = 1000 Mbps. \
  ![part3](./images/part3/connect_iperf3.png)

## Часть 4. Сетевой экран

- Фаерволл ws1 \
  ![part4](./images/part4/ws1_firewall.png)
- Фаерволл ws2 \
  ![part4](./images/part4/w2_firewall.png)
- Запуск файервола ws1 \
  ![part4](./images/part4/w1_running_firewall.png)
- Запуск файервола ws2 \
  ![part4](./images/part4/w2_running_firewall.png)

  Разница подходов в том, что применяется первое правило, значит ws1 нельзя пингануть, а ws2 можно.

- Пингуем ws1 с ws2 \
  ![part4](./images/part4/ws2_ping.png)

- Пингуем ws2 с ws1 \
  ![part4](./images/part4/ws1_ping.png)

  Видим что ws1 не пингуется, как и было написано выше.

- Показываем командой nmap что хост ws1 запущен \
  ![part4](./images/part4/proof_nmap.png)

## Часть 5. Статическая маршрутизация сети

Создаем 5 виртуальных машин (я использовал клонирование для удобства)


#### 5.1. Настройка адресов машин

- Конфиг ws11 \
  ![part5](./images/part5/ws11_config.png)

- Конфиг ws21 \
  ![part5](./images/part5/ws21_config.png)

- Конфиг ws22 \
  ![part5](./images/part5/ws22_config.png)

- Конфиг r1 \
  ![part5](./images/part5/r1_config.png)

- Конфиг r2 \
  ![part5](./images/part5/r2_config.png)

- Вызов ip -4 a для ws11 \
  ![part5](./images/part5/ip4a_ws11.png)

- Вызов ip -4 a для ws21 \
  ![part5](./images/part5/ip4a_ws21.png)

- Вызов ip -4 a для ws22 \
  ![part5](./images/part5/ip4a_ws22.png)

- Вызов ip -4 a для r1 \
  ![part5](./images/part5/ip4a_r1.png)

- Вызов ip -4 a для r2 \
  ![part5](./images/part5/ip4a_r2.png)

#### 5.2. Включение переадресации IP-адресов.

- Включение временной переадресации на r1 \
  ![part5](./images/part5/r1_forward.png)

- Включение временной переадресации на r2 \
  ![part5](./images/part5/r2_forward.png)

- Включение постоянной переадресации на r1 \
  ![part5](./images/part5/r1_sysctlfile.png)

- Включение постоянной переадресации на r2 \
  ![part5](./images/part5/r2_sysctlfile.png)

#### 5.3. Установка маршрута по-умолчанию

- Шлюз по умолчанию для ws11 \
  ![part5](./images/part5/ws11_gatewatadd.png)

- Шлюз по умолчанию для ws21 \
  ![part5](./images/part5/ws21_gatewayadd.png)

- Шлюз по умолчанию для ws22 \
  ![part5](./images/part5/ws22_gatewayadd.png)

- Доказательство что шлюз добавился для ws11 \
  ![part5](./images/part5/ws11_gateway_ipr.png)

- Докатательство что шлюз добавился для ws21 \
  ![part5](./images/part5/ws21_gateway_ipr.png)

- Докатательство что шлюз добавился для ws22 \
  ![part5](./images/part5/ws22_gateway_ipr.png)

- Пинг r2 c ws11 \
  ![part5](./images/part5/ping_r2_from_ws11.png)

- tcpdump с r2\
  ![part5](./images/part5/ping_r2_from_ws11.png)

#### 5.4. Добавление статических маршрутов

- Добавление статического маршрута r1 \
  ![part5](./images/part5/r1_addstaticroutes.png)

- Добавление статического маршрута r2 \
  ![part5](./images/part5/r2_addstaticroutes.png)

- Доказательство что маршрут добавился для r1 \
  ![part5](./images/part5/r1_addstaticroutes_ipr.png)

- Доказательство что маршрут добавился для r2 \
  ![part5](./images/part5/r2_addstaticroutes_ipr.png)

- Сравнение машрутов для двух сетей ws11. Маршруты разные потому что приоритет имеет таблица маршрутизации, а потом используется маршрут по умолчанию \
  ![part5](./images/part5/ipr_compareroutes.png)

#### 5.5. Построение списка маршрутизаторов

- Вызов команды `traceroute 10.20.0.10` \
  ![part5](./images/part5/traceroute.png)

- Вывод команды `tcpdump -tnv -i enp0s8` на r1 \
  ![part5](./images/part5/tcpdump_traceroute.png)

Команда сначала посылает один пакет с ttl 1, затем ttl 2 и т.д. пока не достигнет целевого узла. Каждый раз когда пакет проходит через маршрутизатор ttl уменьшается на один. В конце концов пакет доходит до искомого и  выводится время между отправкой и ответом пакета. Предыдщие же пакеты не доходят и показывают айпи маршрутизаторов (они посылают ответ и говорят что отправить пакет нельзя)

#### 5.6. Использование протокола **ICMP** при маршрутизации

- Вывод команды `tcpdump -n -i enp0s8 icmp` на r1 \
  ![part5](./images/part5/tcpdump_nonexistentaddress.png)

- Пингуем с ws11 несуществующий IP \
  ![part5](./images/part5/ping_nonexistent_address.png)

## Часть 6. Динамическая настройка IP с помощью **DHCP**

- DHCP для r2 \
  ![part6](./images/part6/r2_dhcp.png)

- Прописываем DNS сервер для r2 \
  ![part6](./images/part6/r2_resolv.png)

- Перезапуск службы DHCP r2 \
  ![part6](./images/part6/r2_restart_dhcp.png)

- Показываем что ws21 получила адрес \
  ![part6](./images/part6/ws21_dhcp_ipa.png)

- Пингуем ws22 с ws21 \
  ![part6](./images/part6/ws21_dhcp_ping_ws22.png)

- Даём MAC адресс ws11 \
  ![part6](./images/part6/ws11_macaddress.png)

- Аналогичная настройка r1 только с привязкой по MAC \
  ![part6](./images/part6/r1_dhcp.png) \
  ![part6](./images/part6/r1_resolv.png) \
  ![part6](./images/part6/r1_restart.png)

- Показываем что ws11 получила адрес \
  ![part6](./images/part6/ws11_ipa.png)

- Запрашиваем обновление для ws21. Использовалось просто `sudo dhcclient` \
  ![part6](./images/part6/ws21_get_ip.png)
- Адрес до обновления \
  ![part6](./images/part6/ws21_dhcp_ipa.png)

## Часть 7. **NAT**

- Делаем Апач общедоступным на ws22 и r1 \
  ![part7](./images/part7/ws22_apache.png) \
  ![part7](./images/part7/r1_apache.png)

- Запускаем апач на ws22 и r1 \
  ![part7](./images/part7/ws22_start_apache.png) \
  ![part7](./images/part7/r1_start_apache.png)

- Создаём фаервол на r2 и запускаем \
  ![part7](./images/part7/r2_firewall.png) \
  ![part7](./images/part7/r2_launch_firewall.png)

- Пингуем ws22 c r1 \
  ![part7](./images/part7/ws22_ping_r1_cant.png)

- Разрешаем маршрутизацию ICMP пакетов и запускаем \
  ![part7](./images/part7/r2_allow_icmp_firewall.png) \
  ![part7](./images/part7/r2_launch_firewall.png)

- Пингуем ws22 с r1 \
  ![part7](./images/part7/ws22_ping_r1_can.png)

- Включаем DNAT и SNAT на r2 и запускаем \
  ![part7](./images/part7/r2_dnat_snat_firewall.png)

- Подключаемся с ws22 на r1 \
  ![part7](./images/part7/connect_to_r1.png)

- Подключаемся с r1 на ws22 \
  ![part7](./images/part7/connect_to_ws22.png)

## Часть 8. Знакомство с **SSH Tunnels**

- Фаервол на r2 \
  ![part7](./images/part8/r2_firewall.png)

- Запуск Апач на локалхост ws22 \
  ![part7](./images/part8/ws22_apache2_conf.png)

- Запускаем Local TCP forwarding и подключаемся \
  ![part7](./images/part8/ws21_to_ws22.png) \
  ![part7](./images/part8/ws21_telnet.png)

- Запускаем Remote TCP forwarding и подключаемся \
  ![part7](./images/part8/ws22_reversepng.png) \
  ![part7](./images/part8/ws11_to_ws22_reverse.png)