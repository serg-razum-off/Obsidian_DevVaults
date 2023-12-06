
# Selection in VL Docs
[go](https://vega.github.io/vega-lite/docs/selection.html)

## Partial implementations
##### Brush -- on hover
if not selected, default size
```
"params": {
          "name":"brush",
          "select": {"type": "point", "on":"mouseover"}
        }
"encoding": {
"size":{
          "condition":{
            "param": "brush",
            "empty": false,
            "value": 27
          },
          "value": 17
        }
}
```

##### Select on double click [doca](https://vega.github.io/vega-lite/docs/parameter.html#config)
```
For instance, setting `point` to `{"on": "dblclick"}` populates point selections on double-click by default.
```

##### Clear Selection [doca](https://vega.github.io/vega-lite-v4/docs/clear.html)
```
`false` – disables clear behavior;
```
Other events for clearing selection:
```
"selection": { 
	"brush": { 
		"type": "interval", 
		"init": {"x": [55, 160], "y": [13, 37]}, 
		"clear": "mouseup" 
		}
```
