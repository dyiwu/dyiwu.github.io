# Full name

This awk script translates the format of full name list in input file from the first name first format to the last name first format.
<!--more-->
Here is the awk code.
```awk
$ cat fullname.awk
#!/usr/bin/awk -f
{
  OFS="";
  $1=$1;
  $2=$2;
  $3=$3;
#  print "---",$1,"---,"$2,"---,"$3"---";
  if ($1 == "" && $2 == "" && $3 == "") print ;
  else
  {
  FirstName=$1;
  if ($3 != "")
    { MiddleName=$2;
      LastName=$3;
      print LastName,", ",FirstName," ",MiddleName;
      }
  else
    { LastName=$2;
      print LastName,", ",FirstName;
      }
  }
}
```
Run time example,
```
$ nl -b a namelist.txt
     1  Prest Lin
     2  Cereal Mak
     3  Joo kim Chong
     4
     5
     6  David Wu
     7  Poh Leng Lim
     8
     9  Vincent Ng
    10  Johnson Hsiao 
$
$ ./fullname.awk namelist.txt > out.txt
$
$ nl -b a out.txt
     1  Lin, Prest
     2  Mak, Cereal
     3  Chong, Joo kim
     4
     5
     6  Wu, David
     7  Lim, Poh Leng
     8
     9  Ng, Vincent
    10  Hsiao, Johnson

```

