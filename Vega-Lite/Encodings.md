#VegaLiteEncoding

- facet (small multiples) [pbi-G](https://youtu.be/I6FZYTSKI6Y?list=PL6oIJxyQvMGTxh4tREeKflcKVlOfGdyim&t=84)
	- field 
	- type
	- column (req: num of columns to split)
		- 1 if you want to see charts split one under another
	- these 2 have to B specified additionaly: 
		- width 
		- height 
	- sort is optional:
```
	"sort": {"op":sum, "field":"<>"}
```