<a name=top>
<h1 align=center>
   <a href="https://github.com/timm/shape/blob/master/README.md#top">
     SHape = SHell tricks  for Any Program + documeEntation
   </a>
</h1>
<p align=center>
   <a    href="https://github.com/timm/shape/blob/master/LICENSE.md#top">license</a>
   :: <a href="https://github.com/timm/shape/blob/master/INSTALL.md#top">install</a>
   :: <a href="https://github.com/timm/shape/blob/master/CONTRIBUTE.md#top">contribute</a>
   :: <a href="https://github.com/timm/shape/issues">issues</a>
   :: <a href="https://github.com/timm/shape/blob/master/CITATION.md#top">cite</a>
   :: <a href="https://github.com/timm/shape/blob/master/CONTACT.md#top">contact</a>
</p>
<p align=center>
   <img width=600 src="https://github.com/timm/misc/blob/master/odd/etc/img/solidgallery.gif">
</p>
<p align=center>
   <img src="https://img.shields.io/badge/language-gawk-orange">
   <img src="https://img.shields.io/badge/purpose-ai,se-blueviolet">
   <img src="https://img.shields.io/badge/platform-mac,*nux-informational">
   <a href="https://travis-ci.org/github/timm/shape"> <img src="https://travis-ci.org/timm/shape.svg?branch=master"></a>
   <a href="https://doi.org/10.5281/zenodo.3887420"><img src="https://zenodo.org/badge/DOI/10.5281/zenodo.3887420.svg" alt="DOI"></a>
</p>

# Space

```awk
@include "ape"
@include "tab"
```

Using random projections,
recursively divide the `rows` in a `Tab`le.

```awk
function Best(i,t,    r,rows) {
  Object(i)
  is(i,"Best")
  i.min    = 2*length(t.rows)^THE.best.min
  i.enough = THE.best.enough / length(t.rows)
  i.cols   = THE.best.cols
  has(i,"best")
  has(i,"rest")
  for(r in t.rows) 
    if (rand() < i.enough)
      rows[r];
  print( length(rows) )
  BestGet(i,t,rows)
}
```

Interface functions: connecting `Best` to the services of `Tab`.

```awk
function BestDist(i,t,x,y) {return TabDist(t,x,y, t.my[i.cols])}
function BestFar( i,t,x,a) {return TabFar( t,x,a, t.my[i.cols])}
```
### BestGet()

Divide the data (if there are enough rows). Otherwise,
all the current `rows` are `best`.

```awk
function BestGet(i,t,rows,   x) {
  if (length(rows) >= i.min) 
    BestDiv(i,t,rows)
  else
    copy(rows, i.best)
}
```
### BestDiv()

Random projection (project using cosine rule between two distant points).

```awk
function BestDiv(i,t,rows, 
              one,two,three,c,r,a,b,x,mid,d,best) {
  one   = any(rows)
  two   = BestFar(i,t,  one, rows)
  three = BestFar(i,t,  two, rows)
  c     = BestDist(i,t, two, three)
  for(r in rows) {
    a     = BestDist(i,t, r, two)
    b     = BestDist(i,t, r, three)
    x     = (a^2+c^2 - b^2) / (2*c)  # cosine rule)
    if (x > 1) x = 1
    if (x < 0) x = 0
    mid  += x/length(rows)
    d[r]  = x
  }
  if (TabDom(t,two,three)) 
    for(r in d) 
      d[r] <= mid ? best[r] : i.rest[r]
  else   
    for(r in d) 
      d[r] >= mid ? best[r] : i.rest[r];
  BestGet(i,t,best) 
}
```
