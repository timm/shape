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


