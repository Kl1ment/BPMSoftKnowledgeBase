
### BPMSoft.ViewItemType

| Значение           | Значение | Тип элемента                                                            |
| ------------------ | -------- | ----------------------------------------------------------------------- |
| GRID_LAYOUT        | 0        | Элемент-сетка, включающий в себя размещение других элементов управления |
| TAB_PANEL          | 1        | Набор вкладок                                                           |
| DETAIL             | 2        | Деталь                                                                  |
| MODEL_ITEM         | 3        | Элемент модели представления                                            |
| MODULE             | 4        | Модуль                                                                  |
| BUTTON             | 5        | Кнопка                                                                  |
| LABEL              | 6        | Надпись                                                                 |
| CONTAINER          | 7        | Контейнер                                                               |
| MENU               | 8        | Выпадающий список                                                       |
| MENU_ITEM          | 9        | Элемент выпадающего списка                                              |
| MENU_SEPARATOR     | 10       | Разделитель выпадающего списка                                          |
| SECTION_VIEWS      | 11       | Представления раздела                                                   |
| SECTION_VIEW       | 12       | Представление раздела                                                   |
| GRID               | 13       | Реестр                                                                  |
| SCHEDULE_EDIT      | 14       | Планировщик                                                             |
| CONTROL_GROUP      | 15       | Группа элементов                                                        |
| RADIO_GROUP        | 16       | Группа переключателей                                                   |
| DESIGN_VIEW        | 17       | Настраиваемое представление                                             |
| COLOR_BUTTON       | 18       | Кнопка с выбором цвета                                                  |
| IMAGE_TAB_PANEL    | 19       | Набор вкладок с иконками                                                |
| HYPERLINK          | 20       | Гиперссылка                                                             |
| INFORMATION_BUTTON | 21       | Информационная кнопка c всплывающей подсказкой                          |
| TIP                | 22       | Всплывающая подсказка                                                   |
| COMPONENT          | 23       | Компонент                                                               |
| PROGRESS_BAR       | 30       | Индикатор                                                               |
| GRID_LAYOUT_EDIT   | 31       |                                                                         |
| IFRAMECONTROL      | 32       |                                                                         |


### BPMSoft.DataValueType

| Тип данных                        | Значение |
| --------------------------------- | -------- |
| GUID                              | 0        |
| TEXT                              | 1        |
| INTEGER                           | 4        |
| FLOAT                             | 5        |
| MONEY                             | 6        |
| DATE_TIME                         | 7        |
| DATE                              | 8        |
| TIME                              | 9        |
| LOOKUP                            | 10       |
| ENUM                              | 11       |
| BOOLEAN                           | 12       |
| BLOB                              | 13       |
| IMAGE                             | 14       |
| CUSTOM_OBJECT                     | 15       |
| IMAGELOOKUP                       | 16       |
| COLLECTION                        | 17       |
| COLOR                             | 18       |
| LOCALIZABLE_STRING                | 19       |
| ENTITY                            | 20       |
| ENTITY_COLLECTION                 | 21       |
| ENTITY_COLUMN_MAPPING_COLLECTION  | 22       |
| HASH_TEXT                         | 23       |
| SECURE_TEXT                       | 24       |
| FILE                              | 25       |
| MAPPING                           | 26       |
| SHORT_TEXT                        | 27       |
| MEDIUM_TEXT                       | 28       |
| MAXSIZE_TEXT                      | 29       |
| LONG_TEXT                         | 30       |
| FLOAT1                            | 31       |
| FLOAT2                            | 32       |
| FLOAT3                            | 33       |
| FLOAT4                            | 34       |
| LOCALIZABLE_PARAMETER_VALUES_LIST | 35       |
| METADATA_TEXT                     | 36       |
| STAGE_INDICATOR                   | 37       |
### BPMSoft.ComparisonType

| Тип данных       | Значение |
| ---------------- | -------- |
| BETWEEN          | 0        |
| IS_NULL          | 1        |
| IS_NOT_NULL      | 2        |
| EQUAL            | 3        |
| NOT_EQUAL        | 4        |
| LESS             | 5        |
| LESS_OR_EQUAL    | 6        |
| GREATER          | 7        |
| GREATER_OR_EQUAL | 8        |
| START_WITH       | 9        |
| NOT_START_WITH   | 10       |
| CONTAIN          | 11       |
| NOT_CONTAIN      | 12       |
| END_WITH         | 13       |
| NOT_END_WITH     | 14       |
| EXISTS           | 15       |
| NOT_EXISTS       | 16       |
| INNER_JOIN       | 17       |
| LEFT_EXCLUSIVE   | 18       |

### BPMSoft.LogicalOperatorType

| AND | 0   |
| --- | --- |
| OR  | 1   |


### BPMSoft.MessageBoxButtons
1. CANCEL: {className: 'BPMSoft.Button', caption: 'Отмена', markerValue: 'Отмена', returnCode: 'cancel'}
2. CLOSE: {className: 'BPMSoft.Button', caption: 'Закрыть', markerValue: 'Закрыть', returnCode: 'close', style: 'orange'}
3. NO: {className: 'BPMSoft.Button', caption: 'Нет', markerValue: 'Нет', returnCode: 'no'}
4. OK: {className: 'BPMSoft.Button', caption: 'ОК', markerValue: 'ОК', returnCode: 'ok', style: 'orange'}
5. SAVE: {className: 'BPMSoft.Button', caption: 'Сохранить', markerValue: 'Сохранить', returnCode: 'save'}
6. YES: {className: 'BPMSoft.Button', caption: 'Да', markerValue: 'Да', returnCode: 'yes'}

### BPMSoft.AggregationType

| NONE  | 0   |
| ----- | --- |
| COUNT | 1   |
| SUM   | 2   |
| AVG   | 3   |
| MIN   | 4   |
| MAX   | 5   |
### BPMSoft.AggregationEvalType

| NONE     | 0   |
| -------- | --- |
| ALL      | 1   |
| DISTINCT | 2   |


addAggregationSchemaColumn(column, aggType, alias, AggregationEvalType)