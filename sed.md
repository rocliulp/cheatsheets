# '\n' treatment on bash (sed/tr) 
Actually it's not so easy for 'sed' itself to treat '\n'. So if possible just use 'tr' cmd to work on '\n'  on bash
``` bash
gsed -e "s/^/\(/;s/\t/,\'/;s/\ *$/\\'\)/" tmpmbl.txt | tr '\n' ',' && echo ""
cat tmpmbl.txt | tr '|' ',' | sed -e "s/^,\ */\(/;s/\ *,\ */,\'/;s/\ *,$/\'\)/" | tr '\n' ',' && echo ""
```