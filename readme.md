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

```puml
@startuml
!if %variable_exists("RELATIVE_INCLUDE")
  !include %get_variable_value("RELATIVE_INCLUDE")/C4_Container.puml
!else
  !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!endif

SHOW_PERSON_OUTLINE()

title Context Diagram for Recomendation System
    Person_Ext(customer, Customer, "A customer of video service, who can watch videos based on their preferences and history")
    System_Ext(video_service, "Video service", "A service that provides videos to customers")
    Container(rec_sys, "Recommendation System", "Spark, S3, K8S", "Recommends videos to customers based on their preferences and history")

Rel(customer, video_service, "Watch videos")
Rel_Back(customer, video_service, "Suggest video")
Rel(video_service, rec_sys, "Gives customer watch history")
Rel_Back(video_service, rec_sys, "Suggest video")

SHOW_LEGEND()
@enduml
```


## Containers

![](c4-containers.svg)

## Components

![](c4-components.svg)
