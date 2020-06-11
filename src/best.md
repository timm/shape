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
function Best(i,t, cols) {
  Object(i)
  i.min = length(t.rows)^THE.best.min
  i.enough  = THE.space.some / length(t.rows)
  i.cols = cols ? cols : "x"
}
function BestDist(i,t,r1,r2) { 
  return TabDist(t,r1,r2, t.my[i.cols]) 
}
function BestFar(i,t,r1,r2) { 
  return TabFar(t,r1,r2,t.my[i.cols])
}
function BestHalf(i,t, rest,best,   r,min) {
  for(r in i.data) 
    if (rand() < i.enough)
      push(rows,r)
  BestHalves(i,t,rows, rest,best)
}

function BestHalves(i,t,rows,rest,best,  x,best0) {
  if (length(rows) < i.min) {
    for(x in rows)
      best[x] = rows[x] 
  } else { 
     BestRest( i,t, rows,  rest, best0)
     BestHalves(i,t, best0, rest, best) }
}
      
function BestHalfi,t,rows,rest,best,
              one,two,three,c,r,a,b,x,mid,d) {
  one     = any(rows)
  two     = BestFar( i,t,one,rows )
  three   = BestFar( i,t,two,rows)
  c       = BestDist(i, two, three)
  for(r in rows) {
    a     = BestDist(i,t, r, two)
    b     = BestDist(i,t, r, three)
    x     = (a^2+c^2 - b^2) / (2*c) 
    if (x > 1) x = 1
    if (x < 0) x = 0
    mid  += x/length(rows)
    d[r]  = x
  }
  if (BestDom(i,two,three)) {
    for(r in d) 
      d[r] <= mid ? push(best,r) : push(rest,r)
  } else  
    for(r in d) 
      d[r] >= mid ? push(best,r) : push(rest,r)
}

function BestDom(i,r1,r2,   c,e,n,x,y,s1,s2) {   
  n = length(i.my.goals)
  for(c in i.my.goals) {
    x   = i.data[r1][c]
    y   = i.data[r2][c]
    x   = NumNorm(i, c, x)
    y   = NumNorm(i, c, y)
    s1 -= 2.72 ^ ( i.cols[c].w * (x - y)/n )
    s2 -= 2.72 ^ ( i.cols[c].w * (y - x)/n )
  }
 return s1/n < s2/n
}

BEGIN {rogues()}
