- parse_dates
```
	df = pd.read_csv('filename.csv', parse_dates=['date'])
```
- parse_dates + parser
```
date_parser = lambda x: datetime.strptime(x, '%Y-%m-%d')
df = pd.read_csv('filename.csv', parse_dates=['date'], date_parser=date_parser)
```
