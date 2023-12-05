##### Axis Duplication
this is some dicts that are placed on the same level with "encoding":
- resolve
	- can B used for duplication of axes on the small multiples chart
```
	"resolve": {"axis":{"x":"independent"}}
```
- repeating axis [pbi-G](https://youtu.be/I6FZYTSKI6Y?list=PL6oIJxyQvMGTxh4tREeKflcKVlOfGdyim&t=478)
	- no width and hight
```
	"repeat":["measure1", "measure2","measure3 "],
	"columns": 1,
	"spec": {
		"mark": {...},
		"encoding": {"x":{...}, 
		"y":{"field": 
				{"repeat":"repeat"}, 
				"aggregate":"sum"
			} },
```
![[Pasted image 20231119212657.png]]

##### Axis labels
###### Labels off
| code | result |
| ---- | ------ |
| ![[Pasted image 20231205173704.png]]     |  axis without label      |
