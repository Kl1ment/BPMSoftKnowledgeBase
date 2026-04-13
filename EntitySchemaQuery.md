
### Select

```JS
const recordId = this.get("Id");
let esq = this.Ext.create("BPMSoft.EntitySchemaQuery", {
    rootSchemaName: "Account"
});
esq.addColumn("PrimaryContact.Name", "PrimaryContactName");
esq.getEntity(recordId, function(result) {
    if (!result.success) {
        BPMSoft.showErrorMessage("Ошибка запроса данных");
        return;
    }
    const contactName = result.entity.get("PrimaryContactName");
    BPMSoft.showInformation(contactName);
}, this);
```

```JS
Ext.create("BPMSoft.FilterGroup")
BPMSoft.createFilterGroup()
BPMSoft.createColumnFilterWithParameter(BPMSoft.ComparisonType.EQUAL,
				"IsAllowedToDelegate", true)
BPMSoft.createColumnIsNullFilter("VisaOwner")
BPMSoft.createColumnInFilterWithParameters("Id", items)
```

```JS
let esq = Ext.create("BPMSoft.EntitySchemaQuery", {
    rootSchemaName: "Account"
});
// Вместо метода getEntityCollection(callback, scope) можно также использовать метод execute(callback, scope)
esq.getEntityCollection(function (result) {
    if (!result.success) {
        BPMSoft.showErrorMessage("Ошибка запроса данных");
        return;
    }
    BPMSoft.showInformation("Количество записей в разделе «Контрагент»: " + result.collection.getCount());
}, this);
```

```JS
let esq = Ext.create("BPMSoft.EntitySchemaQuery", {
    rootSchemaName: "Contact"
});
esq.addColumn("Name");
esq.addColumn("Country.Name", "CountryName");
 
const countryFirstFilter = esq.createColumnFilterWithParameter(BPMSoft.ComparisonType.EQUAL, "Country.Name", "Россия");
 
const countrySecondFilter = esq.createColumnFilterWithParameter(BPMSoft.ComparisonType.EQUAL, "Country.Name", "Беларусь");
 
esq.filters.logicalOperation = BPMSoft.LogicalOperatorType.OR;
  
esq.filters.addItem(countryFirstFilter);
esq.filters.addItem(countrySecondFilter);
 
esq.getEntityCollection(function (result) {
    if (result.success) {
        result.collection.each(function (item) {
            // Обработка элементов коллекции.
        });
    }
}, this);
countrySecondFilter.isEnabled = false;
  
esq.getEntityCollection(function (result) {
    if (result.success) {
        result.collection.each(function (item) {
            // Обработка элементов коллекции. 
        });
    }
}, this);
```

### Select
```C#
var esq = new EntitySchemaQuery(_userConnection.EntitySchemaManager, entitySchemaName);

esq.AddAllSchemaColumns();

//esq.Filters.Add(esq.CreateFilterWithParameters(FilterComparisonType.Equal, "Id", recordId));

var file = esq.GetEntity(_userConnection, recordId);
```

### Insert

```C#
public void CreateActivity()
{
	EntitySchema activitySchema = _userConnection.EntitySchemaManager.GetInstanceByName("Activity");
	Entity activityEntity = activitySchema.CreateEntity(_userConnection);
	activityEntity.SetDefColumnValues();
	activityEntity.SetColumnValue("Title", "Тестовая активность");
	activityEntity.Save();
}
```

### Delete

```C#
public bool DeleteActivityById(Guid activityId)
{
	var esqResult = new EntitySchemaQuery(_userConnection.EntitySchemaManager, "Activity");
	var entity = esqResult.GetEntity(_userConnection, activityId);
	entity.Delete();
	var result = entity.ExistInDB(activityId);
	return result;
}
```

BPMSoft.AggregationType.COUNT
esq.addAggregationSchemaColumn("GroupColumn", BPMSoft.AggregationType, "AggAlias", BPMSoft.AggregationEvalType)


GetCanUseFileApi
LoadFileUsingFileApi


ALTER TABLE TestDb.dbo.[File] DROP CONSTRAINT DFYEJgCd3o1CdT5kRWzr7IpHVoE;

ALTER TABLE TestDb.dbo.[File] DROP COLUMN SmFullS3Link;

ALTER TABLE TestDb.dbo.[File] DROP CONSTRAINT DFhITY361XRuu68RSNXQR8zRJH87M;

ALTER TABLE TestDb.dbo.[File] DROP COLUMN SmS3Link;


```C#
var esq = new EntitySchemaQuery(_userConnection.EntitySchemaManager, nameof(UsrConnectionParameters1C))
{
    UseAdminRights = false,
    PrimaryQueryColumn = { IsAlwaysSelect = true },
    RowCount = 1
};
```