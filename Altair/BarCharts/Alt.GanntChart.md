#Alt_SolutionsBar

### Base Gannt Chart

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

### Gannt with overlapping text
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
### Gannt with Project selection and Today() ruler
```
import altair as alt

from datetime import datetime

  

brush = alt.selection_point(fields=['Project'])

  

base = alt.Chart(df)

  

chart = (

    base.mark_bar(size=30)

    .encode(

        alt.X('Target start date'),

        alt.X2('Target end date'),

        alt.Y('Project', sort=alt.EncodingSortField(field='Importance', order="descending")),

        tooltip=['Title', 'Target start date', 'Target end date', 'PhaseLength:Q', 'Importance'],

        color=alt.condition(brush, alt.Color('Title:N', legend=None), alt.value('lightgray'))  

    )

    .properties(height=alt.Step(32))

    .add_params(brush)

)

  

rule = alt.Chart().mark_rule(color='red', strokeDash=[10,3]).encode(

    x=alt.datum(alt.DateTime(

        year=datetime.now().year,

        month=datetime.now().month,

        date=datetime.now().day))

    )

  

(chart + rule).properties(title="Діаграма Ганнта по проєктах ", width=1050).configure_title(fontSize=18).interactive()
```
### Gannt with Project selection, Today() ruler and text for Today's date 
```
import altair as alt

from datetime import datetime

  

brush = alt.selection_point(fields=['Project'])

  

base = alt.Chart(df)

  

bar_chart = (

    base.mark_bar(size=30)

    .encode(

        alt.X('Target start date'),

        alt.X2('Target end date'),

        alt.Y('Project', sort=alt.EncodingSortField(field='Importance', order="descending")),

        tooltip=['Title', 'Target start date', 'Target end date', 'PhaseLength:Q', 'Importance'],

        color=alt.condition(brush, alt.Color('Title:N', legend=None), alt.value('lightgray'))  

    )

    .properties(height=alt.Step(32))

    .add_params(brush)

)

  

today_line = alt.Chart().mark_rule(color='red', strokeDash=[10,3]).encode(

    x=alt.datum(alt.DateTime(

        year=datetime.now().year,

        month=datetime.now().month,

        date=datetime.now().day))

    )

  

text = alt.Chart().mark_text(

    align='right',

    baseline='middle',

    dx=25,  # adjust this value to position the text

    text="-".join([str(datetime.now().year), str(datetime.now().month), str(datetime.now().day)])

).encode(

    x=alt.datum(alt.DateTime(

        year=datetime.now().year,

        month=datetime.now().month,

        date=datetime.now().day)),

    y=alt.value(20),  # adjust this value to position the text vertically

)

  

background = alt.Chart().mark_rect(

    color='white',

    width=60,  

    height=15,

    opacity=.6,

    cornerRadius=20,

    stroke="grey",

    strokeWidth=3

).encode(

    x=alt.datum(alt.DateTime(

        year=datetime.now().year,

        month=datetime.now().month,

        date=datetime.now().day)),

    y=alt.value(20)  # adjust this value to position the background vertically

    ,

)

  

result_chart = (bar_chart + today_line + background + text ).properties(title="Діаграма Ганнта по проєктах ", width=1050).configure_title(fontSize=18).interactive()

  

result_chart
```