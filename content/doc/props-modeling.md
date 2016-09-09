+++
date = "2016-09-09T12:52:00+05:00"
title = "Props modeling"
[menu.doc]
    parent = "schema"
    weight = 110
+++

В зависимости от типа поля, его может характеризовать определённый набор свойств. Кроме того, существует набор свойств,
применимый к каждому типу полей. Ниже перечислены общий свойства, а так же собственные свойства для каждого типа.

## Общие свойства

### access
Массив правил, определяющих доступ к полю. Синтаксис аналогичен свойству [access](/doc/store_reference/#access) Store.

### default
Определяет значение поля по-умолчению. Будет установлено при создании объекта, в случае, если значение не определено в создаваемом объекте.

### required
Флаг обязательного поля (bool). Если `true`, то при сохранении объекта с неустановленным значением поля будет возвращена ошибка.
В пользовательском интерфейсе при этом будет заблокирована кнопка **Сохранить**.

## action
Поле данного типа не сохраняется в БД, используется только пользовательском интерфейсе. Подробнее в разделе
[Props displaying](/doc/props-displaying/#action).

## bool
Булевый тип, может принимать значения `true` или `false`. Не имеет собственных свойств.


## date

### Свойства

#### utc
Флаг определяющий хранение даты во временной зоне UTC+0 (bool).


## file
Поле, применяющееся для хранения файлов. В СУБД при этом хранится только мета-описание файлов, сами файлы располагаются в файловом хранилище,
являющимся отдельным сервисом Blank.

### Свойства

#### accept
Позволяет задать маску принимаемых для загрузки файлов (строка).
В настоящее время не реализовано.

#### store
Название Store с типом `file` для хранения мета-информации о файлах (строка).


## float
Вещественный тип, или тип с плавающей запятой.

### Свойства

#### max
Максимальное допустимое значение поля.

#### min
Минимальное допустимое значение поля.


## int
Целочисленный тип.
Набор собственных свойств аналогичен [type:float](/doc/props-modeling/#float).

## object
Вложенная структура данных. Поддерживается только один уровень вложенности объектов.

### Свойства

#### props
Описание полей вложенной структуры данных.

```javascript
type: "object",
props: {
    // Поля вложенного объекта
},
```


## objectList
Массив вложенных структур данных. Поддерживается только один уровень вложенности объектов.

### Свойства

#### maxLength
Максимальное допустимое количество вложенных структур.

#### maxLength
Минимальное допустимое количество вложенных структур.

#### props
Описание полей вложенных структур данных.

```javascript
type: "objectList",
props: {
    // Поля вложенных объектов
},
```



## ref
Ссылка на другой объект. Подходит для связей 1-1, N-1. Значением является строка с идентификатором объекта.
Работает каскадное обновление связей, подробнее об этом механизме читайте в разделе [References sync](/doc/ref_sync/).

### Свойства

#### disableRefSync
Флаг, указывающий, что не требуется проводить обновление связи данного поля (bool).
Подробнее о механизме каскадного обновления связей читайте в разделе [References sync](/doc/ref_sync/)

#### oppositeProp
Название поля в Store, определённой свойством [store](/doc/props-modeling/#store) (строка).
Требуется для работы механизма каскадного обновления связей.

#### populateIn
Поле, в которое будет загружен соответствующий объект из Store
определённом в свойстве [store](/doc/props-modeling/#store).

#### store {#ref-store}
Название Store объекта, на который указывает ссылка (строка).

## refList
Массив ссылок на другие объекты, используется для создания связей 1-N, M-N. Значение записывается в виде
массива строковых идентификаторов.
Работает каскадное обновление связей, подробнее об этом механизме читайте в разделе [References sync](/doc/ref_sync/).

Набор собственных свойств аналогичен [type:ref](/doc/props-modeling/#ref).

## string
Строковый тип.

### Свойства

#### mask
Маска для значения (строка или объект).

#### maxLength
Максимальная допустимая длина строки (int).

#### minLength
Минимальная допустимая длина строки (int).

#### pattern
Регулярное выражение для проверки вводимого значения (строка или объект).


## virtual
Витруальное поле, вычисляемое при получении данных. Значения данных полей не сохраняются в БД.

### Свойства

#### load
Функция-загрузчик виртуального поля (строка).
В функции доступны объекты `$item` (текущий объект) и `$user` (пользователь, работающий с объектом).

```javascript
type: "virtual",
load: function ($item, $user) {
    return $item.prop1 + $item.prop2;
},
```