```awk
@include "ape"
@include "tab"

BEGIN {tests("tabok","_tab")} 

function _tab(f,    i) {
  Tab(i)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
  ok(f,length(i.rows)==398)
  ok(f ":3", i.cols[1].seen[3] == 4)
  ok(f ":8", i.cols[1].seen[8] == 103)
}

```
