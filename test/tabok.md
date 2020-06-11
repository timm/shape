```awk
@include "ape"
@include "tab"

BEGIN {tests("tabok","_num")} 
function _tab(    i) {
  Tab(i)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
}

BEGIN { _tab() }
```
