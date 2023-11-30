#Alt_SolutionsBar

# Base Gannt Chart

```
(

    alt.Chart(df,   title="Діаграма Ганнта по проєктах ЦМТР", width=950).mark_bar(size=30)

    .encode(

        alt.X('Target start date'),

        alt.X2('Target end date'),

        alt.Y('Project', sort=alt.EncodingSortField(field='Importance', order="descending")),

        alt.Color('Title:N', legend=None),

        tooltip=['Title', 'Target start date', 'Target end date', 'PhaseLength:Q', 'Importance']

    )

    .properties(height=alt.Step(35))

    .configure_title(fontSize=18)

    .interactive()

)
```

# Gannt with overlapping text
```
bar_chart = (

    alt.Chart(df,   title="Діаграма Ганнта по проєктах ЦМТР з текстом", width=1050, height=alt.Step(35))

    .mark_bar(size=40)

    .encode(

        alt.X('Target start date'),

        alt.X2('Target end date'),

        alt.Y('Project', sort=alt.EncodingSortField(field='Importance', order="descending")),

        alt.Color('Title:N', legend=None),

        tooltip=['Title', 'Target start date', 'Target end date', 'PhaseLength:Q', 'Importance']

    )

    .interactive()

)

  

text_layer = (

    alt.Chart(df)

        .mark_text(align='left', baseline='middle', dx=1, fontSize=15)

        .transform_filter(alt.datum.PhaseLength >= 0)

        .encode(

            alt.X('Target start date'),

            alt.Y('Project', sort=alt.EncodingSortField(field='Importance', order="descending")),

            alt.Text('Title')

))

  

(bar_chart + text_layer).configure_title(fontSize=18)
```