# Highload: StackOverflow
## Содержание

## 1. Тема, аудитория, функционал
 **StackOverflow** — веб-сайт для создания вопросов и ответов в области IT.
 
### Аудитория
Согласно [semrush](https://www.semrush.com/website/stackoverflow.com/overview/) и [stack overflow](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow):

- 70+ миллионов пользователей в месяц
- 320 миллионов посещений в месяц
- 4+ миллионов поcещений в день

| Страна   | Процент пользователей от общего числа |
|----------|:-------------------------------------:|
| США      |                 24.1                  |
| Индия    |                 13.19                 |
| Россия   |                 5.39                  |
| Индонезия|                 4.4                   |
| Германия |                 3.21                  |
| Остальные|                 49.71                 |

### Функционал
  - Авторизация и регистрация
  - Создание вопроса
    -  Прикрепление фото и файлов
    -  Добавление тегов
  - Добавление ответа
    -  Прикрепление фото и файлов
    -  Возможность пользователю добавления только одного ответа к вопросу
  - Комментирование вопросов и ответов
  - Поиск по вопросам и тегам
  - Оценки вопросов/ответов
  - Статистика просмотра вопросов
 
## 2. Расчет нагрузки
### Продуктовые метрики
 Данные взяты [semrush](https://www.semrush.com/website/stackoverflow.com/overview/) [sighHouse](https://www.usesignhouse.com/blog/stack-overflow-stats#how-many-questions-are-being-asked-in-stack-overflow) [stack overflow statistics](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow)


| Метрика                                         |      Значение     |
|-------------------------------------------------|:-----------------:|
| MAU                                             |     100 млн.      |
| DAU                                             |     4.6 млн.      |
| Количество вопросов в день                      |     6 тыс.        |
| Количество ответов в день                       |     7 тыс.        |
| Количество оценок в день                        |     45 тыс.       |
| Средний размер вопроса*                         |     5 Кб          |
| Средний размер отвта*                           |     5 Кб          |
| Средний размер комментария*                     |     1 Кб          |
| Средний размер хранилища пользователя*          |     15 Кб         |


   \* Всего на Stack Overflow 23 миллиона пользователей, 24 миллиона вопросов и 36 миллионов ответов. Из чего следует, что в среднем на одного пользователя приходится 1.04(примерно 1) вопроса и 1.56(примерно 2) ответа. Вопрос в среднем занимает 1000 символов - 1 Кб, из них примерно 10% имеют вложения(в большинстве это 1-2 фотографии) - 200-500 Кб, тогда получаем в среднем 5 Кб. Ответ в среднем занимает тоже 1 Кб. Информации об общем количестве оценок нет, но из данных о количестве вопросов и оценок в день можно предположить, что оценок около 160 миллионов. Это примерно 7 оценок на пользователя - 7 байт. Информации о комментариях тоже нет, поэтому предположим, что метрики сзвязанные с ними совпадают с метриками ответов(за исключением наличия вложений). Итого: 17 Кб на пользователя.

   #### Среднее количество действий пользователя по типам:

| Тип действия                                    | Количество в месяц|
|-------------------------------------------------|:-----------------:|
| Создание вопроса                                |       0.0083      |
| Добавление ответа                               |       0.0092      |
| Поиск по вопросам и тегам                       |         5         |
| Оценки вопросов/ответов                         |        0.04       |
| Комментирование                                 |       0.0092      |

### Технические метрики
#### Хранилище

- Вопросы: `23М * (1 Кб + 0.1 * 500 Кб) = 1.173 Тб`
- Ответы: `23М * 2 * 1 Кб = 46 Гб`
  
#### Сетевой трафик и rps

Количество посещений страниц в день равно 12 миллионам. В день создается 6 тыс. вопросов, 7 тыс. ответов и ставится 45 тыс. оценок. Из-за малости количества запросов перечисленных выше можно сказать, что примерно 12 миллионов посещений это просто просматривание страниц и в основном страниц вопросов.

Используя. "инструменты разработчика" в браузере мы можем примерно рассчитать размер .html, .js и .css файлов для
отрисовки каждой из страниц и количество запросов для их получения:

| Страница                         | Размер в Кб | Кол-во запросов |
|----------------------------------|-------------|-----------------|
| Страница вопроса                 | 645         | 30              |
| Страница создания вопроса        | 917         | 32              |
| Главная страница                 | 3470        | 154             |

Рассчитаем общий трафик:

```azure
Трафик: (645 Кб * 12М + 917 Кб * 6K + 3470 Кб * 10К) / 86 400 = 90 Гб/c
RPS: (30 * 12М + 32 * 6K + 154 * 10К) / 86 400 = 4 186
```

Рассмотрим запросы из продуктовых требований для расчёта RPS:

1. Авторизация и регистрация

```
RPS: 58 000 / 86 400 = 0.67
```
    
2. Создание вопроса

```
RPS: 6 000 / 86 400 = 0.07
```

3. Добавление ответа

```
RPS: 7 000 / 86 400 = 0.08
```

4. Комментирование

```
RPS: 7 000 / 86 400 = 0.08
```

4. Поиск по вопросам и тегам

```
RPS: 12М / 86 400 = 138
```

5. Оценки вопросов/ответов

```
RPS: 45 000 / 86 400 = 0.52
```

6. Статистика просмотра вопросов(будем считать что открытие вопроса это один запрос)

```
RPS: 12M / 86 400 = 138
```
Пиковый коэффициент найден не был, поэтому предположим, что он равен 1.4

#### В итоге

| Запрос                                          |  RPS  | Пиковый RPS |
|-------------------------------------------------|:-----:|:-----------:|
| Авторизация/Регистрация                         | 0.67  |    0.938    |
| Создание вопроса                                | 0.07  |    0.098    |
| Добавление ответа                               | 0.08  |    0.113    |
| Поиск по вопросам и тегам                       | 138   |    193.2    |
| Оценки вопросов/ответов                         | 0.52  |    0.728    |
| Статистика просмотра вопросов                   | 138   |    193.2    |


## 3. Глобальная балансировка

### Расположение датацентров

Поскольку Stackoverflow нацелен в первую очередь на глобальный рынок, серверы будут размещаться в разных странах для
обеспечения лучшего подключения.

![image](https://github.com/khristina455/StackOverflowHighload/assets/91967143/55aca888-36f7-4b21-a7e5-403189136d65)


Если мы рассмотрим плотность пользователей Stackoverflow по регионам, то можно сказать, что большая часть активных
пользователей находится в Северной Америке и Европе. Исходя из этого, большее число арендуемых датацентров должны быть
размещены ближе к этим регионам, чтобы обеспечить быстрый доступ и низкую задержку.
В Европе можно рассмотреть аренду датацентра у провайдеров, таких как Digital Realty или Equinix.
У Digital Realty есть несколько датацентров в Европе, включая Лондон, Амстердам и Франкфурт.
У Equinix также есть несколько датацентров в Европе, включая Лондон, Париж и Франкфурт.
В Северной Америке можно рассмотреть аренду датацентра у провайдеров, таких как Amazon Web Services (AWS) или Microsoft
Azure.

Арендуем по одному датацентру во **Франкфурте (Европа)**, **Вирджинии (Северная Америка)** и **Калифорнии (Северная
Америка)**, чтобы обеспечить покрытие основных регионов с наибольшей аудиторией.

Кроме того, учитывая востребованность Stackoverflow в Азиатско-Тихоокеанском регионе, можно арендовать один датацентр в Индии.

### Балансировка

Для обеспечения эффективной балансировки нагрузки и доставки контента в разных регионах, мы можем использовать
комбинацию Anycast и CDN (Content Delivery Network).

## 4. Локальная балансировка

Запросы будут приходить на L4 балансировщик, который будет распределять их на L7 балансировщики(nginx). L4 балансировщик имеет минимальную задержку, поэтому позволит быстро распредлелить первоначальный трафик. Дальше же распределять запросы будут L7 балансировщики, у которых много преимуществ, но конкретно для нашей системы важны:
 - Кэширование. Самые частые запросы - это взятие вопроса, поэтому снизится нагрузка на бэкенд, если мы будем их кэшировать
 - Сжатие gzip
 - Терминация SSL

## 5. Логическая схема БД

![Untitled(1)](https://github.com/khristina455/StackOverflowHighload/assets/91967143/44138fa1-960d-4cc6-b7b1-5ef84b160cd7)


## 6. Физическая схема БД
Для хранения данных будем использовать PostgreSQL(кроме сессий и аналитики). Это популярная БД, которая хорошо масштабируется и имеет большое сообщество, соответственно проще поддерживат и имеет высокую надежность. Также есть возможность использовать репликацию и шардирование.

### User
| Столбец    | Тип          | Размер |
| ---------- | ------------ | ------ |
| id         | int          | 4 Б    |
| avatar     | varchar(256) | 256 Б  |
| nickname   | varchar(256) | 256 Б  |
| email      | varchar(256) | 256 Б  |
| confirmed  | boolean      | 1 Б    |
| passwd     | varchar(256) | 256 Б  |
| created_at | timestamp    | 4 Б    |

Всего 23 млн зарегистрированных пользователей

Размер таблицы: 23 * 10<sup>6</sup> * (4 + 256 + 256 + 256 + 1 + 256 + 4) Б = 23 * 10<sup>6</sup> * 777 Б = 17'871'000'000 Б = 17.87 ГБ

### User Session
| Столбец    | Тип          | Размер |
| ---------- | ------------ | ------ |
| id         | int          | 4 Б    |
| user_id    | int          | 4 Б    |
| token      | varchar(256) | 256 Б  |

Для хранения сессий будем использовать Redis, так как он быстрее и проще в использовании, чем PostgreSQL. Данная БД хранит данные в оперативной памяти, обладает высокой производительностью, поэтому не подходит для хранения больших объемов данных, но для хранения сессий подходит идеально.

Размер таблицы: 23 * 10<sup>6</sup> * (4 + 4 + 256 + 4) Б = 23 * 10<sup>6</sup> * 268 Б = 6'164'000'000 Б = 6.16 ГБ

### Question
| Столбец     | Тип             | Размер    |
| ----------- | --------------  | --------- |
| id          | int             | 4 Б       |
| user_id     | int             | 4 Б       |
| title       | varchar(256)    | 256 Б     |
| message     | varchar(8192)   | 8192 Б    |
| img         | varchar(256)[N] | N * 256 Б |
| tag         | varchar(32)[N]  | N * 32 Б  |
| created_at  | timestamp       | 4 Б       |
| likes_count | int             | 4 Б       |

Всего 23 млн вопросов

Размер таблицы: 23 * 10<sup>6</sup> * (4 + 4 + 256 + 8192 + 256 + 32*4 + 4 + 4) Б = 23 * 10<sup>6</sup> * 8'848 = 203.504 ГБ

Шардирование по **user_id**

### Answer
| Столбец     | Тип             | Размер    |
| ----------- | --------------- | --------- |
| id          | int             | 4 Б       |
| user_id     | int             | 4 Б       |
| question_id | int             | 4 Б       |
| message     | varchar(8192)   | 8192 Б    |
| img         | varchar(256)[N] | N * 256 Б |
| created_at  | timestamp       | 4 Б       |
| likes_count | int             | 4 Б       |

Всего 36 млн ответов

Размер таблицы: 36 * 10<sup>6</sup> * (4 + 4 + 4 + 8192 + 256 + 4 + 4) Б = 36 * 10<sup>6</sup> * 8'768 Б = 298'112'000'000 Б = 298.11 ГБ

Шардирование по **question_id**

### QuestionComment + AnswerComment
| Столбец                 | Тип           | Размер |
| ----------------------- | ------------- | ------ |
| id                      | int           | 4 Б    |
| user_id                 | int           | 4 Б    |
| question_id / answer_id | int           | 4 Б    |
| parent_id               | int           | 4 Б    |
| message                 | varchar(512)  | 512 Б  |
| created_at              | timestamp     | 4 Б    |

Всего 36 млн комментариев

Размер таблицы: 36 * 10<sup>6</sup> * (4 + 4 + 4 + 4 + 512 + 4) Б = 36 * 10<sup>6</sup> * 536 Б = 19'296'000'000 Б = 19.3 ГБ

### QuestionLike + AnswerLike
| Столбец                 | Тип           | Размер |
| ----------------------- | ------------- | ------ |
| id                      | int           | 4 Б    |
| user_id                 | int           | 4 Б    |
| question_id / answer_id | int           | 4 Б    |
| created_at              | timestamp     | 4 Б    |
| state                   | int           | 4 Б    |

Всего 160 млн оценок

Размер таблицы: 160 * 10<sup>6</sup> * (4 + 4 + 4 + 4 + 4) Б = 160 * 10<sup>6</sup> * 20 Б = 3'200'000'000 Б = 3.2 ГБ

Лайки есть только у зарегестрированных пользователей. Хранить будем в ClickHouse. ClickHouse является колоночной БД и хорошо подходит для аналитики, так как быстро обрабатывает большие объемы данных.

### QuestionView
| Столбец     | Тип           | Размер  |
| ----------- | ------------- | ------- |
| id          | int           | 4 Б     |
| user_id     | int           | 4 Б     |
| question_id | int           | 4 Б |
| created_at  | timestamp     | 4 Б     |

Историю просмотров будем хранить только для зарегистрированных пользователей в ClickHouse. 

Размер данных за два месяца: 23 * 10<sup>6</sup> * (4 + 4 + 4 * 2.47 * 60 * 2 + 4) Б = 23 * 10<sup>6</sup> * 1'178'400 Б = 27'115'200'000 Б = 27.12 ГБ

### ElasticSearch
| Столбец     | Тип            | Размер    |
| ----------- | -------------- | --------- |
| title       | varchar(256)   | 256 Б     |
| message     | varchar(8192)  | 8192 Б    |
| tag         | varchar(32)[N] | N * 32 Б  |

Для поиска будем использовать ElasticSearch. Он позволяет использовать полнотекстовый поиск по документам.

Размер таблицы: 23 * 10<sup>6</sup> * (256 + 8192 + 32*4) Б = 23 * 10<sup>6</sup> * 8'576 = 197.248 ГБ

### Media

Для картинок будет использоваться S3. Это хранилище от Amazon, которое хорошо масштабируется и имеет высокую надежность.

### Индексы
| Таблица  | Поле        |
| -------- | ----------- |
| User     | id          |
| Question | user_id     |
| Answer   | question_id |
| Session  | token       |

### Репликация
| Таблица  | Тип          |
| -------  | ------------ |
| User     | master-slave |
| Question | master-slave |
| Answer   | master-slave |
| Comment  | master-slave |

