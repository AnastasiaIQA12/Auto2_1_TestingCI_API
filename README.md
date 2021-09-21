[![Build status](https://ci.appveyor.com/api/projects/status/kge1ucaccd9wav8f?svg=true)](https://ci.appveyor.com/project/AnastasiaIQA12/automation2-1-il3x4)
# Домашнее задание к занятию «1.2. Тестирование API, CI»
## Задача №1 - Настройка CI
**Задание:** взять проект [с лекции](https://github.com/netology-code/aqa-code/tree/aqa4/api-ci), настроить для него CI. Удостовериться, что CI показывает, что сборка падает (в процессе сборки будут автоматически прогоняться все авто-тесты).

**Важно**: иногда можно настроить CI так, что там **всегда будет SUCCESS** 😈! Не забывайте убедиться, что в CI сборка действительно падает, если вы запушите в GitHub падающий тест.

## Задача №2 - JSON Schema

JSON Schema предлагает нам инструмент валидации JSON-документов.
Как строится схема: 
```js
{
  "$schema": "http://json-schema.org/draft-07/schema", // версия схемы: https://json-schema.org/understanding-json-schema/reference/schema.html
  "type": "array", // тип корневого элемента: https://json-schema.org/understanding-json-schema/reference/type.html
  "items": { // какие элементы допустимы внутри массива: https://json-schema.org/understanding-json-schema/reference/array.html#items
    "type": "object", // должны быть объектами: https://json-schema.org/understanding-json-schema/reference/object.html
    "required": [ // должны содержать следующие поля: https://json-schema.org/understanding-json-schema/reference/object.html#required-properties
      "id",
      "name",
      "number",
      "balance",
      "currency"
    ],
    "additionalProperties": false, // дополнительных полей быть не должно 
    "properties": { // описание полей: https://json-schema.org/understanding-json-schema/reference/object.html#properties
      "id": {
        "type": "integer" // целое число: https://json-schema.org/understanding-json-schema/reference/numeric.html#integer
      },
      "name": {
        "type": "string", // строка: https://json-schema.org/understanding-json-schema/reference/string.html
        "minLength": 1 // минимальная длина - 1: https://json-schema.org/understanding-json-schema/reference/string.html#length
      },
      "number": {
        "type": "string", // строка: https://json-schema.org/understanding-json-schema/reference/string.html
        "pattern": "^•• \\d{4}$" // соответствует регулярному выражению: https://json-schema.org/understanding-json-schema/reference/string.html#regular-expressions
      },
      "balance": {
        "type": "integer" // целое число: https://json-schema.org/understanding-json-schema/reference/numeric.html#integer
      },
      "currency": {
        "type": "string" // строка: https://json-schema.org/understanding-json-schema/reference/string.html
      }
    }
  }
}
```

**Задание:**

#### Шаг 1. Добавить зависимость

```groovy
dependencies {
    testImplementation 'io.rest-assured:rest-assured:4.3.0'
    testImplementation 'io.rest-assured:json-schema-validator:4.3.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.6.1'
}
```

#### Шаг 2. Сохранить схему в ресурсах

Создайте каталог `resources` в `src/test` и поместите туда схему

#### Шаг 3. Включить проверку схемы

Модифицируйте существующий тест так, чтобы он проверял соответствие схеме. Для этого:

```java
      // код теста
      .then()
          .statusCode(200)
          // static import для JsonSchemaValidator.matchesJsonSchemaInClasspath
          .body(matchesJsonSchemaInClasspath("accounts.schema.json"))
      ;
```

Удостовериться, что тесты проходят при соответствии ответа схеме и падают, если поменять что-то в схеме (например, тип для `id`)

#### Шаг 4. Доработать схему

Изучите документацию на тип [`object`](https://json-schema.org/understanding-json-schema/reference/object.html) и найдите способ валидации значения поля на два из возможных значения: "RUB" или "USD".

Доработайте схему соответствующим образом, удостоверьтесь, что тесты проходят (в том числе в CI).

Поменяйте "RUB" на "RUR" и удостоверьтесь, что тесты падают (в том числе в CI).

#### Шаг 5. Прислать на проверку ссылку на репозиторий (удостоверясь, что в истории сборки были как Success, так и Fail).
