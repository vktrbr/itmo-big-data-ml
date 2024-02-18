# Case

Нужно разработать рекомендательную систему с 3 моделями:

* которые учитывают видео, просмотренные пользователем за последние 10мин,
* активная выбирается по метрике NDCG каждую неделю по результатам A / B теста

## Mindmap

```mermaid
mindmap
    Рекомендательная система
        Клиент
            Прием данных о клиенте в NRT режиме
            Отправка рекомендаций в NRT режиме
            API для взаимодействия с клиентом
        Модели
            Нужно производить расчет моделей на батче клиентов для выбора лучшей модели
                Батч режим для анализа моделей
                Расчет метрик для выбора активной модели
                NDCG метрика для выбора активной модели
            Нужно считать модель в реальном времени для рекомендаций
                Транспорт данных до сервиса расчета моделей
                NRT режим для возвращения рекомендаций потребителю
        База данных просмотров
            Хранение истории просмотров
            Хранение метрик по пользователям
            Подготовка данных для батчевого расчета, оценки и обучения моделей
```

## Use Cases

```mermaid
flowchart LR
    Client((Клиент))
    ABTesting((A/B тестирования))
    DataScience((DS Платформа))
    subgraph Что умеет рекомендательная система
        client_uc1([Предсказание в NRT режиме])
        client_uc2([Запись в базу данных просмотров])
        client_uc3([Создание новых клиентов])
        client_uc5([Батчевая загрузка истории просмотров])
        ds_uc4([Просмотр метрик по моделям])
        ds_uc1([Расчет моделей на батче клиентов])
        ds_uc2([Расчет метрик для выбора активной модели])
        recsys_uc4([Дизайн A/B теста])
        recsys_uc5([Мониторинг A/B тестов])
    end
    Client --- client_uc1
    Client --- client_uc2
    Client --- client_uc3
    Client --- client_uc5
    DataScience --- ds_uc4
    DataScience --- ds_uc1
    DataScience --- ds_uc2
    ABTesting --- recsys_uc4
    ABTesting --- recsys_uc5
```

# C4

## Context

![](c4-context.svg)

```mermaid
C4Context
    title System Context diagram for recommendation system
    Person(user, "User", "The user of the video service")
    System(recSys, "Rec Sys", "Recommendation system")
    System_Ext(video_service, "Video service", "Video service platform")
    Rel(recSys, video_service, "Sends recommendation")
    Rel(video_service, user, "Sends video")
    Rel(video_service, recSys, "Sends statistics")
```

## Containers

![](c4-containers.svg)

## Components

![](c4-components.svg)
