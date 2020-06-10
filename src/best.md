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

# best

```awk
@include "table"

function best(i,min,out,   rows,r) {
  for(r in i.data) 
    if (rand() < THE.best.want/length(i.data)) 
      push(rows,r)
  besthalves(i,rows, out, 2*length(i.data)^THE.best.min)
}

function besthalves(i,rows,out,min,  x,tmp) {
  if (length(rows) < min)  {
    for(x in rows)
      out[x] = rows[x] 
  } else { 
     besthalf(  i,rows,tmp)
     besthalves(i,tmp, out, min) }
}
      
function besthalf(i,rows,out,
                  one,two,three,c,r,a,b,x,mid,d) {
  one     = any(rows)
  two     = distant(i,one,rows,   i.my.goals)
  three   = distant(i,two,rows,   i.my.goals)
  c       = dist(i,   two, three, i.my.goals)
  for(r in rows) {
    a     = dist(r, two)
    b     = dist(r, three)
    x     = (a^2+c^2 - b^2) / (2*c) 
    if (x > 1) x = 1
    if (x < 0) x = 0
    mid   += x/length(rows)
    d[r]  = x
  }
  if (dom(i,two,three)) {
    for(r in d) 
      if (d[r] <= mid) push(out,r)
  } else  
    for(r in d) 
      if (d[r] >= mid) push(out,r)
}
function distant(i,r1,rows,cols,  a,n,r2) {
  for(r2 in rows) 
    if(r1 != r2) {
      a[r2].row = r2
      a[r2].dist = dist(i,r1,r2,cols) }
  n = keysort(a,"dist")
  n = int(n*THE.distant.far)  
  return a[n].row
}
function dom(i,r1,r2,   c,e,n,x,y,s1,s2) {   
  n = length(i.my.goals)
  for(c in i.my.goals) {
    x   = i.data[r1][c]
    y   = i.data[r2][c]
    x   = norm(i, c, x)
    y   = norm(i, c, y)
    s1 -= 2.72 ^ ( i.cols[c].w * (x - y)/n )
    s2 -= 2.72 ^ ( i.cols[c].w * (y - x)/n )
  }
 return s1/n < s2/n
}

BEGIN {rogues()}
