# Описание проекта

Источником вдохновения является тема моей дипломной работы.

## Формулировка задачи

### Введение

В свете недавних заявлений представителей власти Российской Федерации, касающихся развития
фондового рынка, актуальность исследований и проектов в этой области значительно
возрастает. Президент Российской Федерации в своем обращении к Федеральному Собранию
[заявил](https://www.rbc.ru/quote/news/article/65e05bd49a794704415b7f6e) о планах удвоить
капитализацию фондового рынка к 2030 году. В кругу финансовых экспертов бытует мнение, что
этот план предполагает активное вовлечение крупных российских компаний и потенциальное
принуждение к формулированию дивполитики при листинге акций, что может привести к более
широкому распределению активов среди населения.

### Цель проекта

Целью данного проекта является разработка новых инструментов и подходов для анализа и
прогнозирования состояния индекса МосБиржи (IMOEX) на основе данных из открытых
источников, включая новостные ленты и социальные сети. Проект направлен на создание
соверменных и эффективных инструментов управления инвестициями.

## Данные

### IMOEX

Данные о значения индекса МосБиржи можно получить непосредственно с
информационно-статистического сервера Московской Биржи (ИСС / ISS), либо через готовые
обертки типа [`aimoex`](https://wlm1ke.github.io/aiomoex/build/html/index.html)

### Новостные данные

Тексты новостей можно получать через RSS с помощью
[feedparser](https://pypi.org/project/feedparser/) или
[gnewsclient](https://pypi.org/project/gnewsclient/). Пример потоков изданий:
[Ведомости](https://www.vedomosti.ru/info/rss),
[РБК](http://static.feed.rbc.ru/rbc/logical/footer/news.rss),
[ТАСС](https://tass.ru/rss/google.xml).

### Данные из соц. сетей

Чтобы разнообразить набор данных, можно рассмотреть опцию их обогащения постами из
профильных (экономических, экспретных, инвестиционных. Например, из топа
[каталога tgstat](https://tgstat.ru/economics)) телеграм-каналов и комментарии в новостных
пабликах c помощью [`telethone`](https://docs.telethon.dev/en/stable/)

### Предобработка текстовых данных

#### Очистка текста

- Удаление HTML-тегов, ссылок, специальных символов
- Приведение текста к нижнему регистру.
- Удаление стоп-слов

#### Токенизация

- Разделение текста на отдельные слова или токены.

#### Лемматизация и стемминг

- Приведение слов к их базовой или корневой форме для уменьшения размерности данных.

Из предобработанных тестовых данных можно определять тональность (чтобы определить
позитивное, негативное или нейтральное настроение текста) с помощью
[ruBERT](https://huggingface.co/DeepPavlov/rubert-base-cased).

### Прогнозирование

На данном этапе предполагается использовать классические модели машинного обучения на
собранных и предобработанных данных: RF, Gradient Boosting, etc. Выбор модели будет
зависеть от качества предсказаний на собранных данных.

## Пайплайн проекта

Регулярные расчеты можно автоматизировать с помощью `airflow`:

```
Сбор таргета (индекс IMOEX)
->
Сбор текстов с выделенных новостных сайтов и соц. сетей
->
Предобработка собранных текстов
->
Анализ тональности собранных текстов
->
Запись собранных и/или обработанных данных
->
Обучение/до-обучение модели
->
Выдача прогноза индекса IMOEX
```

## Структура

```
mlops_project/
├── Dockerfile
├── README.md
├── imoex-forecaster
│   ├── __init__.py
│   ├── loaders
│   │   ├── __init__.py
│   │   ├── features_infer_loader.py
│   │   ├── features_train_loader.py
│   │   └── target_loader.py
│   ├── preprocess
│   │   ├── __init__.py
│   │   └── preprocess_data.py
│   └── utils
│       ├── __init__.py
│       └── date_utils.py
├── infer.py
├── poetry.lock
├── pyproject.toml
└── train.py
```
