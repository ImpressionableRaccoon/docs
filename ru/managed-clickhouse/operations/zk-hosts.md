# Включение отказоустойчивости для кластера

Кластер {{ mch-name }} с одним хостом не отказоустойчив и не обеспечивает репликацию данных. Чтобы кластер был более надежным, вы можете добавить дополнительные хосты {{ CH }}, но для управления ими сначала необходимо добавить к кластеру хосты {{ ZK }}.

{% note warning %}

Вы можете добавить к кластеру не больше и не меньше 3 хостов {{ ZK }}.

{% endnote %}


## Добавить хосты {{ ZK }} {#add-zk}

Для добавляемых хостов {{ ZK }} вы можете указать вычислительную мощность ([класс хоста](../concepts/instance-types.md)) и подсети, в которых должны размещаться хосты.

По умолчанию для хостов {{ ZK }} задаются следующие характеристики:

* класс хоста `b2.medium`;
* объем диска 10 ГБ;
* [тип хранилища](../concepts/storage.md) — быстрое сетевое.

Чтобы добавить хосты {{ ZK }}:

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ mch-name }}**.

  1. Нажмите на имя нужного кластера, затем выберите вкладку **Хосты**.

  1. Нажмите кнопку **Добавить хосты {{ ZK }}**.


- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды CLI для добавления хостов {{ ZK }}:

     ```
     $ yc managed-clickhouse cluster add-zookeeper --help
     ```

  1. Запустите операцию с характеристиками хостов по умолчанию:

     ```bash
     $ yc managed-clickhouse cluster add-zookeeper clickhouse417 \
                             --host zone-id=ru-central1-c,subnet-name=default-c \
                             --host zone-id=ru-central1-a,subnet-name=default-a \
                             --host zone-id=ru-central1-b,subnet-name=default-b
     ```

     Если в сети, в которой расположен кластер, ровно 3 подсети, по одной в каждой зоне доступности, то явно указывать подсети для хостов необязательно: {{ mch-name }} автоматически распределит хосты по этим подсетям.

     Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

  Добавить в кластер хосты {{ ZK }} можно с помощью метода [addZookeeper](../api-ref/Cluster/addZookeeper.md).

{% endlist %}