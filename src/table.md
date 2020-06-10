<a name=top>
<h1 align=center>
   <a href="https://github.com/timm/shape/blob/master/shape/README.md#top">
     SHape = Simple HAck 4   Programs + documeEntation
   </a>
</h1>
<p align=center>
   <a    href="https://github.com/timm/shape/blob/master/LICENSE.md#top">license</a>
   :: <a href="https://github.com/timm/shape/blob/master/INSTALL.md#top">install</a>
   :: <a href="https://github.com/timm/shape/blob/master/CODE_OF_CONDUCT.md#top">contribute</a>
   :: <a href="https://github.com/timm/shape/issues">issues</a>
   :: <a href="https://github.com/timm/shape/blob/master/CITATION.md#top">cite</a>
   :: <a href="https://github.com/timm/shape/blob/master/CONTACT.md#top">contact</a>
</p>
<p align=center>
   <img width=600 src="https://github.com/timm/misc/blob/master/odd/etc/img/solidgallery.gif">
</p>
<p align=center>
   <img src="https://img.shields.io/badge/language-lua-orange">
   <img src="https://img.shields.io/badge/purpose-ai,se-blueviolet">
   <img src="https://img.shields.io/badge/platform-mac,*nux-informational">
   <a href="https://travis-ci.org/github/timm/shape"> <img src="https://travis-ci.org/timm/shape.svg?branch=master"></a>
   <a href="https://zenodo.org/badge/latestdoi/263210595"> <img src="https://zenodo.org/badge/263210595.svg" alt="DOI"></a>
</p>

# Table

```awk
@include "ape"
@include "num"
@include "sym"
@include "poly"

function Table(i) {
  Object(i)
  is(i,"Table")
  has(i,"cols")
  has(i,"rows")
  has(i,"my") # stores indexes to particular subsets of cols 
}
function what(i,txt,pos) { 
  if (txt ~ /[<>]/) i.my.goals[pos]
  return txt ~ /[\$<>]/ ? "Num" : "Sym" 
}

function read(i,f,    r,c) {
  FS = ","
  f  = f ? f : "-"
  r  = -1
  while((getline f) > 0)  { 
    gsub(/([ \t\r]*|#.*)/, "")
    if (length($0)==0) continue
    if(++r == 0) {
      for(c=1; c<=NF; c++) 
        if ($c !~ /\?/)
          hass(i.cols, c, what(i,$c,c), $c, c) 
    } else
        for(c in i.cols)
          i.rows[r][c] = add(i.cols[c], $c)
}
```

```awk
function dist(i,r1,r2,cols,  p,x,y,d,n) {
  n = 0.00001 # stop divide by zero errors
  p = the.dist.p
  for(c in cols)
    x  = norm(i, c, data[r1][c])
    y  = norm(i, c, data[r2][c])
    d += abs(x-y)^p
    n++
  }
  return (d/n)^(1/p)
}
function norm(i,c,x,   lo,hi) {
  if (x ~ /\?/) return x
  lo = i.cols[c].lo
  hi = i.cols[c].hi
  return (x - lo)/(hi - lo + 10^-32)
}
```
