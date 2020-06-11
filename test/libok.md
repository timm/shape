

```awk
@include "ape"

BEGIN {  tests("libok","_list") }

function _list(    a) {
  copy(THE,"",a)
  a.tab.p = 10
  oo(THE,"the")
  oo(a,"a")
}


```
