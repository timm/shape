
```awk
@include "tabs"

function Bore(i,f) {
  has(i,"tabs","Tabs")
  TabsRead(i.tabs, f)
  BoreRead(i)
}
function BoreRead(i,   x,y) {
  y = i.tabs.all.the.klass
  for(x in i.tabs.all.the.nums)
    if (x in i.tabs.all.the.x)  
      BoreRead1(i,x,y)
}
function BordRead1(i,x,y,  k) {
  for(k in i.tabs)
    print(k,x,y)
}
```
