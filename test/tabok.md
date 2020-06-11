```awk
@include "tab"

function _tab(    i) {
  Tab(i)
  TabRead(i,"data/raw/auto93-10000" APE.dot "csv") 
}

BEGIN { _tab() }
```
