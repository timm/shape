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

# Lib :o:

## Maths

```awk
function abs(x)  { return x>=0 ? x : -1*x }
```
## Lists

Misc

```awk
function any(a)    { return int(0.5 + rand()*length(a)) }
function push(a,x) { a[length(a)+1] = x; return x }
```
### copy()

Deep copy.

```awk
function copy(a, b,     i){
  for (i in a) {
    if(isarray(a[i])) {
      b[i][SUBSEP]
      copy(a[i], b[i])
      delete b[i][SUBSEP]
    } else 
      b[i] = a[i] }
}
```      
### o()

Convert a flat array to a string.

```awk
function o(a,     sep,    sep1,i,s) {
  for(i in a) {
    s    = s sep1 a[i]
    sep1 = sep ? sep : ", " }
  return s 
}
```      
### oo()

Display nested lists.
Print keys in sorted order.

```awk
function oo(a,prefix,    indent,   i,txt) {
  txt = indent ? indent : (prefix AU["dot"] )
  ooSortOrder(a)
  for(i in a)  {
    if (isarray(a[i]))   {
      print(txt i"" )
      oo(a[i],"","|  " indent)
    } else
      print(txt i (a[i]==""?"": ": " a[i])) }
}
function ooSortOrder(a, i) {
  for (i in a)
   return PROCINFO["sorted_in"] =\
     typeof(i+1)=="number" ? "@ind_num_asc" : "@ind_str_asc"
}
```

### keysort()

Some nested array `a` by some field `k`.

```awk
function keysort(a,k) {
  APE.keysort = k
  return asort(a,a,"keysorter")
}

function keysorter(i1,x,i2,y) {
  return compare(x[ APE.keysort ] + 0,
                 y[ APE.keysort ] + 0)
} 

function compare(x,y) {
  if (x < y) return -1
  if (x == y) return 0
  return 1
}
```

## Unit tests

### tests()

Top level unit-test driver.  Resets the random number generator
before each test.  Prints the group and name of the test.
Warns about stray globals at the end.

```awk
function tests(what, all,   f,a,i,n) {
  n = split(all,a,",")
  print "\n#--- " what " -----------------------"
  for(i=1;i<=n;i++) { 
    srand(1)
    f = a[i]; 
    @f(f) 
  }
  rogues()
}
```
### ok()

Report the `yes` or `no` message if a test passes or fails.
Increments the global `test.yes` and `test.no` counters.

```awk
function ok(f,yes,    msg) {
  msg = yes ? "PASSED!" : "FAILED!"
  if (yes) 
     APE.test.yes++ 
  else
     APE.test.no++;
  print "#test:\t" msg "\t" f
}
```
### near()

Return true if what you `got` is within `epsilon` of
what you `want` (`epsilon` defaults to 0.001).

```awk
function near(got,want,     epsilon) {
   epsilon = epsilon ? epsilon : 0.001
   return abs(want - got)/(want + 10^-32)  < epsilon
}
```
### rogues()

Report variables that have escaped from functions.

```awk
function rogues(    s) {
  for(s in SYMTAB) 
    if (s ~ /^[A-Z][a-z]/) 
      print "#W> Global " s>"/dev/stderr"
  for(s in SYMTAB) 
    if (s ~ /^[_a-z]/    ) 
      print "#W> Rogue: " s>"/dev/stderr"
}
```

## Input

## csv() 

Iterator.

Standard GAWK can read simple comma seperated files. 
But what about CSV files with:

- comments, that should be stripped away?
- blank lines, that should be skipped?
- spurious white space, that should be deleted?
- records that break over multiple lines?

For example, here is a csv file where the first record is really
`name,age,shoesize`, there are comments and new lines and white
space to ignore, and the last record is split over multiple lines.

        name,     # clients name
        age,      # posint, 0..120
        shoesize
        #-------------------------
 
        tim,   21,  12
        susan, 22,  2132
        sarah,
               14,  101

This `csv` function can handle all that. Note that the second `file`
argument is optional and, if omitted, this code will read from
standard input.

```awk   
function csv(a,file,     b4, status,line) {
  file   = file ? file : "-"           # [1]
  status = getline < file
  if (status<0) {   
    print "#E> Missing file ["file"]"  # [2]
    exit 1 
  }
  if (status==0) {
    close(file) 
    return 0
  }                                    # [3]
  line = b4 $0                         # [4]
  gsub(/([ \t]*|#.*$)/, "", line)      # [5]
  if (!line)       
    return csv(a,file, line)           # [6]
  if (line ~ /,$/) 
    return csv(a,file, line)           # [4]
  split(line, a, ",")                  # [7]
  return 1
}
```
Notes:

1. Defaults to standard input
2. Complains on missing input
3. At end of file, close this stream
4. If line ends in ",", combine this line with the next.
   Else....    
5. Kill whitespace and comments
6. Skip blank lines
7. Split line on "," into the array "a"

Example usage:

     function lintcsv(file,    row,n,want) 
        while(csv(row,file)) 
          if (++n == 1) 
            want = length(row) # row1 defines what we `want`
          else if (want != length(row)) 
            print "#E> row " n " wrong number of fields" 
     }

## cells()

Iterator.

Read a csv file `f`, 

- Set `i.cells` to all columns that _do not_ start with `?`.
- Set `i.r` to the current row number (and first row as `i.r=0`)


```awk
function Row(i,file) {
  i.file = file
  has(i,"use")
  has(i,"cells")
  i.r = -1
}
function Rows(i,   c,tmp) {
  if (!csv(tmp,i.file)) 
    return 0
  if (!length(i.cells)) 
    for(c in tmp) 
      if (tmp[c] !~ /\?/)
        i.use[c] 
  i.r++
  for(c in i.use)  
    i.cells[c] = tmp[c] 
  return 1
}
```
Note: `i` needs to be reset before
each call to `Rows` e.g.

     Row(i, "data" AU.dot "csv")
     while( Rows(i) ) {
       print i.cells[1]


