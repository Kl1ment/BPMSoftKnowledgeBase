Список доступных для управления функций находится в таблице **Features** базы данных приложения. По умолчанию в этой таблице записи отсутствуют.

Информация о состоянии функциональности (активна/неактивна) находится в таблице **AdminUnitFeatureState** в поле **FeatureState**. Таблица AdminUnitFeatureState предназначена также для связи таблиц Feature и SysAdminUnit, в которой определены пользователи и группы пользователей приложения.

Для управления функциональностью для текущего пользователя предусмотрена особая страница «FeaturesPage», которая используется для точечного управления функциональностью приложения при разработке и тестировании. Для перехода на страницу управления функциональностью необходимо перейти на страницу:

1. Для приложения на базе .NET 8:  
    [BPMSoftApp]/Nui/ViewModule.aspx#BaseSchemaModuleV2/FeaturesPage.
2. Для приложения на базе .NET Framework:  
    [BPMSoftApp]/0/Nui/ViewModule.aspx#BaseSchemaModuleV2/FeaturesPage.

#### Скрипт активации/деактивации фичи для всех пользователей
```SQL
DECLARE 
@featureCode varchar(max) = 'FeatureCode',
@featureId uniqueidentifier,
-- Все пользователи портала.
@user1 uniqueidentifier = 'A29A3BA5-4B0D-DE11-9A51-005056C00008',
-- Все сотрудники.
@user2 uniqueidentifier = '720B771C-E7A7-4F31-9CFB-52CD21C3739F';
 
SET @featureId = (SELECT TOP 1 Id FROM Feature WHERE Code = @featureCode);

IF @featureId is null
BEGIN
	INSERT INTO Feature
	(Name, Code)
	VALUES
	(@featureCode, @featureCode);
	SET @featureId = (SELECT TOP 1 Id FROM Feature WHERE Code = @featureCode);
END;
DELETE FROM AdminUnitFeatureState WHERE FeatureId = @featureId;
INSERT INTO AdminUnitFeatureState
(SysAdminUnitId, FeatureState, FeatureId)
VALUES
(@user1, 1, @featureId),
(@user2, 1, @featureId);
```

Проверка состояния функциональности на front-end
```JS
if (BPMSoft.Features.getIsEnabled("TestFeature")) {
	...
}
```

Проверка состояния функциональности на back-end
```C#
if (UserConnection.GetIsFeatureEnabled("TestFeature"))
{
	...
}
```
