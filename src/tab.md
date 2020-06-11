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

# `tab`les

```awk
@include "ape"  # standard libraries
@include "num"  # knows "mu", standard deviations "sd"
@include "sym"  # knows "mode" and counts of "seen" symbols
@include "poly" # polymorphic functions
```

`tab`les store raw data in `rows`  and summaries of
those data in `cols` (columns). For example `i.rows[r][c]`
holds  data from row `r` and columns `c`.

```awk
function Tab(i) { 
  Object(i)
  is(i,"Tab")
  has(i,"cols")
  has(i,"rows")
  has(i,"my") # stores indexes to particular subsets of cols 
}
```

The first row of data names the columns. Special symbols
on those names distinguish different kinds of columns. For example:

    outlook, $temp,  <humid, wind,  !play        
    rainy,   68,     80,     FALSE, yes # comments
    sunny,   85,     85,     FALSE, no 
    ...

In this example:

- `$temp` is a column of numbers; 
- `<humid` is a goal to be minimized; 
- `!play` is a symbolic class;
-  and the rest of the columns hold symbols.

```awk
function TabCol(i,x,c) { 
  if (x ~ /!/)    i.my.klass[c]
  if (x ~ /[<>]/) i.my.goals[c]
  if (x ~ /[\$<>]/) { hass(i.cols,c,"Num",x,c); i.my.nums[c] }
  else              { hass(i.cols,c,"Sym",x c); i.my.syms[c] }
}
```

`Tab`les can be initialize from  comma separated value files via 
`tabRead`.

```awk
function TabRead(i,f,    r,c) {
  FS = ","
  f  = f ? f : "-"
  r  = -1
  while((getline < f) > 0)  { 
    gsub(/([ \t\r]*|#.*)/, "")
    if (!length($0)) continue
    r++
    for(c=1; c<=NF; c++) 
      if(r == 0) 
        TabCol(i,$c,c)
      else
        i.rows[r][c] = add(i.cols[c], $c) }
}
```

```awk
function TabDist(i,r1,r2,cols,  n,p,d,d1) {
  n = 0.00001 # stop divide by zero errors
  p = THE.dist.p
  for(c in cols)  {
    d1 += dist( i.cols[c], i.rows[r1][c], i.rows[r2][c] )
    n++
  }
  return (d/n)^(1/p)
}

function TabFar(i,r1,rows,cols,  a,n,r2) {
  for(r2 in rows) 
    if(r1 != r2) {
      a[r2].row = r2
      a[r2].dist = dist(i,r1,r2,cols) }
  n = keysort(a,"dist")
  n = int(n*THE.distant.far)  
  return a[n].row
}
```
