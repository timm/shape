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
@include "ape"
@include "tab"

BEGIN {tests("tabok","_tab,_rows")} 

function _tab(f,    i) {
  Tab(i)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
  ok(f,length(i.rows)==398)
  ok(f ":3", i.cols[1].seen[3] == 4)
  ok(f ":8", i.cols[1].seen[8] == 103)
}

function _rows(f,    m,n,i,some,r1,r2,a,dom,j) {
  Tab(i)
  TabRead(i,"data/raw/auto93" APE.dot "csv") 
  m = 256
  while(m--)  
    some[ anyi(i.rows) ]
  for(r1 in some) {
    #n = TabAround(i, r1, some, i.the.x, a)
    #ok(f r1, a[1].dist < a[n].dist)
    for(r2 in some)
      if(r2>r1) {
       dom[r1].r1 = r1 
       dom[r1].dom += TabDom(i,r1,r2) }
  }
  print ""
  n= keysort(dom,"dom")
  for(j=1; j<=5;j++)
    print o(i.rows[ dom[j].r1 ] )
  print ""
  for(j=n-5;j<=n;j++)
    print o(i.rows[ dom[j].r1 ] )

}
```
