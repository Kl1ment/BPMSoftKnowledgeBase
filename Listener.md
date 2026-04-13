https://edu.bpmsoft.ru/baza-znaniy/sobytiynyy-sloy/vozmozhnosti-sobytijnogo-sloya/?version=1.4&#id-4

| Метод-обработчик                                    | Описание метода                              |
| --------------------------------------------------- | -------------------------------------------- |
| OnDeleting(object sender, EntityBeforeEventArgs e)  | Обработчик события перед удалением записи.   |
| OnDeleted(object sender, EntityAfterEventArgs e)    | Обработчик события после удаления записи.    |
| OnInserting(object sender, EntityBeforeEventArgs e) | Обработчик события перед добавлением записи. |
| OnInserted(object sender, EntityAfterEventArgs e)   | Обработчик события после добавления записи.  |
| OnSaving(object sender, EntityBeforeEventArgs e)    | Обработчик события перед сохранением записи. |
| OnSaved(object sender, EntityAfterEventArgs e)      | Обработчик события после сохранения записи.  |
| OnUpdating(object sender, EntityBeforeEventArgs e)  | Обработчик события перед обновлением записи. |
| OnUpdated(object sender, EntityAfterEventArgs e)    | Обработчик события после обновления записи.  |

```C#
/* Слушатель событий сущности "Контакт". */
[EntityEventListener(SchemaName = "Contact")]
public class ContactEntityEventListener : BaseEntityEventListener
{
	/* Переопределение обработчика события после сохранения сущности.*/
	public override void OnSaved(object sender, EntityAfterEventArgs e) 
	{
		/* Вызов родительской реализации. */
		base.OnSaved(sender, e);
		
		/* Получение записи. */
		var entity = (Entity)sender;
		
		/* Получение UserConnection. */
		var userConnection = entity.UserConnection;
	}
}
```

```C#
[EntityEventListener(SchemaName = "Opportunity")]
public class UsrOpportunityEntityEventListener : BaseEntityEventListener
{
    private ILog _log = LogManager.GetLogger<UsrOpportunityEntityEventListener>();

    public override void OnUpdated(object sender, EntityAfterEventArgs e)
    {
        base.OnUpdated(sender, e);

        var opportunity = (Opportunity)sender;

        var userConnection = opportunity.UserConnection;

        var modifiedColumnValues = e.ModifiedColumnValues;

        var account = modifiedColumnValues.GetByName(nameof(Opportunity.AccountId));

        if (account != null)
        {
            _log.Info($"Opportunity.Account Old Value - {account.OldValue}; New Value {account.Value}");
        }
    }
}
```