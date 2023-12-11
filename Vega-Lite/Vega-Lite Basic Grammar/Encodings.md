#VegaLiteEncoding

##### Facet 
- facet (small m
- 
- ultiples) [pbi-G](https://youtu.be/I6FZYTSKI6Y?list=PL6oIJxyQvMGTxh4tREeKflcKVlOfGdyim&t=84)
	- field 
	- type
	- column (req: num of columns to split)
		- 1 if you want to see charts split one under another
	- these 2 have to B specified additionaly: 
		- width 
		- height 
	- sort is optional:
```json
	"sort": {"op":sum, "field":"<>"}
```

##### Tooltips
| issue                                                                                                                                                                                      | pic |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --- |
| Vega-Lite chart with tooltip like in PBI [SOF](https://community.fabric.microsoft.com/t5/Custom-Visuals-Development/Deneb-Line-Chart-Tooltips-that-don-t-require-you-to-hover/m-p/3122408) | ![[Pasted image 20231208144047.png]]    |

###### Code 
```javascript
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "params": [{"name": "dt", "expr": "hover['date']"}],
  "width": 800,
  "height": 400,
  "description": "Google's stock price over time.",
  "data": {"url": "data/stocks.csv"},
  "transform": [{"filter": "datum.symbol==='GOOG'"}],
  "layer": [
    {
      "encoding": {"x": {"field": "date", "type": "temporal"}},
      "layer": [
        {
          "mark": "line",
          "encoding": {"y": {"field": "price", "type": "quantitative"}}
        },
        {
          "params": [
            {
              "name": "hover",
              "select": {
                "type": "point",
                "fields": ["date"],
                "nearest": true,
                "on": "mouseover",
                "clear": "mouseout"
              }
            }
          ],
          "mark": {"type": "rule", "y": 0, "y2": {"expr": "height"}},
          "transform": [{"calculate": "format(datum['price'], '$')", "as": "price"}],
          "encoding": {
            "opacity": {
              "condition": {
                "test": "+hover['date'] === datum['date']",
                "value": 0.5
              },
              "value": 0
            },
            "tooltip": [
              {"title": "Symbol", "field": "GOOG"},
              {"title": "Price", "field": "price"}
            ]
          }
        }
      ]
    }
  ],
  "config": {"axis": {"grid": false}}
}
```