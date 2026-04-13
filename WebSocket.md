
[WebSocket](https://edu.bpmsoft.ru/baza-znaniy/peredacha-soobshcheniy/websocket/?sphrase_id=275954&version=1.4)

Для публикации сообщения по WebSocket используются методы класса BPMSoft.Configuration.MsgChannelUtilities:

- PostMessage(UserConnection userConnection, string senderName, string messageText) — для отправки сообщения определённому пользователю;
- PostMessageToAll(string senderName, string messageText) — для отправки сообщения всем пользователям.
Где «userConnection» — пользовательское подключение, «senderName» — имя сообщения, «messageText» — текст сообщения.


```C#
// Получение ФИО Контакта.
var name = Entity.GetTypedColumnValue<string>("Name");
// Получение даты рождения Контакта.
var birthDate = Entity.GetTypedColumnValue<DateTime>("BirthDate");
// Формирование текста сообщения.
var messageText = "{\"name\": \"" + name + "\", \"birthday\": \"" + birthDate.ToString("dd.MM.yyyy") + "\"}";
// Присвоение сообщению имени.
var sender = "NewMessage";
// Публикация сообщения по WebSocket.
MsgChannelUtilities.PostMessageToAll(sender, messageText);
return true;
```

На клиентской части необходимо переопределить клиентский модуль ClientMessageBridge

```JS
define("ClientMessageBridge", [], function() {
	return {
		/** 
		 * Сообщения схемы. 
		 */
		messages: {
			/* Имя сообщения. */
			"NewMessage": {
				/* Тип сообщения — широковещательное, без указания конкретного подписчика. */
				"mode": BPMSoft.MessageMode.BROADCAST,
				/* Направление сообщения — публикация. */
				"direction": BPMSoft.MessageDirectionType.PUBLISH
			}
		},
		methods: {
			/**
			 * Инициализация схемы. 
			 */
			init: function() {
				/* Вызов родительского метода. */
				this.callParent(arguments);
				/* Добавление нового конфигурационного объекта в коллекцию конфигурационных объектов. */
				this.addMessageConfig({
					/* Имя сообщения, получаемого по WebSocket. */
					sender: "NewMessage",
					/* Имя сообщения, с которым оно будет разослано. */
					messageName: "NewMessage"
				});
			},
			/**
			 * Метод, выполняемый после публикации сообщения.
			 * @param {sandboxMessageName} Имя сообщения, с которым оно было разослано.
			 * @param {webSocketBody} Содержимое сообщения.
			 * @param {result} Результат отправки сообщения.
			 * @param {publishConfig} Конфигурационный объект рассылки сообщения.
			 */
			afterPublishMessage: function(sandboxMessageName, webSocketBody, result, publishConfig) {
				/* Проверка, что сообщение соответствует добавленному в конфигурационный объект. */
				if (sandboxMessageName === "NewMessage") {
					/* Сохранение содержимого в локальные переменные. */
					var name = webSocketBody.name;
					var birthday = webSocketBody.birthday;
					/* Вывод содержимого в консоль браузера. */
					window.console.info("Опубликовано сообщение: " + sandboxMessageName +
										". Данные: ФИО: " + name +
										"; Дата рождения: " + birthday);
				}
			}
		}
	};
});
```

Свойства «ClientMessageBridge»

|Свойство|Тип|Описание|
|---|---|---|
|WebSocketMessageConfigs|Array|Коллекция сконфигурированных объектов сообщений|
|LocalStoreName|String|Имя хранилища для хранения истории сообщений|
|LocalStore|BPMSoft.LocalStore|Экземпляр класса, предоставляющий доступ к локальному хранилищу|
Методы «ClientMessageBridge»

| Параметр метода                                                                                                                                                  | Тип параметра | Описание параметра                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **init()** – инициализация схемы                                                                                                                                 |               |                                                                                                                                                                          |
| **getSandboxMessageListenerExists(sandboxMessageName)** – проверка наличия обработчика сообщения. Возвращает «Boolean» - результат наличия обработчика сообщения |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| **publishMessageResult(sandboxMessageName, webSocketMessage)** – выполнение публикации сообщения. Возвращает результат, полученный от «Listener»                 |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| webSocketMessage                                                                                                                                                 | Object        | Объект сообщения полученного по WebSocket                                                                                                                                |
| **beforePublishMessage(sandboxMessageName, webSocketBody, publishConfig)** – обработка сообщения перед публикацией                                               |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| webSocketBody                                                                                                                                                    | Object        | Объект сообщения полученного по WebSocket                                                                                                                                |
| publishConfig                                                                                                                                                    | Object        | Объект конфигурации рассылки                                                                                                                                             |
| **afterPublishMessage(sandboxMessageName, webSocketBody, result, publishConfig)** – обработка сообщения после публикации                                         |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| webSocketBody                                                                                                                                                    | Object        | Объект сообщения полученного по WebSocket                                                                                                                                |
| result                                                                                                                                                           | *             | Результат публикации                                                                                                                                                     |
| publishConfig                                                                                                                                                    | Object        | Объект конфигурации рассылки                                                                                                                                             |
| **addMessageConfig(config)** – добавление сконфигурированного объекта сообщения в коллекцию                                                                      |               |                                                                                                                                                                          |
| config                                                                                                                                                           | Object        | Конфигурационный объект. Пример:  <br>{  <br>    "sender": "webSocket sender key",  <br>    "messageName": "sandbox message name",  <br>    "isSaveHistory": true  <br>} |
| sender                                                                                                                                                           | String        | Имя сообщения, ожидаемое для получения по WebSocket                                                                                                                      |
| messageName                                                                                                                                                      | String        | Имя сообщения для публикации                                                                                                                                             |
| isSaveHistory                                                                                                                                                    | Boolean       | Признак, сохраняющий сообщение в истории                                                                                                                                 |
| **saveMessageToHistory(sandboxMessageName, webSocketBody)** – сохранение сообщения в истории                                                                     |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| webSocketBody                                                                                                                                                    | Object        | Объект сообщения полученного по WebSocket                                                                                                                                |
| **getMessagesFromHistory(sandboxMessageName)** – получение коллекции сохраненных в истории сообщений                                                             |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
| **deleteSavedMessages(sandboxMessageName)** – удаление сообщения из истории                                                                                      |               |                                                                                                                                                                          |
| sandboxMessageName                                                                                                                                               | String        | Имя сообщения                                                                                                                                                            |
