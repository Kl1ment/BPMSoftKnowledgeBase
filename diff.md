```JS
{
	"operation": "insert"/"move"/"remove"/"merge",
	"parentName": "LeftContainer",
	"propertyName": "items",
	"name": "UsrPreviousRecordButton",
	"values": {
		"itemType": BPMSoft.ViewItemType.BUTTON,
		"click": { "bindTo": "closeRecord" },
		"tag": "Close",
		"classes": {
			// класс для обертки
			"wrapperClass": "notification-image-wrapper",
			// класс для картинки
			"imageClass": ["usr-standard-img-btn"],
			"labelClass": ["", ""],
			"textClass": [""],
			"wrapClass": [""],
			"hyperlinkClass": [""],
			"menuClass": [""],
			"wrapClassName": [""]
		},
		"wrapClass": ["right-container-wrapClass"],
		"linkWrapClass": ["org-structure-unit-filter"],
		"style": BPMSoft.controls.ButtonEnums.style.TRANSPARENT,
		"imageConfig": { "bindTo": "Resources.Images.UsrCloseRecordButtonIcon" },
		"visible": { "bindTo": "IsUsrCloseRecordButtonVisible" },
		"linkclick": { "bindTo": "openLookupLinkBlank" }, // Привязка клика по ссылке к методу
	},
	"index": 1
},
```

```JS
{
	"operation": "insert",
	"name": "accountSfaContainer",
	"parentName": "remainsFiltersContainer",
	"propertyName": "items",
	"index": 10,
	"values": {
	  "id": "accountSfaContainer",
	  "itemType": BPMSoft.ViewItemType.CONTAINER,
	  "wrapClass": ["filters-container-item"],
	  "items": []
	}
},
{
	"operation": "insert",
	"parentName": "accountSfaContainer",
	"propertyName": "items",
	"name": "Account",
	"values": {
	  "itemType": BPMSoft.ViewItemType.COMPONENT,
	  "className": "BPMSoft.CfxMultiselectEdit",
	  "lookupEntity": "Account",
	  "value": { "bindTo": "Account"},
	  "caption": "Контрагент"
	}
},
```