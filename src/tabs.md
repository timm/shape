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

```awk
@include "tab"

function Tabs(i) {
  has(i,"all","Tab")
  has(i,"parts")
}
function TabsRow(t,r,c) {t.rows[r][c] = add(t.cols[c],$c)}
function TabsRead(i,f,  r,c) {
  FS = ","
  f  = f ? f : "-"
  r  = -1
  while((getline < f) > 0)  { 
    gsub(/([ \t\r]*|#.*)/, "")
    if (!length($0)) continue
    r++
    if(r == 0) 
      for(c=1; c<=NF; c++) 
        TabCol(i.all,$c,c)
    else {
      k = $i.all.my.klass
      if (!(k in i.parts)) 
        has(i.parts, k, "Tab", i.all.header)
      for(c=1; c<=NF; c++) {
        TabsCell(i.all,      r,c)
        TabsCell(i.parts[k], r,c) }}}
  close(f)
}
