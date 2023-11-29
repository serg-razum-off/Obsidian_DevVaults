#Alt_Solutions

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
