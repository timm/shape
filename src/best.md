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
function Best(i,t,   cols,    r,rows) {
  Object(i)
  is(i,"Best")
  has(i,"best")
  has(i,"rest")
  i.min    = 2*length(t.rows)^MY.best.min
  i.enough = MY.best.enough / length(t.rows)
  i.cols   = cols ? cols : MY.best.cols
  for(r in t.rows) 
    if (rand() < i.enough) 
      push(rows,r);
  BestDiv(i,t,rows)
}
```
### BestDiv()

Random projection (project using cosine rule between two distant points).

```awk
function BestDiv(i,t,rows, 
              u,v,w,c,j,a,b,x,r,mid,d,best) {
  if (length(rows) < i.min) 
    return copy(rows, i.best)
  u = any(rows)
  v = BestFar(i,t,  u, rows)
  w = BestFar(i,t,  v, rows)
  c = BestDist(i,t, v, w)
  for(j in rows) {
    r     = rows[j]
    a     = BestDist(i,t, r, v)
    b     = BestDist(i,t, r, w)
    x     = (a^2+c^2 - b^2) / (2*c)  # cosine rule)
    if (x > 1) x = 1
    if (x < 0) x = 0
    mid  += x/length(rows)
    d[r]  = x
  }
  if (TabDom(t,v,w)) 
    for(r in d) 
      d[r] <= mid ? push(best,r) : push(i.rest,r)
  else   
    for(r in d) 
      d[r] <= mid ? push(i.rest, r) : push(best,r);
  oo(best)
  BestDiv(i,t,best) 
}
```
Interface functions: connecting `Best` to the services of `Tab`.

```awk
function BestDist(i,t,x,y) {return TabDist(t,x,y, t.the[i.cols])}
function BestFar( i,t,x,a) {return TabFar( t,x,a, t.the[i.cols])}
```

