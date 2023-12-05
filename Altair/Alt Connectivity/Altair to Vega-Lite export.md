#### Print Json + Write to File [go](https://altair-viz.github.io/user_guide/internals.html#altair-chart-to-vega-lite-spec)
```
import altair as alt
from vega_datasets import data

chart = alt.Chart(data.cars.url).mark_point().encode(
    x='Horsepower:Q',
    y='Miles_per_Gallon:Q',
    color='Origin:N',
).configure_view(
    continuousHeight=300,
    continuousWidth=300,
)

vl_schema = chart.to_json(indent=2)

with open('./GanntCh.json', "w") as file:

    file.write(vl_schema)
```

#### Save Chart as Json [go](https://joelostblom.github.io/altair-docs/user_guide/saving_charts.html)
```
import altair as alt
from vega_datasets import data

chart = alt.Chart(data.cars.url).mark_point().encode(
    x='Horsepower:Q',
    y='Miles_per_Gallon:Q',
    color='Origin:N'
)

chart.save('chart.json')
```