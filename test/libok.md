

```awk
@include "ape"

BEGIN {  tests("libok","_list,_copy") }

function _list(f,    a) {
  copy(THE,a)
  ok(f, a.tab.p == 2)
}

function _copy(f,    a,b) {
  a.now.here.one   = 111
  a.now.here.two   = 112
  a.now.there.one  = 121
  a.now.there.two  = 122
  a.then.here.one  = 211
  a.then.here.two  = 212
  a.then.there.one = 221
  a.then.there.two = 222 
  copy(a, b)
  b.here.now.one = "here_now_one"
  ok(f, a.now.here.one == 111)
  ok(f, b.here.now.one == "here_now_one")
}
```

