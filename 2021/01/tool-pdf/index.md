# pdf tools usage

pdf tools usage
<!--more--> 
## PDF
### Merge files
Merge multiple pdf files as single one.
```bash
gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=finished.pdf page1.pdf page2.pdf
```

