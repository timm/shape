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

# `Tab`les

```awk
@include "ape"  # standard libraries
@include "num"  # knows "mu", standard deviations "sd"
@include "sym"  # knows "mode" and counts of "seen" symbols
@include "poly" # polymorphic functions
```

`Tab`les store raw data in `rows`  and summaries of
those data in `cols` (columns). For example `i.rows[r][c]`
holds  data from row `r` and columns `c`.

```awk
function Tab(i) {
  Object(i)
  is(i,"Table")
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
function TabWhat(i,txt,c) { 
  if (txt ~ /!/)        i.my.klass[c]
  if (txt ~ /[<>]/)     i.my.goals[c]
  if (txt ~ /[\$<>]/) { i.my.nums[c]; return "Num" }
  i.my.syms[c]
  return "Sym" }
}
```

Tables can be initializd from  comma separated value files via 
the `TabRead`.

```awk
function TabRead(i,f,    r,c) {
  FS = ","
  f  = f ? f : "-"
  r  = -1
  while((getline f) > 0)  { 
    gsub(/([ \t\r]*|#.*)/, "")
    if (length($0)==0) continue
    if(++r == 0) {
      for(c=1; c<=NF; c++) 
        if ($c !~ /\?/)
          hass(i.cols, c, TabHat(i,$c,c), $c, c) 
    } else
        for(c in i.cols)
          i.rows[r][c] = add(i.cols[c], $c) }
}
```

```awk
function TabDist(i,r1,r2,cols,  c,p,x,y,d,n) {
  n = 0.00001 # stop divide by zero errors
  p = THE.dist.p
  for(c in cols) {
    x  = TabNorm(i, c, i.data[r1][c])
    y  = TabNorm(i, c, i.data[r2][c])
    d += abs(x-y)^p
    n++
  }
  return (d/n)^(1/p)
}
function TabNorm(i,c,x,   lo,hi) {
  if (x ~ /\?/) return x
  lo = i.cols[c].lo
  hi = i.cols[c].hi
  return (x - lo)/(hi - lo + 10^-32)
}
```
