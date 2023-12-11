#VegaLite_Solutions_Decor
```
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "mark": {"type": "text", "baseline": "middle", "align": "left"},
  "encoding": {
    "x": {"field": "Start Date", "type": "temporal"},
    "y": {"field": "End Date", "type": "temporal"},
    "text": {"field": "txt_merged", "type": "nominal"}
  },
 
  "transform": [
    {
      "calculate": "'start date: ' + timeFormat(datum['Start Date'], '%Y-%m-%d')",
      "as": "start_date"
    },
    {
      "calculate": "'end date: ' + timeFormat(datum['End Date'], '%Y-%m-%d')",
      "as": "end_date"
    },
    {
      "calculate": "[datum['start_date'], datum['end_date']]",
      "as": "txt_merged"
    }
  ],
  "data": {"values": [
      {"Start Date": "2023-01-01", "End Date": "2023-01-15", "Title": "Project A"},
      {"Start Date": "2023-02-01", "End Date": "2023-02-28", "Title": "Project B"},
      {"Start Date": "2023-03-01", "End Date": "2023-03-31", "Title": "Project C"}
  ]}
}

```