+++
date = "2016-08-06T19:38:05+05:00"
title = "Filters"
[menu.doc]
    parent = "schema"
    weight = 40
+++

Фильтры в Blank предназначены для определения заранее заданных критериев поиска объектов в Store.
В основном фильтры используются в веб-приложении, поэтому кроме запроса к БД в фильтрах настраивается
его внешний вид. Однако, при запросах Find через API также можно использовать названия фильтров и передавать
их значения.

Пример для создания фильтра по владельцу объектов:
~~~javascript
"ownerFilter": {
    //Вид фильтра в интерфейсе веб-приложения
    "label": "Владелец",
    "display": "searchBox",
    "store": "users",
    "searchBy": ["name"],
    "multi": true,
    "filterBy": "_id",
    //Запрос к БД
    "query": { "_ownerId": { "$in": "$value" } },
}
~~~

Если используются несколько фильтров, их запросы объединяются через оператор $and:
~~~javascript
"query": { "$and": [ Filter1Query, Filter2Query, ..., FilterNQuery ] },
~~~

## query

### funсtion ($value) {}:MongoQuery
Функция, принимающая на вход значение фильтра в виде переменной `$value` и возвращающая запрос в формате
[MongoDB Query](https://docs.mongodb.com/manual/tutorial/query-documents/).

~~~javascript
"query": ($value) => {
    if ($value) {
        return { orderState: { $in: $value } };
    }
}
~~~
### MongoQuery {}
Вариант настройки непосредственно через [MongoDB Query](https://docs.mongodb.com/manual/tutorial/query-documents/),
в котором в значении одного или нескольких условий указано `"$value"`.
Пример:
```javascript
"query": { "_ownerId": { "$in": "$value" } },
```
Blank заменит все вхождения `"$value"` на значения фильтра.