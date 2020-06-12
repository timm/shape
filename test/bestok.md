```awk
@include "best"

BEGIN {tests("bestok","_best")} 

function _best(f,    i,t) {
  Tab(t)
  TabRead(t,"data/raw/auto93" APE.dot "csv") 
  Best(i,t)
}

```
