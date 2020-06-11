```awk
@include "tab"

function _tab(    i) {
  Tab(i)
  TabRead(i,"data/raw/weather" APE.dot "csv") 
  oo(i)
}

BEGIN { _tab() }
```
