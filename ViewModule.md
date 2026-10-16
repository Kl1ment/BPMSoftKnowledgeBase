```JS
define("EntityConfiguration", ["ext-base", "BPMSoft", "EntityConfigurationResources", "BaseViewModule", "BpmObjectView"], function(Ext, BPMSoft, resources) {
	
	Ext.define("BPMSoft.configuration.EntityConfiguration", {
		extend: "BPMSoft.BaseViewModule",
		alternateClassName: "BPMSoft.EntityConfiguration",
		
		objectViewComponent: null,
		
		init: function() {
			this.callParent(arguments);
			this.objectViewComponent = document.createElement("bpm-object-view");
		},
		
		render: function(renderTo) {
			this.callParent(arguments);
			this.renderObjectViewComponent(this);
		},
		
		renderObjectViewComponent: function(scope) {
			var dataContainer = document.querySelector("div[id=MainContainer]");
			if(dataContainer && dataContainer.appendChild) {
				dataContainer.appendChild(scope.objectViewComponent);
			} else {
				setTimeout(scope.renderGanttComponent, 500, scope);
			}
		},
		
		loadMainPanelsModules: BPMSoft.emptyFn,
		
		getMessages: function() {
			const parentMessages = this.callParent(arguments) || {};
			return this.Ext.apply(parentMessages, {
				"LoadModule": {
					mode: BPMSoft.MessageMode.PTP,
					direction: BPMSoft.MessageDirectionType.SUBSCRIBE
				},
				"HistoryStateChanged": {
					mode: BPMSoft.MessageMode.BROADCAST,
					direction: BPMSoft.MessageDirectionType.SUBSCRIBE
				},
				"RefreshCacheHash": {
					mode: BPMSoft.MessageMode.PTP,
					direction: BPMSoft.MessageDirectionType.SUBSCRIBE
				},
				"NavigationModuleLoaded": {
					mode: BPMSoft.MessageMode.BROADCAST,
					direction: BPMSoft.MessageDirectionType.SUBSCRIBE
				},
				"GetHistoryState": {
					mode: BPMSoft.MessageMode.PTP,
					direction: BPMSoft.MessageDirectionType.PUBLISH
				}
			});
		},
		
		diff: [
			{
				"operation": "insert",
				"name": "MainContainer",
				"values": {
					"itemType": BPMSoft.ViewItemType.CONTAINER,
					"id": "MainContainer",
					"items": []
				}
			},
		]
	});
	
	return BPMSoft.EntityConfiguration;
});
```


```JS
define("SystemDesigner", [], function() {
	return {
		methods: {
			navigateToEntityConfiguration: function() {
				var importSessionId = this.BPMSoft.generateGUID();
				var url = this.BPMSoft.combinePath(this.BPMSoft.workspaceBaseUrl, "Nui",
												   "ViewModule.aspx?vm=EntityConfiguration");
				setTimeout(function() {
					window.open(url, "_blank");
				}, 0);
			},
		},
		diff: [
			{
				"operation": "insert",
				"index": 0,
				"propertyName": "items",
				"parentName": "ConfigurationTile",
				"name": "EntityConfiguration",
				"values": {
					"itemType": BPMSoft.ViewItemType.LINK,
					"caption": {"bindTo": "Resources.Strings.EntityConfigurationLinkCaption"},
					"tag": "navigateToEntityConfiguration",
					"click": {"bindTo": "invokeOperation"}
				}
			}
		]
	};
});

```