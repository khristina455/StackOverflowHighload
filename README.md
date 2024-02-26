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
  - Добавление ответа
  - Прикрепление к ответам/вопросам фото/видео/файл
  - Поиск по вопросам и тегам
  - Оценки вопросов/ответов
 
## 2. Расчет нагрузки
### Продуктовые метрики
 Данные взяты [semrush](https://www.semrush.com/website/stackoverflow.com/overview/) [sighHouse](https://www.usesignhouse.com/blog/stack-overflow-stats#how-many-questions-are-being-asked-in-stack-overflow) [stack overflow statistics](https://observablehq.com/@ayhanfuat/the-fall-of-stack-overflow)

 - Месячная аудитория 100 млн
 - Дневная аудитория 4.6 млн
 - 6 тыс. вопросов в день
 - 7 тыс. ответов в день
 - 45 тыс. оценок в день

   Всего на Stack Overflow 23 миллиона пользователей, 24 миллиона вопросов и 36 миллионов ответов. Из чего следует, что в среднем на одного пользователя приходится 1.04(примерно 1) вопроса и 1.56(примерно 2) ответа. Вопрос в среднем занимает 1000 символов - 1 Кб, из них примерно 10% имеют вложения(в большинстве это 1-2 фотографии) - 200-500 Кб. Ответ в среднем занимает тоже 1 Кб. Информации об общем количестве оценок нет, но из данных о количестве вопросов и оценок в день можно предположить, что оценок около 160 миллионов. Это примерно 7 оценок на пользователя - 7 байт. Итого: 3.5 Кб на пользователя.

   #### Среднее количество действий пользователя по типам:

| Тип действия                                    | Количество/период |
|-------------------------------------------------|:-----------------:|
| Создание вопроса                                |     1 в 10 лет    |
| Добавление ответа                               |     1 в 9 лет     |
| Прикрепление к ответам/вопросам фото/видео/файл |     1 в 100 лет   |
| Поиск по вопросам и тегам                       |     5 в месяц     |
| Оценки вопросов/ответов                         |     1 в 2 года    |

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

4. Поиск по вопросам и тегам

```
RPS: 12М / 86 400 = 138
```

5. Оценки вопросов/ответов

```
RPS: 45 000 / 86 400 = 0.52
```


#### В итоге

| Запрос                                          |  RPS  |
|-------------------------------------------------|:-----:|
| Авторизация/Регистрация                         | 0.67  |
| Создание вопроса                                | 0.07  |
| Добавление ответа                               | 0.08  |
| Поиск по вопросам и тегам                       | 138   |
| Оценки вопросов/ответов                         | 0.52  |




