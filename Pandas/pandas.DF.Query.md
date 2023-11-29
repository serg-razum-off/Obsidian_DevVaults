| method                    | examples                                                                                                                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| .qwr                      | searchPhrase = 'your_search_phrase' <br> result_df = df_calendars.query("subject.str.contains(@searchPhrase, case=False, na=False)", engine='python') |
| .qwr no NaN               | df.query("Project == 'Рейтинг' & `Target start date`.notna()")  <br>             *use `` to enclose column Name in .query*                              |
| .qwr --list of conditions | qwr = ["Project.str.contains('СЕДО')", "`Target start date` <= '2023-09-22'"] <br>  df.query(" and ".join(qwr)).sort_values(by='Target start date',ascending=True)                                                                                                                                                    |



