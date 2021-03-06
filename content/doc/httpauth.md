+++
date = "2016-08-17T14:54:48+05:00"
title = "Auth"
[menu.doc]
    parent = "httpapi"
    weight = 0
+++

Помимо полнофункционального WAMP API, **Blank** имеет в своем арсенале несколько вспомогательных HTTP методов,
в частности, методы, выполняющие функции авторизации и регистрации нового пользователя. Все методы возвращают ответ в формате JSON.

### Авторизация

Авторизация пользователя выполняется путём отправки **POST** запроса на адрес `{_commonSettings.baseUrl}/login`.
Запрос должен содержать `form-data` со следующими полями:
```
    login // логин пользователя (login) или адрес электронной почты (email)
    password // пароль
```

При успешной авторизации, будет возвращен ответ со статусом `200`, в теле которого содержится JSON документ с двума полями: `access_token`,
содержащее JWT token для последующих запросов и `user`, содержащий описание авторизованного пользователя:
```JSON
    {
        "access_token": "05547313-0ceb-41ad-965c-dee3c8992c71",
        "user": {...}
    }
```

Если любое из полей не указано, вернётся результат со статусом `400` и телом `invalid arguments`.

В случае, возникновения прочих ошибок, результат может быть возвращен со статусом `303` и подробным описанием возникшей ошибки в теле ответа,
либо со статусом `500` и телом `unknown error`.

### Регистрация

Регистрация пользователя выполняется путём отправки **POST** запроса на адрес `{_commonSettings.baseUrl}/register`.
Запрос должен содержать `form-data` со следующими полями:
```
    email // адрес электронной почты
    password // пароль
```

При успешной регистрации, будет возвращен пустой ответ со статусом `200`.

Если любое из полей не указано, вернётся результат со статусом `400` и телом `invalid arguments`.

В случае, возникновения прочих ошибок, результат может быть возвращен со статусом `303` и подробным описанием возникшей ошибки в теле ответа,
либо со статусом `500` и телом `unknown error`.
