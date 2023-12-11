#TransformCalculate 

### Concat string with field [go](https://stackoverflow.com/questions/65591202/how-to-combine-text-and-a-field-value-in-a-single-line-of-display)
```json
{
  mark:
    {
      type: "text",
      align: "center",
      fontSize: 40,
      fontWeight: "bold"
    },
  transform:
    [
      {"calculate": "'The yield is ' + datum.Yield + '%'", "as": "annotated_yield"}
    ],
  encoding: 
    {
      "text": {"field": "annotated_yield", "type": "nominal"}
    }
}
```