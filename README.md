#  Дипломная работа по профессии «Системный администратор»
#### Выполнил: Важничев Георгий Владимирович
#### Группа: SYS-47

### 1 Заполнение конфигурационного файла terraform `main.tf`meta.yaml .terraformrc

[main.tf](https://github.com/vajnichev/diplom_rab/blob/main/main.tf)
[meta.yaml](https://github.com/vajnichev/diplom_rab/blob/main/meta.yaml)
[.terraformrc](https://github.com/vajnichev/diplom_rab/blob/main/.terraformrc)

### Hеобходимо азвернуть через terraform следующий ресурcы:
##### Сайт. Веб-сервера. Nginx.
- Создать две ВМ в разных зонах, установить на них сервер nginx.
- Создать Target Group, включить в неё две созданные ВМ.
- Создать Backend Group, настроить backends на target group, ранее созданную. Настроить healthcheck на корень (/) и порт 80, протокол HTTP.
- Создать HTTP router. Путь указать — /, backend group — созданную ранее.
- Создать Application load balancer для распределения трафика на веб-сервера, созданные ранее. Указать HTTP router, созданный ранее, задать listener тип auto, порт 80



##### Логи. Elasticsearch. Kibana. Filebeat.
- Cоздать ВМ, развернуть на ней Elasticsearch. Установить Filebeat в ВМ к веб-серверам, настроить на отправку access.log, error.log nginx в Elasticsearch.
- Создать ВМ, развернуть на ней Kibana, сконфигурировать соединение с Elasticsearch.

##### Сеть.
- Развернуть один VPC.
- Сервера web, Elasticsearch поместить в приватные подсети. 
- Сервера Zabbix, Kibana, application load balancer определить в публичную подсеть.
- Настроить Security Groups соответствующих сервисов на входящий трафик только к нужным портам.
- Настроить ВМ с публичным адресом, в которой будет открыт только один порт — ssh. Эта вм будет реализовывать концепцию bastion host.

##### Резервное копирование.
- Создать snapshot дисков всех ВМ. 
- Ограничить время жизни snaphot в неделю. 
- Сами snaphot настроить на ежедневное копирование.

### 2 Проверка развернутых ресурсов в Yandex Cloud.
#### Все ресурсы через terraform развернуты и работают.
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.1.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.2.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.3.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.4.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.5.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.6.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.7.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.8.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.9.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.10.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.11.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.12.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.1.13.png)


##### Сайт. Веб-сервера. Nginx.
  
Устанавливаю сервер nginx на 2 ВМ. Заменяю стандартный файл `index.nginx-debian.html`
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.1.png)

#### Мониторинг. Zabbix. Zabbix-agent.

Разворачиваю Zabbix.
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.2.png)

На каждую ВМ устанавливаю Zabbix Agent, настраиваю агенты на отправление метрик в Zabbix.

##### Логи. Elasticsearch. Kibana. Filebeat.

Разворачиваю на ВМ Elasticsearch.

![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.3.png)

Разворачиваю на другой ВМ Kibana, конфигурирую соединение с Elasticsearch и добавляю параметр `server.publicBaseUrl:
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.4.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.5.png)

Устанавливаю Filebeat в ВМ к веб-серверам, настраиваю на отправку access.log, error.log nginx в Elasticsearch.
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.6.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.2.7.png)

#### Все сервисы через ansible развернуты.

##### Сайт.
Протестирую работу сайта с ip балансировщика.

![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.5.1.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.6.1.png)

##### Мониторинг.
Проверка работы Zabbix. Перехожу на страницу с Zabbix `http://158.160.200.127/zabbix`.
Логин: Admin
Пароль: zabbix
Добавляю сервера.
астраиваю дешборды с отображением метрик, минимальный набор
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.4.1.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.4.2.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.4.3.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.4.4.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.4.5.png)

##### Логи.

захожу в kibana `http://http://158.160.176.157:5601

Создаю Index patterns.
Логи отправляются.

![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.3.1.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.3.2.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.3.3.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.3.4.png)

##### Резервное копирование.

![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.7.1.png)
![png](https://github.com/vajnichev/diplom_rab/blob/main/img/1.7.2.png)

