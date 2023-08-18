# ShellScript

Project for practicing shell script in Bash. Creating ETL pipeline using bash.

Step 1
Extracts data from /etc/passwd file into a CSV file.

The csv data file contains the user name, user id and 
home directory of each user account defined in /etc/passwd

Transforms the text delimiter from ":" to ",".
Loads the data from the CSV file into a table in PostgreSQL database.

Extract phase

echo "Extracting data"

Extract the columns 1 (user name), 2 (user id) and 
6 (home directory path) from /etc/passwd

cut -d":" -f1,3,6 /etc/passwd 

Step 2
theia@theiadocker-dhwaninijhaw:/home/project$ bash csv2db.sh

Extracting data
```
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/var/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
messagebus:101:/nonexistent
theia:1000:/home/theia
mongodb:102:/var/lib/mongodb
ntp:103:/nonexistent
cassandra:104:/var/lib/cassandra
postgres:105:/var/lib/postgresql
```
Step 3
Redirect the extracted output into a file.

theia@theiadocker-dhwaninijhaw:/home/project$ cut -d":" -f1,3,6 /etc/passwd > extracted-data.txt
theia@theiadocker-dhwaninijhaw:/home/project$ bash csv2db.sh
```
Extracting data
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/var/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
messagebus:101:/nonexistent
theia:1000:/home/theia
mongodb:102:/var/lib/mongodb
ntp:103:/nonexistent
cassandra:104:/var/lib/cassandra
postgres:105:/var/lib/postgresql
theia@theiadocker-dhwaninijhaw:/home/project$ cat extracted-data.txt
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/var/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
messagebus:101:/nonexistent
theia:1000:/home/theia
mongodb:102:/var/lib/mongodb
ntp:103:/nonexistent
cassandra:104:/var/lib/cassandra
postgres:105:/var/lib/postgresql
```
Step - 4
Transform the data into CSV format

The extracted columns are separated by the original “:” delimiter.

We need to convert this into a “,” delimited file.

Add the following lines to the bash script 
Transform phase
echo "Transforming data"
read the extracted data and replace the colons with commas.

tr ":" "," < extracted-data.txt

save and run 

theia@theiadocker-dhwaninijhaw:/home/project$ bash csv2db.sh
```
Extracting data
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/var/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
messagebus:101:/nonexistent
theia:1000:/home/theia
mongodb:102:/var/lib/mongodb
ntp:103:/nonexistent
cassandra:104:/var/lib/cassandra
postgres:105:/var/lib/postgresql
Transforming data
root,0,/root
daemon,1,/usr/sbin
bin,2,/bin
sys,3,/dev
sync,4,/bin
games,5,/usr/games
man,6,/var/cache/man
lp,7,/var/spool/lpd
mail,8,/var/mail
news,9,/var/spool/news
uucp,10,/var/spool/uucp
proxy,13,/bin
www-data,33,/var/www
backup,34,/var/backups
list,38,/var/list
irc,39,/var/run/ircd
gnats,41,/var/lib/gnats
nobody,65534,/nonexistent
_apt,100,/nonexistent
messagebus,101,/nonexistent
theia,1000,/home/theia
mongodb,102,/var/lib/mongodb
ntp,103,/nonexistent
cassandra,104,/var/lib/cassandra
postgres,105,/var/lib/postgresql
```
Verified that : is replaced with ,

Replace the tr command at end of the script with the command below.
tr ":" "," < extracted-data.txt > transformed-data.csv

Step 5 

Save and run 

theia@theiadocker-dhwaninijhaw:/home/project$ cat transformed-data.csv
```
root,0,/root
daemon,1,/usr/sbin
bin,2,/bin
sys,3,/dev
sync,4,/bin
games,5,/usr/games
man,6,/var/cache/man
lp,7,/var/spool/lpd
mail,8,/var/mail
news,9,/var/spool/news
uucp,10,/var/spool/uucp
proxy,13,/bin
www-data,33,/var/www
backup,34,/var/backups
list,38,/var/list
irc,39,/var/run/ircd
gnats,41,/var/lib/gnats
nobody,65534,/nonexistent
_apt,100,/nonexistent
messagebus,101,/nonexistent
theia,1000,/home/theia
mongodb,102,/var/lib/mongodb
ntp,103,/nonexistent
cassandra,104,/var/lib/cassandra
postgres,105,/var/lib/postgresql
```
Step -6 
Load the data into the table ‘users’ in PostgreSQL
Do this by copying the following code at the end of bash script 

Load phase
echo "Loading data"
Send the instructions to connect to 'template1' and
copy the file to the table 'users' through command pipeline.

echo "\c template1;\COPY users  FROM '/home/project/transformed-data.csv' DELIMITERS ',' CSV;" | psql --username=postgres --host=localhost

theia@theiadocker-dhwaninijhaw:/home/project$ bash csv2db.sh
```
Extracting data
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/var/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
messagebus:101:/nonexistent
theia:1000:/home/theia
mongodb:102:/var/lib/mongodb
ntp:103:/nonexistent
cassandra:104:/var/lib/cassandra
postgres:105:/var/lib/postgresql
Transforming data
root,0,/root
daemon,1,/usr/sbin
bin,2,/bin
sys,3,/dev
sync,4,/bin
games,5,/usr/games
man,6,/var/cache/man
lp,7,/var/spool/lpd
mail,8,/var/mail
news,9,/var/spool/news
uucp,10,/var/spool/uucp
proxy,13,/bin
www-data,33,/var/www
backup,34,/var/backups
list,38,/var/list
irc,39,/var/run/ircd
gnats,41,/var/lib/gnats
nobody,65534,/nonexistent
_apt,100,/nonexistent
messagebus,101,/nonexistent
theia,1000,/home/theia
mongodb,102,/var/lib/mongodb
ntp,103,/nonexistent
cassandra,104,/var/lib/cassandra
postgres,105,/var/lib/postgresql
Loading data
You are now connected to database "template1" as user "postgres".
COPY 25
```
save and run bash script

Step 8 

Run this command to verify

theia@theiadocker-dhwaninijhaw:/home/project$ echo '\c template1; \\SELECT * from users;' | psql --username=postgres --host=localhostYou are now connected to database "template1" as user "postgres".
```
  username  | userid |    homedirectory    
------------+--------+---------------------
 root       |      0 | /root
 daemon     |      1 | /usr/sbin
 bin        |      2 | /bin
 sys        |      3 | /dev
 sync       |      4 | /bin
 games      |      5 | /usr/games
 man        |      6 | /var/cache/man
 lp         |      7 | /var/spool/lpd
 mail       |      8 | /var/mail
 news       |      9 | /var/spool/news
 uucp       |     10 | /var/spool/uucp
 proxy      |     13 | /bin
 www-data   |     33 | /var/www
 backup     |     34 | /var/backups
 list       |     38 | /var/list
 irc        |     39 | /var/run/ircd
 gnats      |     41 | /var/lib/gnats
 nobody     |  65534 | /nonexistent
 _apt       |    100 | /nonexistent
 messagebus |    101 | /nonexistent
 theia      |   1000 | /home/theia
 mongodb    |    102 | /var/lib/mongodb
 ntp        |    103 | /nonexistent
 cassandra  |    104 | /var/lib/cassandra
 postgres   |    105 | /var/lib/postgresql
(25 rows)
```
SUCCCESS!!



