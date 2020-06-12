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

# Tabs

Read a csv file into multiple tables:

- `i.all`   holds all data
- `i.klass[k]` does jsut the rows for class `k`.

```awk
@include "tab"

function Tabs(i) {
  has(i,"all",  "Tab")
  has(i,"klass")
}
```
Ensure that we have a  `klass` for a row.

```awk
function TabsK(i,row,   k) {
  k = row[i.all.my.klass]
  if (!(k in i.klass)) 
    hass(i.klass, k, "Tab", i.all.header)
  return k
}
```

```awk
function TabsRead(i,f,  it,k,c,x) {
  Row(it, f)
  while(Rows(it)) {
    if (it.r == 0) 
      TabCols(i.all, it.cells)
    else {
      k = TabsK(i, it.cells)
      print(it.r,k)
      for(c in it.cells)  {
        x = it.cells[c]
        i.klass[k].rows[r][c]= add(i.klass[k].cols[c], x)
        i.all.rows[r][c]     = add(i.all.cols[c],      x)}}}
  close(f)
}
```
