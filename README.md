# Влияние Твиттера на финансовые рынки

Данный репозиторий содержит мою итоговою проектную работу по курсу OTUS "**Machine Learning. Professional**" на тему "**Влияние Твиттера на финансовые рынки**".

Целью работы является проверка мифа о том, что твиты влиятельных людей могут влиять на фондовые рынки. В данной работе планируется:

- Попытаться кластеризовать твиты различными методами
- Попытаться сделать тематическое моделирование твитов
- Попытаться предсказать рост/падение актива по содержимому твита

В данном [README](README.md) содержится краткое описание проделанной работы с необходимыми вставками кода и графиков из соответствующих jupyter notebook-ов, ссылки на которые будут даны по тексту работы.

## Данные

Краеугольным камнем любого ML проекта являются данные. Для моего проекта мне потребовались данные о твитах и о биржевом курсе финансового актива.

### Твиты

В качестве людей, чье содержимое твиттер аккаунтов я использовал, мною были выбраны Илон Маск и Дональд Трамп, так как они являются активными пользователями твиттера и считается, что твиты Маска могут влиять на курсы криптовалют (таких как DOGE).

Изначально я собирался спарсить твиттер при помощи beautifulsoup4, но с этим планом возникло несколько проблем:

- Твиттер - это SPA, которое динамически подгружает содержимое страницы, поэтому просто спарсить html не получится ([пруф](https://stackoverflow.com/questions/68987825/problem-while-scraping-twitter-using-beautiful-soup)).
- С недавнего времени Твиттер заблокирован в России.
- Аккаунт Дональда Трампа заблокирован в Твиттере.

Хотя часть этих проблем решаема, я решил найти готовые наборы данных. На помощь, как это часто бывает, пришел Kaggle. Мною были найдены архивы твитов Илона Маска ([ссылка](https://www.kaggle.com/datasets/ayhmrba/elon-musk-tweets-2010-2021)) и Дональда Трампа ([ссылка](https://www.kaggle.com/datasets/headsortails/trump-twitter-archive)). Данные наборы содержат разные столбцы, среди которых нас будут интересовать такие общие столбцы как ID, дата и текст твита.

### Биржевой курс

В качестве источника цены финансового актива мной был взят [Yahoo Finance](https://finance.yahoo.com/). Я выбрал следующие активы:

- Криптовалюта Bitcoin ([ссылка](https://finance.yahoo.com/quote/BTC-USD/history?p=BTC-USD))
- Криптовалюта Dogecoin ([ссылка](https://finance.yahoo.com/quote/DOGE-USD/history?p=DOGE-USD))
- Акции компании Tesla ([ссылка](https://finance.yahoo.com/quote/TSLA/history?p=TSLA))

## Предобработка данных

После загрузки данных их необходимо предобработать.

### Твиты

Как было сказано раньше, данные твитов Илона Маска и Дональда Трампа имели разный набор столбцов. Из них я выделил такие общие столбцы как ID, дата и текст твита.
Затем все тексты были специальным образом обработаны (удалены упоминания других пользователей, ссылки, служебные html символы, а также заменены все последовательности небуквенных символов на пробел), используя следующую функцию:

```python
def text_preprocessor(v: str) -> str:
    v = v.lower()
    v = re.sub(r'@[^\s]+', '', v)
    v = re.sub(r'https?://[^\s]+', '', v)
    v = re.sub(r'&\w+;', '', v)
    v = re.sub('\W+', ' ', v)
    v = v.strip()
    return v
```

После данной обработки пустые твиты были удалены (они содержали только ссылки, упоминания других пользователей и т.п.).

В результате получился набор данных, содержащий столбцы id, date, text и cleared_text.

Для ускорения дальнейшей работы для каждого твита были сгенерированы векторные представления при помощи [векторов из библиотеки FastText](https://fasttext.cc/docs/en/english-vectors.html) с весами TF-IDF и [трансформерной модели LaBSE](https://huggingface.co/cointegrated/LaBSE-en-ru).

Полный исходный код обработки представлен в notebook-ах [Tweets Preprocessing](preprocessing/Tweets%20Preprocessing.ipynb) и [Tweets Preprocessing Transformers](preprocessing/Tweets%20Preprocessing%20Transformers.ipynb).

### Биржевой курс

Данные, загруженные с Yahoo Finance, содержат столбцы Date (Дата торгов), Open (Цена на момент открытия), High (Максимальная цена), Low (Минимальная цена), Close (Цена на момент закрытия), Adj Close (Скорректированная цена на момент закрытия) и Volume (Объем торгов).

Единственной обработкой, примененной к данным данным, было заполнение пропусков в выходные дни значениями из ближайшего следующего рабочего дня (данное справедливо только к акциям/обычным валютам, так как биржа в выходные дни не работает).

Исходный код можно найти в notebook-е [Tweets Ticker Template](ticker%20prediction/Tweets%20Ticker%20Template.ipynb) в разделе **Data loading**

## Кластеризация

### TF-IDF

### FastText

### LaBSE-en-ru

## Тематическое моделирование

Методы: NMF, LDA

### Дональд Трамп

### Илон Маск

## Предсказание роста/падения акций/криптовалют

### Обзор данных

### Baseline

### Logistic regression

### Random Forest

### Интерпретация результатов
