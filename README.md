# Data Engineering Zoomcamp 2026 — Module 2: Workflow Orchestration

Этот репозиторий содержит решение домашнего задания для второго модуля курса **Data Engineering Zoomcamp 2026**. Основная цель модуля — освоение инструментов оркестрации данных на примере **Kestra**.

## Содержание проекта

* `flows/`: YAML-конфигурации потоков Kestra.
    * `04_postgres_taxi.yaml`: Поток для загрузки данных NYC Taxi в локальный Postgres.
    * `05_postgres_taxi_scheduled.yaml`: Поток с настроенным расписанием и таймзоной.
* `docker-compose.yml`: Конфигурация среды (Kestra + Postgres).

## Инструкция по запуску

1.  **Запуск среды:**
    Убедитесь, что у вас установлен Docker, и выполните команду в корневой папке проекта:
    ```bash
    docker compose up -d
    ```

2.  **Доступ к интерфейсу:**
    * **Kestra UI:** [http://localhost:8080](http://localhost:8080)
    * **pgAdmin:** [http://localhost:8085](http://localhost:8085) (если настроен в docker-compose)

3.  **Импорт потоков:**
    Скопируйте содержимое файлов из папки `flows/` и создайте новые потоки (Flows) в интерфейсе Kestra.

## Ответы на вопросы домашнего задания (Quiz)

### Вопрос 1: Размер распакованного файла `yellow_tripdata_2020-12.csv`
**Ответ:** `128.3 MiB`  
*Как получено:* Проверка логов выполнения задачи `extract` в Kestra после запуска потока за декабрь 2020 года.

### Вопрос 2: Значение переменной `file` при `taxi=green`, `year=2020`, `month=04`
**Ответ:** `green_tripdata_2020-04.csv`  
*Как получено:* Анализ выражения `{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv`.

### Вопрос 3: Количество строк для Yellow Taxi за весь 2020 год
**Ответ:** `24,648,499`  
*Как получено:* Выполнение Backfill для Yellow Taxi (2020-01-01 по 2020-12-31) и SQL-запрос:
```sql
SELECT count(*) FROM public.yellow_tripdata WHERE filename LIKE 'yellow_tripdata_2020%';
```

Вопрос 4: Количество строк для Green Taxi за весь 2020 год
Ответ: 1,734,051

Как получено: Выполнение Backfill для Green Taxi за 2020 год и SQL-запрос:
```sql
SELECT count(*) FROM public.green_tripdata WHERE filename LIKE 'green_tripdata_2020%';
```

Вопрос 5: Количество строк для Yellow Taxi за март 2021 года
Ответ: 1,925,152

Как получено: Ручной запуск потока для yellow, 2021, 03 и SQL-запрос:
```sql
SELECT count(*) FROM public.yellow_tripdata WHERE filename LIKE 'yellow_tripdata_2021-03%';
```

Вопрос 6: Настройка таймзоны в Schedule trigger
Ответ: Add a timezone property set to America/New_York in the Schedule trigger configuration

Как получено: Согласно документации Kestra, свойство timezone внутри триггера Schedule позволяет задать конкретный регион.

Выполнено в рамках обучения на курсе [Data Engineering Zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)
