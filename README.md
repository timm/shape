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

## How

To get started, see our [install](INSTALL.md) instructions and our
[guide for contributors](CONTRIBUTING.md).


## What

- GAWK is very portable, very succinct, scripting language. 
- GOLD is an object layer that extends GAWK with objects, aggregation, polymorphism and inheritance. 
- GOLDMINE are some data mining tools written in GOLD.
- The GOLDRUSH are a set of data mining fairness operators for GOLDMINE. 

GOLDEN is a cross-platform development environment for GOLD based on bash, vim and tmux that looks like this:

<p align=center><a href="etc/img/screen.png"><img src="etc/img/screen900.png" width=900></a></p>

To support easy documentation

- GOLD code is stored in x.md  markdown files 
- To give a repository its own identify and a common look-and-feel,
     the first paragraph of the main `README.md` can be automatically   pushed out across all other `*.md` filess
    in the repo. 

To support easy access to nested fields
- A dot notation is added 
- e.g  `a.b.c = 2.3` becomes `a["b"]["c"] = 2.3`. 
- Note that GOLD knows not to alter  the decimal point in 2.3

To implement the GOLD:
- Code in src/\*.md is transpiled to .var/x.awk
- The transpiler is a one line long:

```
s = gensub(/\.([^0-9\\*\\$\\+])([a-zA-Z0-9_]*)/,"[\"\\1\\2\"]","g",s)
```

To support object creation aggregation, and inheritance 
- There is  30 likes of portable awk (so not preprocessing needed there)

To support polymorphism, 
- All  objects have a type field called   "ois". 
- This, plus indirect functions, are  used to defined  polymorphic verbs e.g.

```awk
function add(i,x,    f) { f=i.ois "Add";   return @f(i,x)  }
function dec(i,x,    f) { f=i.ois "Dec";   return @f(i,x)  }
function show(i,     f) { f=i.ois "Show";  return @f(i)    }
function score(i,    f) { f=i.ois "Score"; return @f(i)    }
function dist(i,x,y, f) { f=i.ois "Dist";  return @f(i,x,y)}
```    

To support unit testing:
- Code in `src/x.md` has a test file `test/xok.md`. 
- A shell script changes to /test and rules all the \*ok.md files
- A .travis.yml file uses that script to retest the code after each commit

To support easy install and uninstall:
- All files in GOLDEN are defined relative to the position of the  main `gold`. 
- So installation is mostly just unzipping a file to a directory
- Uninstalling is  zapping that directory.


## Example
Here's an iterator that prunes away columns that start with a "?" in their name.  A variable i.cells is reset for every step of the loop. This variable holds   just the columns we want to use

     Row(it, "somecsvfile")    # Row defines "it", which is the iterator
     while( Rows(it) ) { # Rows runs the Row interator
       print it.cells[1]

Fyi- having coded this iterator in lua and python and coffeescript, I can assert that that the following is 
the simplest implementation 
I've yet seen 

```awk
function Row(i,file) {
  i.file = file
  has(i,"use") # aggregation
  has(i,"cells") # aggregation
  i.r = -1
}
function Rows(i,   c,tmp,n) {
  if (!csv(tmp,i.file))   # iterators can be nested; e.g. csv is another iterator
    return 0              # signal end of iterator
  if (!length(i.cells))   # the initialization step. only called for line 1
    for(c in tmp)
      if (tmp[c] !~ /\?/)
        i.use[c] = ++n;
  i.r++
  for(c in i.use)
    i.cells[ i.use[c] ] = tmp[c]
  return 1                # signal to continue the iteration
}
```
Iterators can be nested. e.g. the above code calls the csv iterator (no new objects here,  just good old gawk):

```awk
function csv(a,file,     b4, status,line) {
  file   = file ? file : "-"            # can read from standard in
  status = getline < file
  if (status<0) {                       # complain about missing files
    print "#E> Missing file ["file"]"  
    exit 1  }                           # crash on error
  if (status==0) {
    close(file)
    return 0  }                         # signal that iteration can continue                                
  line = b4 $0                         
  gsub(/([ \t]*|#.*$)/, "", line)       # no spaces or comments
  if (!line) return csv(a,file, line)   # skip blank likes
  if (line ~ /,$/) return csv(a,file, line) # join lines ending with "," to next line
  split(line, a, ",")                   # reset the output  list
  return 1                              # signal that iteration can continue 
}
```
