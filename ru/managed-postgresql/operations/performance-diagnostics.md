# Диагностика производительности

{{ mpg-name }} предоставляет встроенный инструмент для диагностики производительности кластера СУБД. Этот инструмент помогает анализировать метрики производительности {{ PG }} для [сессий](#get-sessions) и [запросов](#get-queries).

## Активировать сбор статистики {#activate-stats-collector}

Чтобы воспользоваться инструментом диагностики, включите опцию **Сбор статистики** при [создании кластера](cluster-create.md) или [изменении его настроек](update.md#change-additional-settings) (по умолчанию опция отключена). 

При необходимости задайте интервалы сбора статистики для сессий и запросов.

## Получить информацию о сессиях {#get-sessions}

Для сессий доступна история запросов, выполненных в рамках сессии, а также статистика по сессиям:
- График количества сессий для выбранного среза данных. Можно скрыть или показать отдельные категории на графике, нажав на имя категории в легенде графика.
- Таблица со статистикой по сессиям и запросам.

Подробнее про отображаемые сведения см. [в документации {{ PG }}](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW).

{% list tabs %}

- Статистика по сессиям

  1. В консоли управления перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Диагностика производительности**.
  1. Выберите вкладку **Сессии → Статистика**.
  1. Задайте интересующий интервал времени, при необходимости настройте фильтры.
  1. Выберите нужный [срез данных](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW) из списка.

- История запросов

  1. В консоли управления перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Диагностика производительности**.
  1. Выберите вкладку **Сессии → История**.
  1. Задайте интересующий интервал времени, при необходимости настройте фильтры.
  
{% endlist %}

## Получить информацию о запросах {#get-queries}

Для запросов доступна статистика, а также сравнение статистических данных за два временных интервала.

Например, пусть в первом интервале было выполнено 10 запросов `SELECT * FROM cities`, а во втором — 20. Тогда при сравнении статистических данных разница по метрике «количество запросов» (столбец `Calls` в таблице) будет равняться `+100%`.

Подробнее про отображаемые сведения см. [в документации {{ PG }}](https://www.postgresql.org/docs/current/pgstatstatements.html#id-1.11.7.38.6).

{% list tabs %}

- Статистика запросов

  1. В консоли управления перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Диагностика производительности**.
  1. Выберите вкладку **Запросы → Интервал**.
  1. Задайте интересующий интервал времени, при необходимости настройте фильтры.

- Сравнение запросов

  1. В консоли управления перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Диагностика производительности**.
  1. Выберите вкладку **Запросы → 2 Интервала**.
  1. Задайте интересующие интервалы времени, при необходимости настройте фильтры.

{% endlist %}

## Получить информацию о плане выполнения запроса {#auto-explain-enable}

[Модуль `auto_explain`](https://www.postgresql.org/docs/current/auto-explain.html) позволяет логировать план выполнения медленных запросов автоматически, обходясь без [команды `EXPLAIN`](https://www.postgresql.org/docs/current/sql-explain.html). Это полезно для отслеживания неоптимизированных запросов. Логирование выполняется в общий лог {{ PG }}.

Чтобы включить логирование запросов, [измените настройки СУБД](update.md#change-postgresql-config):

1. В поле **Shared preload libraries** выберите значение `auto_explain`.
1. Включите настройку **Auto explain log analyze**.
1. Задайте настройки модуля `auto_explain`:

    {% note warning %}
    
    Установка значения `0` для настройки **Auto explain log min duration** или включение настройки **Auto explain log timing** могут существенно снизить производительность кластера.
    
    {% endnote %}

    * [**Auto explain log buffers**](../concepts/settings-list.md#setting-auto-explain-log-buffers)
    * [**Auto explain log min duration**](../concepts/settings-list.md#setting-auto-explain-log-min-duration)
    * [**Auto explain log nested statements**](../concepts/settings-list.md#setting-auto-explain-log-nested-statements)
    * [**Auto explain log timing**](../concepts/settings-list.md#setting-auto-explain-log-timing)
    * [**Auto explain log triggers**](../concepts/settings-list.md#setting-auto-explain-log-triggers)
    * [**Auto explain log verbose**](../concepts/settings-list.md#setting-auto-explain-log-verbose)
    * [**Auto explain sample rate**](../concepts/settings-list.md#setting-auto-explain-sample-rate)
