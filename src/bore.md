
```awk
@include "tabs"

function Bore(i,f) {
  has(i,"tab","Tab")
  TabRead(i.tab, f)
  BoreRead(i,i.tab)
}
function BoreRead(i,t,   x,y) {
  y = t.the.klass
  for(x in t.the.nums)
    if (x in t.the.x)  
      BoreRead1(i,t,x,y)
}
function BoreRead1(i,t,x,y,  xval,a,r,k) {
  for(r in t.rows) {
    xval = t.rows[r][x] + 0
    if (xval != "?")  {
      a[r].x = xval
      a[r].y = t.rows[r][y]
  }}
  n = keysort(a,"x")
  min = n / 16
  has(syms,s=1,"Sym")
  for(j=1; j<=n; j++)  {
    add(syms[s], a
  }
}
```
function SymAdds(i,j,k,v) {
  copy(i,k)
  k.n += j.n
  k.lo = i.lo < j.lo ? i.lo : j.lo
  k.hi = i.hi > j.hi ? i.hi : j.hi
  for(v in j.seen)
    k.seen[v] += j.seen[v]
  for(v in k.seen) {
    if (k.seen[v] > k.most) {
     k.most = k.seen[v]
     k.mode = v
}}}
```awk
function SymBore(i,k, b,r) {
  b = k in i.seen ? i.seen[k] : 0
  b = b / i.n
  r = i.n - b
  r = r / i.n
  return b^2/(b+r)
}
```

