# Миграция базы данных из стороннего кластера {{ GP }} или {{ PG }} в {{ mgp-full-name }} с помощью {{ data-transfer-full-name }}

Вы можете перенести вашу базу данных из стороннего кластера {{ GP }} или {{ PG }} в {{ mgp-name }} с помощью сервиса {{ data-transfer-full-name }}. Этот способ позволяет:

* перенести базу без остановки обслуживания пользователей;
* мигрировать со старых версий {{ GP }} на более новые;
* обойтись без создания промежуточной виртуальной машины или разрешения доступа к вашему кластеру-приемнику {{ mpg-name }} из интернета.

Подробнее см. в разделе [{#T}](../../data-transfer/concepts/use-cases.md).

{% note info %}

{{ data-transfer-name }} позволяет перенести состояние базы-источника на приемник {{ mgp-name }} без поддержания ее в актуальном состоянии (тип трансфера [_{{ dt-type-copy }}_](../../data-transfer/concepts/transfer-lifecycle.md#copy)). Изменения, произошедшие на источнике после завершения трансфера, перенесены не будут.

{% endnote %}

Чтобы перенести базу данных, нужно непосредственно перенести данные, закрыть старую базу данных на запись и переключить нагрузку на кластер БД в {{ yandex-cloud }}.

## Перед началом работы {#before-you-begin}

[Создайте кластер-приемник {{ mgp-name }}](../../managed-greenplum/operations/cluster-create.md), вычислительная мощность и размер хранилища которого соответствуют среде, в которой развернута мигрируемая база данных.

Имя базы в кластере-приемнике должно совпадать с именем базы-источника.

## Перенос данных {#data-transfer}

1. Подготовьте кластер-источник:
    * [{{ GP }}](../../data-transfer/operations/prepare.md#source-gp)
    * [{{ PG }}](../../data-transfer/operations/prepare.md#source-pg)
1. [Подготовьте кластер-приемник](../../data-transfer/operations/prepare.md#target-gp).
1. [Создайте эндпоинт для источника](../../data-transfer/operations/endpoint/index.md#create) со следующими параметрами:

    * **Тип базы данных** — `{{ GP }}` или `{{ PG }}`.
    * **Параметры эндпоинта** → **Настройки подключения** — `Пользовательская инсталляция`.

        Укажите параметры подключения к кластеру-источнику.

1. [Создайте эндпоинт для приемника](../../data-transfer/operations/endpoint/index.md#create) со следующими параметрами:

    * **Тип базы данных** — `{{ GP }}`.
    * **Параметры эндпоинта** → **Настройки подключения** — `Кластер MDB`.

        Укажите идентификатор кластера-приемника.

1. [Создайте трансфер](../../data-transfer/operations/transfer.md#create) типа _{{ dt-type-copy }}_, использующий созданные эндпоинты.
1. [Активируйте трансфер](../../data-transfer/operations/transfer.md#activate).
1. Дождитесь перехода трансфера в статус {{ dt-status-finished }}.

    Подробнее о статусах трансфера см. в разделе [Жизненный цикл трансфера](../../data-transfer/concepts/transfer-lifecycle.md#statuses).

1. Переведите кластер-источник в режим <q>только чтение</q> и переключите нагрузку на кластер-приемник.
1. [Удалите завершившийся трансфер](../../data-transfer/operations/transfer.md#delete).
1. [Удалите эндпоинты](../../data-transfer/operations/endpoint/index.md#delete) для источника и приемника.