# Мониторинг состояния кластера и хостов

Вы можете отслеживать состояние кластера {{ mrd-name }} и отдельных его хостов с помощью инструментов мониторинга в консоли управления. Эти инструменты предоставляют диагностическую информацию в виде графиков.

## Мониторинг состояния кластера {#monitoring-cluster}

Для просмотра детальной информации о состоянии кластера {{ mrd-name }}:

1. Перейдите на страницу каталога и выберите сервис **{{ mrd-name }}**.
1. Нажмите на имя нужного кластера и выберите вкладку **Мониторинг**.

На странице появятся следующие графики:

* **Connected Clients, [count]** — количество открытых соединений для каждого хоста кластера.

    Если кластер [шардированный](../concepts/sharding.md) или использует [репликацию](../concepts/replication.md), часть соединений будет использована для обмена данными между хостами кластера.
    Если при подключении к кластеру возникают ошибки, возможно, неактивные приложения держат соединения открытыми слишком долго. В этом случае измените значение параметра [Timeout](../concepts/settings-list.md#settings-timeout) в [настройках СУБД](../operations/update.md#change-redis-config).

* **Command Processed** — среднее количество запросов, обработанных каждым хостом кластера.

* **Is Alive** — показывает доступность кластера в виде суммы состояний его хостов.

    Каждый хост в состоянии **Alive** увеличивает общую доступность на 1. При выходе из строя одного из хостов общая доступность уменьшается на 1.
    Для повышения доступности кластера вы можете [добавить в него хосты](hosts.md#add).

* **Outer memory limit** — показывает объем всей памяти, доступной для использования на хостах:

    * **memory_limit** — объем памяти, выделенной каждому хосту;
    * **used_memory_rss** — использование памяти процессами {{ RD }}.

    При приближении значения параметра **used_memory_rss** к значению параметра **memory_limit** процесс {{ RD }} может быть принудительно завершен операционной системой. Чтобы избежать этого:
    * измените логику работы приложения таким образом, чтобы снизить объем данных, хранимых в {{ RD }};
    * измените в [настройках {{ RD }}](./update.md#change-redis-config) значение параметра [Maxmemory policy](../concepts/settings-list.md#settings-maxmemory-policy), отвечающего за политику управления памятью при ее дефиците;
    * [повысьте класс хоста](./update.md#change-resource-preset).

* **Inner memory limit** — показывает объем памяти, доступной для использования процессами {{ RD }}:

    * **maxmemory** — максимальный объем памяти, доступной для использования {{ RD }};
    * **used_memory** — фактическое использованием памяти на хосте.

    Если значение параметра **used_memory** достигнет значения параметра **maxmemory**, {{ RD }} применит режим управления памятью, установленный настройкой [Maxmemory policy](../concepts/settings-list.md#settings-maxmemory-policy) при попытке вставить новые записи.

    {% note info %}

    Значение параметра **maxmemory** для хостов Redis устанавливается в размере 75% от объема доступной памяти. Например, для хоста с 8 ГБ памяти значение **maxmemory** будет 6 ГБ.

    {% endnote %}

* **Is Master** — показывает, какой хост и как долго является мастером.

    Если включено [шардирование](../concepts/sharding.md), на графике будут отображаться данные о хостах-мастерах в каждом шарде.

* **Redis-server OOM kills** — количество прерываний процессов {{ RD }} из-за нехватки оперативной памяти (_OOM_ — out-of-memory).

    Чтобы сократить количество прерываний:
    * измените логику работы приложения таким образом, чтобы снизить объем данных, хранимых в {{ RD }};
    * измените в [настройках СУБД](./update.md#change-redis-config) значение параметра [Maxmemory policy](../concepts/settings-list.md#settings-maxmemory-policy), отвечающего за политику управления оперативной памятью при ее дефиците;
    * [повысьте класс хоста](./update.md#change-resource-preset).

* **Redis Used Memory on Masters** — использование памяти на хостах-мастерах:

    * **db_hashtable_overhead** — для хранения хеш-таблиц всех баз данных;
    * **used_memory_scripts** — для хранения и работы [скриптов](https://redis.io/commands/script-load);
    * **mem_aof_buffer** — для буфера [AOF](../concepts/replication.md#setting-appendonly);
    * **mem_clients_normal** — для обслуживания внешних подключений;
    * **mem_clients_slaves** — для обслуживания подключений репликации;
    * **mem_replication_backlog** — для циклического буфера репликации;
    * **used_memory_startup** — для процессов {{ RD }} при запуске (например, после перезагрузки кластера);
    * **used_memory_dataset** — для хранения данных.

* **Replication Lag** — отставание реплики от мастера.

    Ненулевое значение говорит о долгих запросах на реплике или ее перегруженности.

    Подробнее читайте в разделе [{#T}](../concepts/replication.md).

* **DB keys** — количество ключей, хранящихся во всех базах данных кластера.

* **Redis Used Memory on Replicas** — использование оперативной памяти на хостах-репликах:

    * **db_hashtable_overhead** — для хранения хеш-таблиц всех баз данных;
    * **used_memory_scripts** — для хранения и работы [скриптов](https://redis.io/commands/script-load);
    * **mem_aof_buffer** — для буфера [AOF](../concepts/replication.md#setting-appendonly);
    * **mem_clients_normal** — для обслуживания внешних подключений;
    * **mem_clients_slaves** — для обслуживания подключений репликации;
    * **mem_replication_backlog** — для циклического буфера репликации;
    * **used_memory_startup** — для процессов {{ RD }} при запуске (например, после перезагрузки кластера);
    * **used_memory_dataset** — для хранения данных.

* **Replication buffer size** — размер буфера [репликации](../concepts/replication.md#replication):

    * **repl_backlog_size** — максимальный объем памяти, доступный под буфер репликации;
    * **repl_backlog_histlen** — текущий объем памяти, занятый данными в буфере репликации.

    Когда память в циклическом буфере будет исчерпана, запустится процесс полной репликации. Это снизит производительность кластера, т. к. при полной репликации значительно возрастает использование оперативной памяти и нагрузка на CPU и сеть.

* **Evicted keys** — количество ключей, удаленных из памяти при вставке новых данных.

    По умолчанию для управления памятью используется политика **noeviction** — не удалять ключи, вернуть ошибку, если для вставки новых данных недостаточно памяти. Чтобы использовать другую политику управления памятью, измените значение параметра [Maxmemory policy](../concepts/settings-list.md#settings-maxmemory-policy) в [настройках {{ RD }}](./update.md#change-redis-config).

* **Client recent max output buffer size** — использование памяти для обслуживания клиентских подключений, выполняющих чтение данных:

    * **soft_limit** — мягкий лимит использования памяти;
    * **hard_limit** — жесткий лимит использования памяти;
    * **buffer** — текущий размер данных в буфере.

    Если значение параметра **buffer** возрастет до **soft_limit**, кластер в течение нескольких секунд будет ожидать его снижения. Если значение параметра **buffer** не уменьшится, подключение будет закрыто.
    Если значение параметра **buffer** достигнет значения параметра **hard_limit**, соединение будет закрыто сразу же.

* **Copy-on-write allocation** — потребление памяти процессами {{ RD }} при использовании механизма [COW (Copy-on-write)](https://ru.wikipedia.org/wiki/Копирование_при_записи).

    На графике показаны последние измеренные {{ RD }} значения параметров:

    * **module_fork_last_cow_size** — количество данных, скопированных при вызове `fork()` с использованием механизма COW.
    * **aof_last_cow_size** — количество данных, скопированных при создании AOF-файла.
    * **rdb_last_cow_size** — количество данных, скопированных при создании RDB-файла.

    Подробнее см. в разделе [{#T}](../concepts/backup.md).

* **Cache Hit Rate** — процент попаданий в кеш на каждом хосте.

    Значения, близкие к 1, говорят об эффективном использовании кластера в качестве кеширующего сервера. Если процент попаданий в кеш ближе к 0, возможно, следует изменить логику работы приложения, время жизни ключей или [политику управления оперативной памятью](../concepts/settings-list.md#settings-maxmemory-policy) при ее дефиците.

* **Client recent max input buffer size** — использование памяти для обслуживания клиентских подключений, выполняющих запись данных.

## Мониторинг состояния хостов {#monitoring-hosts}

Для просмотра детальной информации о состоянии отдельных хостов {{ mrd-name }}:

1. Перейдите на страницу каталога и выберите сервис **{{mrd-name }}**.
1. Нажмите на имя нужного кластера, затем выберите вкладку **Хосты**.
1. Перейдите на страницу **Мониторинги**.
1. Выберите нужный хост из выпадающего списка.

На этой странице выводятся графики, показывающие нагрузку на отдельный хост кластера:

* **CPU** — загрузка процессорных ядер. При повышении нагрузки значение **Idle** уменьшается.

* **Memory** — использование оперативной памяти (RAM) в байтах. При высоких нагрузках значение параметра **Free** уменьшается, а **Used** и других растет.

* **Disk Bytes** — средний объем данных, записанных в хранилище и прочитанных из него (в байтах).

* **Disk IOPS** — среднее количество операций ввода-вывода в хранилище.

* **Network Bytes** — средний объем данных, отправленных в сеть и полученных из нее (в байтах).

* **Network Packets** — среднее количество пакетов, отправленных в сеть и полученных из нее.

На графиках **Disk bytes** и **Disk IOPS** характеристика **Read** растет при активном чтении из базы данных, а **Write** — при записи в нее.

Для хостов с ролью **Replica** нормально преобладание **Received** над **Sent** на графиках **Network Bytes** и **Network Packets**.