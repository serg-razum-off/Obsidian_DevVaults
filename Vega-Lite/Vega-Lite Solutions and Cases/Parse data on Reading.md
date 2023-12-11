
##### Parse as numbers example [go](https://stackoverflow.com/questions/60837660/issues-transforming-string-to-date-vega-lite#:~:text=%22parse%22:%20{)
```
{
  "$schema": "https://vega.github.io/schema/vega-lite/v4.json",
  "data": {
    "url": "https://raw.githubusercontent.com/DanStein91/Info-vis/master/CoffeeRobN.csv",
    "format": {
      "type": "csv",
            "parse": {
       "Number_of_Bags": "number",
        "Bag_weight": "number",
        "Harvest_Year": "number"
      }
    }
  },
```
