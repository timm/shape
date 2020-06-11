```awk

@include "ape"
@include "poly"
@include "num"

BEGIN {  tests("numok","_num") }

function _num(f,   n,lst,s,i) {
   s=" 9 2 5 4 12 7 8 11 9 3" \
     " 7 4 12 5 4 10 9 6 9 4"
   split(s, lst, " ")
   Num(n)
   for(i in lst)
    add(n,lst[i])
   ok(f, int(100*n.sd)== 306)
   ok(f, n.mu ==  7)
}

