```JS
define("UsrModalPage", 
	   [ "ModalBox",
		"UsrGrid",
		"ConfigurationGridUtilitiesV2",], 
	   function(ModalBox) {
	return {
		attributes: {
			ColumnPositionCollection: {
				dataValueType: BPMSoft.DataValueType.COLLECTION,
				type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
				value: Ext.create("BPMSoft.BaseViewModelCollection")
			},
		},
		messages: {},
		mixins: {
			ConfigurationGridUtilitiesV2: "BPMSoft.ConfigurationGridUtilitiesV2"
		},
		methods: {
			init: function() {
				this.callParent(arguments);
				this.setGridData();
			},

			setGridData: function() {
				var data = {
					productTypeId: this.$ProductTypeId
				};

				config = {
					serviceName: "",
					methodName: "",
					data: data
				};

				this.callService(config, function(result) {
					var settingsResult = result.GetResult;
					if (settingsResult) {
						this.loadGridDataFromJson(settingsResult.UsrColumnPosition);
					}
				}, this);
			},

			loadGridDataFromJson: function(json) {
				this.$ColumnPositionCollection.reloadAll();
				var collection = JSON.parse(json);
				collection.sort((x, y) => x.Position - y.Position);
				BPMSoft.each(collection, function(columnPosiont) {
					if (!columnPosiont.Id) {
						columnPosiont.Id = BPMSoft.utils.guid.generateGUID();
					}
					var item = this.createItemModel(columnPosiont);
					item.id = item.$Id;
					item.save = BPMSoft.emptyFn
					this.$ColumnPositionCollection.add(item);
				}, this);
			},

			getGridData: function() {
				return this.$ColumnPositionCollection;
			},

			createItemModel: function(values) {
				return Ext.create("BPMSoft.BaseViewModel", {
					columns: this.getColumns(),
					values: values,
					rowConfig: this.getRowConfig(),
				});
			},

			getColumns: function() {
				return {
					Id: {
						dataValueType: BPMSoft.DataValueType.GUID,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "Id",
						enabled: false
					},
					ColumnDisplayName: {
						dataValueType: BPMSoft.DataValueType.TEXT,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "ColumnDisplayName",
						enabled: false
					},
					ColumnName: {
						dataValueType: BPMSoft.DataValueType.TEXT,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "ColumnName",
						enabled: false
					},
					Position: {
						dataValueType: BPMSoft.DataValueType.INTEGER,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "Position",
						enabled: false
					},
					Visible: {
						dataValueType: BPMSoft.DataValueType.BOOLEAN,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "Visible",
						enabled: true
					},
					ColumnType: {
						dataValueType: BPMSoft.DataValueType.TEXT,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						name: "ColumnType",
						enabled: false
					}
				}
			},

			getRowConfig: function() {
				return {
					ColumnDisplayName: {
						dataValueType: BPMSoft.DataValueType.TEXT,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						enabled: false
					},
					Position: {
						dataValueType: BPMSoft.DataValueType.INTEGER,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						enabled: false
					},
					Visible: {
						dataValueType: BPMSoft.DataValueType.BOOLEAN,
						type: BPMSoft.ViewModelColumnType.VIRTUAL_COLUMN,
						enabled: true
					},
				}
			},

			selectRow: function(selectedRowId) { 
				this.set("SelectedRowId", selectedRowId);
				this.set("SelectedRow", this.$ColumnPositionCollection.findByFn(i => i.$Id === selectedRowId));
			},

			getItemData: function(item) {
				return {
					Id: item.$Id,
					ColumnDisplayName: item.$ColumnDisplayName,
					ColumnName: item.$ColumnName,
					Position: item.$Position,
					Visible: item.$Visible,
					ColumnType: item.$ColumnType
				}
			},

			getActiveRow: function() {
				return this.$SelectedRow;
			},

			getColumnByColumnName: function(columnName) {
				return this.getColumnByName(columnName);
			},

			getColumnByName: function(columnName) {
				return this.getColumns()[columnName];
			},
		},
		rules: {},
		diff: [
			{
				"operation": "insert",
				"name": "GridContainer",
				"parentName": "CardContentContainer",
				"propertyName": "items",
				"index": 1,
				"values": {
					"itemType": BPMSoft.ViewItemType.CONTAINER,
					"items": [],
					"wrapClass": ["grid-container"]
				},
			},
			{
				"operation": "insert",
				"name": "ColumnPositionGridData",
				"parentName": "GridContainer",
				"propertyName": "items",
				"index": 0,
				"values": {
					"id": "ColumnPositionGridData",
					"className": "BPMSoft.UsrForecastColumnGrid",
					"generateControlsConfig": {"bindTo": "generateActiveRowControlsConfig"},
					"activeRowAction": {"bindTo": "onActiveRowAction"},
					"multiSelect": false,
					"selectRow": {"bindTo": "selectRow"},
					"itemType": BPMSoft.ViewItemType.GRID,
					"type": BPMSoft.GridType.LISTED,
					"collection": { "bindTo": "ColumnPositionCollection" },
					"captionsConfig": [
						{ "cols": 15, "name": "Колонка" },
						{ "cols": 5, "name": "Позиция" },
						{ "cols": 5, "name": "Видимость" }
					],
					"columnsConfig": [
						{
							"cols": 15,
							"key": [
								{
									"name": { "bindTo": "ColumnDisplayName" },
									"caption": "Колонка"
								}
							]
						},
						{
							"cols": 5,
							"key": [
								{
									"name": { "bindTo": "Position" },
									"caption": "Позиция"
								}
							]
						},
						{
							"cols": 5,
							"key": [
								{
									"name": { "bindTo": "Visible" },
									"caption": "Видимость"
								}
							]
						}
					]
				},
			},
		]
	};
});
```