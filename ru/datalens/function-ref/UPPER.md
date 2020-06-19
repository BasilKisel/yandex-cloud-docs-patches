---
editable: false
---

# UPPER

_Строковые функции_

#### Синтаксис


```
UPPER( string )
```

#### Описание
Возвращает строку `string` в верхнем регистре.

**Типы аргументов:**
- `string` — `Строка`


**Возвращаемый тип**: `Строка`

#### Примеры

```
UPPER("Lorem ipsum") = "LOREM IPSUM"
```

```
UPPER("Карл у Клары") = "КАРЛ У КЛАРЫ"
```


#### Поддержка источников данных

`Материализованный датасет`, `ClickHouse 1.1`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `PostgreSQL 9.3`.