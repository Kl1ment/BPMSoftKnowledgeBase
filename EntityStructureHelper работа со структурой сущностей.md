
---
### 1. Описание модуля

`EntityStructureHelper` — это утилитарный модуль, разработанный для работы со схемами сущностей в BPMSoft. Он предоставляет набор методов для получения информации о колонках сущностей, включая их типы данных, агрегационные функции, обратные связи (backward columns) и фильтрацию. Модуль используется в клиентской части системы и интегрируется с песочницей (sandbox) для взаимодействия с другими компонентами.

### 2. Зависимости

Модуль зависит от следующих компонентов:

- `"ext-base"` — базовая библиотека ExtJS.
- `"BPMSoft"` — основная библиотека BPMSoft.
- `"ConfigurationEnums"` — перечисления конфигурации (например, типы данных, агрегационные функции).
- `"EntityStructureHelperResources"` — локализованные ресурсы (строки) для отображения подписей.

### 3. Инициализация

Модуль инициализируется с помощью функции `init`, которая принимает объект параметров для настройки поведения.

### Параметры инициализации:

- `sa` (обязательный) — объект песочницы (sandbox).
- `summaryColumnsOnly` (логический) — включает фильтрацию только агрегируемых колонок.
- `useBackwards` (логический) — разрешает получение обратных связей (по умолчанию `true`).
- `displayId` (логический) — отображает идентификатор первичного ключа (по умолчанию `false`).
- `lookupsColumnsOnly` (логический) — фильтрует только колонки типа "Справочник" (lookup).
- `dataValueType` (enum) — фильтрует колонки по указанному типу данных (например, `BPMSoft.DataValueType.INTEGER`).
- `aggregationType` (enum) — задает тип агрегации (например, `BPMSoft.AggregationType.SUM`).
- `useExists` (логический) — добавляет колонку для функции EXISTS.
- `useCount` (логический) — добавляет колонку для функции COUNT (по умолчанию `true`).
- `columnsFiltrationMethod` (функция) — пользовательская функция фильтрации колонок.
- `excludeDataValueTypes` (массив) — типы данных, которые нужно исключить (по умолчанию исключается BLOB, если не указано иное).
- `allowedReferenceSchemas` (массив) — разрешенные схемы для справочников.
- `allowedColumns` (массив) — разрешенные колонки по их UId.
- `aggregatedDataValueTypes` (массив) — типы данных, поддерживающие агрегацию (по умолчанию: DATE_TIME, DATE, TIME, INTEGER, FLOAT, MONEY).
- `enableBlobDataValueType` (логический) — разрешает использование типа BLOB (по умолчанию `false`).

### Пример инициализации:

```javascript
var helper = Ext.create("EntityStructureHelper");
helper.init({
    sa: sandbox,
    summaryColumnsOnly: false,
    useBackwards: true,
    displayId: true,
    dataValueType: BPMSoft.DataValueType.INTEGER,
    aggregationType: BPMSoft.AggregationType.SUM
});

```

---

### 4. Основные методы

### 4.1. `init(params)`

Инициализирует модуль с заданными параметрами.

### 4.2. `getItems(identifier, callback, hasBackwardElements, scope)`

Получает список колонок сущности с учетом фильтров.

- `identifier` — объект с именем схемы (например, `{ referenceSchemaName: "Contact" }`).
- `callback` — функция обратного вызова, получает объект с колонками.
- `hasBackwardElements` — включает поддержку обратных связей.
- `scope` — контекст выполнения callback.

**Пример:**

```javascript
helper.getItems({ referenceSchemaName: "Contact" }, function(columns) {
    console.log(columns);
}, true, this);

```

### 4.3. `getChildren(identifier, callback, scope)`

Получает дочерние колонки типа "Справочник" (lookup) для заданной схемы.

- `identifier` — объект с именем схемы справочника.
- `callback` — функция обратного вызова.
- `scope` — контекст выполнения.

**Пример:**

```javascript
helper.getChildren({ referenceSchemaName: "Account" }, function(columns) {
    console.log(columns);
}, this);

```

### 4.4. `getItemCaption(identifier, callback, scope)`

Возвращает подпись (caption) схемы.

**Пример:**

```javascript
helper.getItemCaption({ referenceSchemaName: "Contact" }, function(caption) {
    console.log(caption); // "Контакт"
}, this);

```

### 4.5. `getBackwardColumns(identifier, callback)`

Получает обратные связи (backward columns) для схемы.

**Пример:**

```javascript
helper.getBackwardColumns("Contact", function(result) {
    result.collection.each(function(column) {
        console.log(column.values);
    });
});

```

### 4.6. `hasAggregationColumns(schemaName, columnAggregationType, callback, scope)`

Проверяет, есть ли в схеме колонки, поддерживающие заданный тип агрегации.

**Пример:**

```javascript
helper.hasAggregationColumns("Contact", BPMSoft.AggregationType.SUM, function(result) {
    console.log(result); // true или false
}, this);

```

### 4.7. `getColumnPathCaption(dataSend, callback, scope)`

Получает подпись пути колонки через вызов сервиса.

**Пример:**

```javascript
helper.getColumnPathCaption({ schemaName: "Contact", columnPath: "Name" }, function(response) {
    console.log(response);
}, this);

```

### 4.8. `isAggregateFunctionCanBeUsedToType(columnDataValueType, columnAggregationType)`

Проверяет, применима ли агрегационная функция к типу данных.

**Пример:**

```javascript
var canUse = helper.isAggregateFunctionCanBeUsedToType(
    BPMSoft.DataValueType.INTEGER,
    BPMSoft.AggregationType.SUM
);
console.log(canUse); // true

```

---

### 5. Пример полного использования

```javascript
define("MyModule", ["EntityStructureHelper"], function(EntityStructureHelper) {
    var sandbox = { /* ваш объект sandbox */ };
    var helper = Ext.create("EntityStructureHelper");

    // Инициализация
    helper.init({
        sa: sandbox,
        useBackwards: true,
        displayId: false,
        aggregationType: BPMSoft.AggregationType.COUNT
    });

    // Получение колонок сущности Contact
    helper.getItems({ referenceSchemaName: "Contact" }, function(columns) {
        console.log("Колонки:", columns);

        // Получение дочерних справочников для первой колонки
        var firstColumn = columns[Object.keys(columns)[0]];
        if (firstColumn.isLookup) {
            helper.getChildren({ referenceSchemaName: firstColumn.referenceSchemaName }, function(childColumns) {
                console.log("Дочерние колонки:", childColumns);
            }, this);
        }
    }, true, this);

    return {};
});

```

---

### 6. Примечания

- Модуль активно использует асинхронные вызовы, поэтому большинство методов требуют callback-функции.
- Поддержка обратных связей (`useBackwards`) может замедлить выполнение при большом количестве связанных сущностей.
- Фильтрация колонок настраивается через параметры инициализации или пользовательскую функцию `columnsFiltrationMethod`.