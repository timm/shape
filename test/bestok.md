```awk
@include "best"

BEGIN {tests("bestok","_best")} 

function _best(f,    i,j,r,t) {
  Tab(t)
  TabRead(t,"data/raw/auto93" APE.dot "csv") 
  Best(i, t)
  print o(t.header), ", !best"
  for(r in i.rest) print  o(t.rows[r]) ", false" 
  for(r in i.best) print  o(t.rows[r]) ", true" 
}

```
