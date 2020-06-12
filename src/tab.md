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

# Tab

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
function Tab(i,  header,c) { 
  Object(i)
  is(i,"Tab")
  has(i,"cols")
  has(i,"rows")
  has(i,"my") # stores indexes to particular subsets of cols 
  has(i,"header")
  for(c in header)
    TabCol(i, header[c], c)
}
```
The first row of data names the columns. Special symbols
on those names distinguish different kinds of columns. For example, 
in the following data fragment...

    outlook, $temp,  <humid, wind,  !play        
    rainy,   68,     80,     FALSE, yes # comments
    sunny,   85,     85,     FALSE, no 
    ...

... then the columns are:

- `$temp` is a column of `num`bers; 
- `<humid` is a `goal` to be minimized; 
- `!play` is a `sym`bolic class;
-  and the rest of the columns hold `sym`bols.

Note that we say that the goals and classes
are the `y` columns and the others are the `x`
columns.

### TabCol()

Define a new column whose name is `x` at position `c`.

```awk
function TabCol(i,x,c) { 
  i.header[c] = x
  if  ( x ~ /!/     ) i.my.klass[c]
  if  ( x ~ /[<>]/  ) i.my.goals[c]
  i.my[ x ~ /[!<>]/ ? "y"   : "x" ][c]
  if (x ~ /[\$<>]/) { hass(i.cols,c,"Num",x,c); i.my.nums[c] }
  else              { hass(i.cols,c,"Sym",x c); i.my.syms[c] }
}
```

### TabRead()

`Tab`les can be initialize from  comma separated value files via 
`tabRead`. If `f` is omitted, then this code reads from standard input.

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
  close(f)
}
```
### TabDist()

Returns the distance between two rows, manured
in terms of the columns `cols`.


```awk
function TabDist(i,r1,r2,cols,  
                 c,d,inc,n,p) {
  p = THE.tab.p
  n = length(cols)+0.00001
  for(c in cols) {
    inc = dist(i.cols[c], i.rows[r1][c], i.rows[r2][c])
    d  += inc^p
  }
  return (d/n)^(1/p)
}
```
### TabFar()

Returns a row that is far away from row `r1`.

- To avoid extreme outliers, we only go `THE.tab.far` away from `r1`.
- This function:
  - Searchers everything in `i.rows`
  - Computes distance using all the optimization `i.my.x`. 

```awk
function TabFar(i,r1,rows,cols,  a,n) {
  n = TabAround(i,r1,rows,cols,a)
  n = int(n*THE.tab.far)  
  return a[n].row
}

function TabAround(i,r1,rows,cols,a,   n,r2) {
  List(a)
  for(r2 in rows) 
    if(r1 != r2) {
      a[r2].row = r2
      a[r2].dist = TabDist(i,r1,r2,cols) }
  return keysort(a,"dist")
}
```

```awk
function TabDom(i,r1,r2,   c,e,n,x,y,s1,s2) {   
  n = length(i.my.goals)
  for(c in i.my.goals) {
    x   = i.data[r1][c]
    y   = i.data[r2][c]
    x   = NumNorm(i.cols[c], x)
    y   = NumNorm(i.cols[c], y)
    s1 -= 2.72 ^ ( i.cols[c].w * (x - y)/n )
    s2 -= 2.72 ^ ( i.cols[c].w * (y - x)/n )
  }
 return s1/n < s2/n
}
```
