
```JS
messages: {
	"messageName": {
		mode: BPMSoft.MessageMode.PTP,
		direction: BPMSoft.MessageDirectionType.PUBLISH
	},
}
```

BPMSoft.MessageDirectionType

| BROADCAST | "broadcast" |
| --------- | ----------- |
| PTP       | "ptp"       |

BPMSoft.MessageDirectionType

| BIDIRECTIONAL | "bidirectional" |
| ------------- | --------------- |
| PUBLISH       | "publish"       |
| SUBSCRIBE     | "subscribe"     |


```JS
let path = BPMSoft.combinePath(cardModule, cardSchema, "edit", value);
let historyStateConfig = { hash: path };
this.sandbox.publish("ReplaceHistoryState", historyStateConfig);
```

```JS
this.sandbox.publish("PushHistoryState",
	this.getStateObject(objectRecordId, schemaName + "Page"))
```

```JS
this.sandbox.publish("BackHistoryState");
```

NavigationModule
```JS
sandbox.subscribe("PushHistoryState", onPushHistoryState);
sandbox.subscribe("ReplaceHistoryState", onReplaceHistoryState);
sandbox.subscribe("GetHistoryState", onGetHistoryState);
sandbox.subscribe("BackHistoryState", onBackHistoryState);
sandbox.subscribe("ForwardHistoryState", onForwardHistoryState);
```

