```awk
@include "ape"
@include "tab"

BEGIN {tests("tabok","_tab,_rows")} 

function _tab(f,    i) {
  Tab(i)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
  ok(f,length(i.rows)==398)
  ok(f ":3", i.cols[1].seen[3] == 4)
  ok(f ":8", i.cols[1].seen[8] == 103)
}

function _rows(f,    m,n,i,some,r1,r2,rows,a) {
  Tab(i)
  List(rows)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
  m = 64
  while(m--)  
    some[ any(i.rows) ]
  for(r1 in some) {
    n = TabAround(i, r1, some, i.my.x, a)
    ok(f r1, a[1].dist < a[n].dist)
  }
}
```
