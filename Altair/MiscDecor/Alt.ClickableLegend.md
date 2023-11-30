#Alt_SolutionsDecor

[Interactive Charts — Vega-Altair 5.2.0 documentation (altair-viz.github.io)](https://altair-viz.github.io/user_guide/interactions.html#selection-targets)
![[Pasted image 20231130170941.png]]

```
selection = alt.selection_point(fields=['Origin'])
color = alt.condition(
    selection,
    alt.Color('Origin:N').legend(None),
    alt.value('lightgray')
)

scatter = alt.Chart(cars).mark_point().encode(
    x='Horsepower:Q',
    y='Miles_per_Gallon:Q',
    color=color,
    tooltip='Name:N'
)

legend = alt.Chart(cars).mark_point().encode(
    alt.Y('Origin:N').axis(orient='right'),
    color=color
).add_params(
    selection
)

scatter | legend
```