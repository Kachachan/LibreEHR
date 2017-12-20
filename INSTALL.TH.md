#วิธีการติดตั้ง
อัปเดตล่าสุด: 19 ธันวา 2017

### สารบัญ

**[ไดเรกทอรีเบื้องต้น](#ไดเรกทอรีเบื้องต้น)**

**[การแตกไฟล์](#การแตกไฟล์)**

**[การตั้งค่า](#การตั้งค่า)**

**[การตั้งค่าแอคเซสคอนโทรล](#การตั้งค่าแอคเซสคอนโทรล)**

**[การอัพเกรด](#การอัพเกรด)**

**[การติดตั้งบนวินโดวส์](#การติดตั้งบนวินโดวส์)**

**[FAQ](#faq)**

## ไดเรกทอรีเบื้องต้น

 ท่านสามารถหา documentation ล่าสุดได้ที่ [LibreHealth](http://librehealth.io/).

contrib: สำหรับฟอร์มกับยูทิลิตี้ที่สร้างโดยผู้ใช้
custom: สำหรับสคริปท์และไฟล์ตัวหนังสืออื่นๆที่ถูกใช้งานเยอะ
Documentation: สำหรับ documentation ที่มีประโยชน์
Interface: สำหรับ สคริปท์ User Interface และการตั้งค่า
Library: สำหรับสคริปท์ที่มักใช้ในสคริปท์อื่น
sql: สำหรับรูปภาพใน database
gacl: สำหรับ php-GACL (การควบคุม access)

## การแตกไฟล์

ไฟล์ LibreHealthEHR ที่ดาว์นโหลดมาควรมีชื่อดังนี้

`librehealthehr-<เวอร์ชั่น>-release.tar.gz  -หรือ-  librehealthehr-<เวอร์ชั่น>-release.zip`

เพื่อแตกไฟล์ ท่านควรใช้คำสั่งต่อไปนี้ อันนึงเท่านั้น

`bash# tar -pxvzf librehealthehr-<version>-release.tar.gz`
`bash# unzip librehealthehr-<version>-release.tar.gz`

หากใช้ tar ต้องใช้ flag `-p` เพราะมีเพอร์มิชชั่นหลายอย่างที่เราไม่ควรเปลี่ยน

LibreHealthEHR จะถูกแตกไปยังโฟลเดอร์ `librehealthehr`

อีกวิธีนึง ท่านสามารถดาวน์โหลดซอร์ซโค้ดได้จาก repository ต่อไปนี้ [librehealthehr](https://github.com/LibreHealthIO/librehealthehr) ผ่านคำสั่ง


```
git clone https://github.com/LibreHealthIO/LibreEHR librehealthehr
```


## การตั้งค่า

ก่อนที่จะใช้งาน LibreHealthEHR ท่านต้องติดตั้งเวปเซิร์ฟเวอร์ที่สามารถใช้PHPได้ (เช่น MariaDB (ดีที่สุด) MySQL หรือ Apache) ก่อน

ถ้าท่านยังไม่มี ท่านควรดาว์นโหลดและติดตั้ง  [PHP.](www.php.net) และ เซิร์ฟเวอร์ [Apache](www.apache.org) [MariaDB](https://mariadb.org) (ดีที่สุด) หรือ [MySQL](www.mysql.com) ก่อน 

**ข้อควรระวัง:**

1. PHP เวอร์ชั่น 7.1 ขึ้นไปไม่สามารถใช้ได้

2. MySQL เวอร์ชั่น 5.7 ขึ้นไปนั้นมี strict mode เปิดเป็นค่าเริ่มต้น ท่านต้องปิดมันก่อน ท่านสามารถหาวิธิปิดใด้ในส่วน FAQ

3. LibreHealthEHR ใช้ ความสามารถ PHP หรือ webserver หลายอย่างที่อาจไม่ได้ถูกเปิดไว้ในเครื่องของท่าน ดังต่อไปนี้
   * PHP Index support (เช็คว่า index.php นั้นอยู่ใน Index path ของท่าน ใน httpd.conf)
   * Session Variables
   * PHP libcurl support (ใช้สำหรับการคิดเงิน)

4. หากท่านกำลังติดตั้งบนระบบ linux ท่านควรเช็กว่า ท่านมี dependencies ต่อไปนี้

```
apache2
mysql-server (or if using mariadb, then use 'mariadb-server' instead)
libapache2-mod-php
libtiff-tools
php
php-mysql
php-cli
php-gd
php-gettext
php-xsl
php-curl
php-mcrypt
php-soap
php-json
imagemagick
php-mbstring
php-zip
php-ldap
php-xml
```

ก็อปปี้ โฟลเดอร์ LibreHealthEHR ไปยัง root ของเว็บเซิร์ฟเวอร์ท่าน ตัวอย่างเช่น หากท่านใช้ Mandrake Linux ท่านจะใช้คำสั่ง   
`bash# mv librehealthehr /var/www/html/`

ต่อไป เช็คว่า เวปเซิร์ฟเวอร์ของท่านกำลังทำงานอยู่ และใช้ web-browser เปิด `setup.php` ในโฟล์เดอร์ librehealthehr. หากท่านใดติดตั้ง LibreHealthEHR ใน root directory ของท่าน URL เมื่อเปิด `setup.php` ควรเขียนว่า `http://localhost/librehealthehr/setup.php`.

สคริปท์ setup นี้จะพาท่านติดตั้งโปรแกรม LibreHealthEHR

หน้าแรกของสคริปท์ setup จะตรวจสอบให้แน่ใจว่าผู้ใช้เวปเซิร์ฟเวอร์ (บน Linux ส่วนมากจะเป็น `apache` `www-data` หรือว่า `nobody`) นั้นสามารถ write ทับไฟล์บางไฟล์ได้ โปรแกรม LibreHealthEHR จำเป็นที่จะต้อง write ทับ `librehealthehr/sites/default/sqlconf.php` กับ `librehealthehr/interface/modules/zend_modules/config/application.config.php` ได้

บน Linux ท่านสามารถใช้คำสั่ง `chmod a+w filename` เพื่อเปลี่ยน global write permissions ได้ ท่านจำเป็นที่จะต้องมี write permissions บน directory ต่อไปนี้

```
librehealthehr/gacl/admin/templates_c
librehealthehr/sites/default/edi
librehealthehr/sites/default/era
librehealthehr/sites/default/documents
librehealthehr/interface/main/calendar/modules/PostCalendar/pntemplates/compiled
librehealthehr/interface/main/calendar/modules/PostCalendar/pntemplates/cache. 
```

**ข้อควรระวัง** บน Linux หากชื่อผู้ใช้เวปเซิร์ฟเวอร์คือ `apache` ท่านสามารถใช้คำสั่ง  `chown -R apache:apache ชื่อไดเรคตอรี่` เพื่อให้ global write permissions แก่ directory ในคำสั่งได้ เราแนะนำว่าท่านควรทำการเปลี่ยนแปลงอย่างถาวร 

หาก สคริปท์ setup ขึ้น error เกี่ยวกับ writing permissions แลัว ท่านควรกด 'Check Again' เพื่อลองใหม่หลังจากทำการเปลี่ยนแปลง permissions ตามข้างบน


#### ขั้นที่ 1
ท่านต้องบอก สคริปท์ setup ว่า มันต้องสร้าง database ของมันขึ้นมาเอง หรือว่าท่านได้สร้างไว้แล้ว โดยหากท่านต้องการให้สคริปท์สร้าง database ขึ้นมาเอง สคริปท์จำเป็นที่จะต้องมี MySQL root priveleges.

### ขั้นที่ 2
ท่านจำเป็นที่จะต้องกรอกช่องต่อไปนี้ เกี่ยวกับ MySQL เซิร์ฟเวอร์ และ แผน directory `librehealthehr`

ในช่อง `Server Host`ท่านต้องใส่ที่อยู่ของเซิร์ฟเวอร์ MySQL หากท่านใช้ MySQL และ Apache/PHP บน เซิร์ฟเวอร์เดียวกัน ท่านควรทิ้งช่องนี้เป็น `localhost` หาก MySQL และ Apache/PHP อยู่คนละเซิร์ฟเวอร์ ท่านควรใส่ IP Address (หรือ host name) ของเซิร์ฟเวอร์ที่มี MySQL

ในช่อง `Server Port`ท่านต้องใส่ชื่อพอร์ทที่ท่านใช้เวลาคอมฯท่านเชื่อมต่อกับเซิร์ฟเวอร์ MySQL ผ่าน TCP/IP หากท่านไม่ได้เปลี่ยนพอร์ทตอนติดตั้งเซิร์ฟเวอร์ MySQL ท่านควรทิ้งช่องนี้เป็น `3306`

นช่อง `Database Name`คือชื่อของ database ที่ LibreHealthEHR จะใช้ หากท่านได้เลือกในขั้นตอนที่หนึ่งให้สคริปท์สร้าง database ขึ้นมาใหม่ โปรแกรมจะเติมช่องนี้ให้เอง แต่ถ้าท่านเลือกที่จะใช้ database ที่มีอยู่แล้ว สคริปท์จะนำข้อมูลที่ท่านให้ในช่องนี้ไปติดต่อกับเซิร์ฟเวอร์ MySQL ท่านจำเป็นที่จะต้องใส่ password ที่มีความยาวอย่างน้อยหนึ่งตัวอักษร

ในช่อง `Login Name` คือชื่อของผู้ใช้ MySQL ที่จะเชื่อมต่อกับ database LibreHealthEHR หากท่านได้เลือกในขั้นตอนที่หนึ่งให้สคริปท์สร้าง database ขึ้นมาใหม่ โปรแกรมจะเติมช่องนี้ให้เอง แต่ถ้าท่านเลือกที่จะใช้ database ที่มีอยู่แล้ว สคริปท์จะนำข้อมูลที่ท่านให้ในช่องนี้ไปติดต่อกับเซิร์ฟเวอร์ MySQL
 
ในช่อง `Password` คือ password ของ user MySQL ข้างบนนี้ หากท่านได้เลือกในขั้นตอนที่หนึ่งให้สคริปท์สร้าง database ขึ้นมาใหม่ โปรแกรมจะเติมช่องนี้ให้เอง แต่ถ้าท่านเลือกที่จะใช้ database ที่มีอยู่แล้ว สคริปท์จะนำข้อมูลที่ท่านให้ในช่องนี้ไปติดต่อกับเซิร์ฟเวอร์ MySQL

ในช่อง `Name for Root Account` (ซึ่งจะปรากฏเมื่อสคริปท์จะเป็นคนสร้าง database เท่านั้น) คือชื่อของ root account ของ เซิร์ฟเวอร์ MySQL หากท่านกำลังใช้ localhost ท่านสามารถใช้ชื่อ `root` ในช่องนี้ได้

ในช่อง `Root Pass` (ซึ่งจะปรากฏเมื่อสคริปท์จะเป็นคนสร้าง database เท่านั้น) คือ password ของ root account ของท่าน สคริปท์จะใช้ root user และ root pass ที่ท่านใส่ในที่นี้เพื่อสร้าง database และ user ใหม่ขึ้นมา

ในช่อง `User Hostname` (ซึ่งจะปรากฏเมื่อสคริปท์จะเป็นคนสร้าง database เท่านั้น) คือ hostname ของ Apache/PHP เซิร์ฟเวอร์ ที่ user (ตามที่ใส่ในช่อง `Login Name`) จะใช้ติดต่อกับ database MySql ซึ่งหาก เซิร์ฟเวอร์ MySQL และ Apache/PHP อยู่บนคอมเครื่องเดียวกัน ท่านสามารถใช้ `localhost` ในช่องนี้ได้

ในช่อง `UTF-8 Collation` คือระบบ collation ของเซิร์ฟเวอร์ MySQL ของท่าน หากภาษาที่ท่านต้องการที่จะใช้นั้นอยู่ในเมนู ท่านสามารถเลือกภาษานั้นได้ ไม่เช่นนั้นท่านควรเลือก `General` ท่านไม่ควรเลือก `None` เพราะมันจะบังคับให้โปรแกรมใช้ latin1 encoding ซึ่งไม่ใช่สิ่งที่ท่านต้องการ

ในช่อง `Initial User` คือ username ของผู้ใช้คนแรก และเป็นชื่อที่จะใช้ login เข้าโปรแกรม โดย username นี้จะต้องเป็นคำๆเดียวเท่านั้น (ไม่มีเว้นวรรค)

ในช่อง `Initial User Password` คือ password ของ username ตามที่ใส่ไปในช่องข้างบนนี้

ในช่อง `Initial User's First name` คือชื่อจริงของ user คนแรก ซึ่งข้อมูลในที่นี้สามารถเปลี่ยนได้ในข้อมูลการตั้งค่า (user administration) ได้ในภายหลัง


ในช่อง `Initial User's Last Name` คือนามสกุลของ user คนแรก ซึ่งข้อมูลในที่นี้สามารถเปลี่ยนได้ในข้อมูลการตั้งค่า (user administration) ได้ในภายหลัง

ในช่อง `Initial Group` คือชื่อของกลุ่มผู้ใช้ที่จะถูกสร้างขึ้นเป็นกลุ่มแรก (ควรเป็นชื่อของคลินิกหรือโรงพยาบาล) โดย user คนนึงสามารถอยู่ในกลุ่มผู้ใช้หลายกลุ่มได้ แต่เราแนะนำว่าไม่ควรมีมากกว่าหนึ่งกลุ่มผู้ใช้ต่อออฟฟิศ

#### ขั้นที่ 3

ในขั้นตอนนี้ สคริปท์จะทำการติดตั้ง LibreHealthEHR มันจะสร้าง database และเชื่อมต่อเข้ากับมันเพื่อสร้าง table เบื้องต้น หลังจากนั้น สคริปท์จะ สร้างไฟล์ mysql database configuration ไว้ที่ `librehealthehr/sites/default/sqlconf.php`

หากมี error ใดๆในขั้นตอนนี้ ท่านต้องลบ database และ table ที่ถูกสร้างขึ้นมาก่อน ค่อยลองใหม่อีกครั้ง หากไม่มีปัญหาอะไร ท่านจะเห็นปุ่ม `Continue` ข้างใต้เพจ

#### ขั้นที่ 4

ในขั้นตอนนี้ สคริปท์จะทำการติดตั้ง phpGACL access controls โดยสคริปท์จะสร้าง configuration settings ก่อน แล้วค่อยทำการ configure database ก่อนที่จะมอบ administrator access ให้แก่ `Initial User`

หากมี error ใดๆในขั้นตอนนี้ ท่านต้องลบ database และ table ที่ถูกสร้างขึ้นมาก่อน ค่อยลองใหม่อีกครั้ง หากไม่มีปัญหาอะไร ท่านจะเห็นปุ่ม `Continue` ข้างใต้เพจ

#### ขั้นที่ 5
ท่านจะได้รับคำแนะนำการติดตั้ง PHP เราแนะนำให้ท่านปรินท์คำแนะนำนี้ไว้เพื่อต้องใช้ในภายหลัง โดยท่านจะต้องทำการเปลี่ยนแปลงไฟล์ `php.ini` (หากเป็นไปได้ ที่อยู่ของไฟล์นี้จะปรากฏในสีเขียว)

หากที่อยู่ของไฟล์นี้ไม่ขึ้นมา ท่านจะต้องไปหามัน ซึ่งที่อยู่ของไฟล์นี้จะเปลี่ยนไปตาม OS ของท่าน 

สำหรับ Windows 

* หากใช้ WAMP :
`C:/WAMP/BIN/PHP/php.ini` หรือ  คลิกซ้ายบน icon wampmanager -> PHP -> php.ini

* หากใช้ XAMPP:
`C:\xampp\php\php.ini.`.

บนระบบ Linux ท่านสามารถหามันได้ที่ 
`/etc/php/7.0/php.ini` หรือใกล้เคียง

ท่านควรเปลี่ยนไฟล์ php.ini ดังต่อไปนี้ 
(ท่านควรเซิร์ชชื่อต่อไปนี้ และเปลี่ยนค่า parameter ที่ตั้งไว้อยู่)

```
max_execution_time = 600
max_input_time = 600
max_input_vars = 3000
memory_limit = 512M
post_max_size = 32M
upload_max_filesize = 32M
session.gc_maxlifetime = 14400
short_open_tag = On
display_errors = Off
upload_tmp_dir ถูกตั้งไปยัง default directory ที่เวิร์คบนคอมของท่าน
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT & ~E_NOTICE
file_uploads = On
upload_max_filesize ตั้งไว้ตามที่ท่านต้องใช้
upload_tmp_dir ถูกตั้งไปยัง default directory ที่เวิร์คบนคอมของท่าน
``` 

ท่านต้องเช็คว่าไฟล์  MYSQL /etc/mysql/my.cnf นั้นถูกตั้งค่าดังต่อไปนี้ด้วยเช่นกัน

```
key_buffer_size ตั้งไว้ที่ 1024M
innodb_buffer_pool_size ตั้งไว้ที่ 70% ของแรมบนคอมท่าน.
```

นอกจากนั้นแล้ว ท่านควรเช็คว่า Mysql Strict Mode นั้นถูกปิดไปแล้ว

## วิธีปิด Mysql Strict Mode

เปิดไปที่ `my.ini/my.cnf` ท่านสามารถหาไฟล์นี้ได้ดังต่อไปนี้

* หากใช้ WAMP :
`C:\WAMP\BIN\MYSQL\MySQL Server 5.6\my.ini` หรือ  คลิกซ้ายบน icon wampmanager -> MYSQL -> my.ini

* หากใช้ XAMPP:
`C:\xampp\mysql\bin\my.ini`

บนระบบ Linux ท่านสามารถหามันได้ที่ 
`/etc/mysql` หรือใกล้เคียง

ท่านควรเปลี่ยนไฟล์ `my.ini/my.cnf` ดังต่อไปนี้
	1. หาบรรทัดต่อไปนี้
	   sql-mode = STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
	   (บางที มันอาจถูกเรียกว่า sql_mode)

        2. เปลี่ยนมันเป็น
	   `sql-mode=""` 
           (ไม่ต้องไส่อะไรข้างใน)

	3. Restart ระบบ MySQL


	4. Restart เซิร์ฟเวอร์ WAMPP/XAMPP


(XAMPP)

หากท่านไม่สามารถหา parameter นี้ในไฟล์ my.ini ท่านควรเปิดเซิร์ฟเวอร์ แล้วไปที่ http://localhost/phpmyadmin/ คลิกบน "variables" เซิรชหา "sql mode" และตั้งมันเป็น ""

สุดท้ายแล้ว ท่านต้องทำการรีสตาร์ท Apache service (ท่านสามารถหาวิธีทำได้ใน FAQ)



#### ขั้นที่ 6
ท่านจะได้รับคำแนะนำการติดตั้ง Apache web server เราแนะนำให้ท่านปรินท์คำแนะนำนี้ไว้เผื่อต้องใช้ในภายหลัง โดยคำแนะนำในขั้นตอนนี้จะบอกไห้ท่าน มอบ access แก่ไดเรคทอรี่ librehealthehrwebroot/sites/&ast;/documents, librehealthehrwebroot/sites/&ast;/edi และ librehealthehrwebroot/sites/&ast;/era ที่จะมีไว้เก็บข้อมูลคนไข้ ซึ่งท่านสามารถทำใด้โดยการใส่ file `.htaccess` เข้าไปใน directory ดังกล่าว หรือ จากการเปลี่ยนแปลง configuration ของ apache ก็ได้เช่นกัน

โดยที่อยู่ของไฟล์ configuration ของ apache นั้นแล้วแต่ OS ของท่าน บนระบบ linux ท่านสามารถใช้คำสั่ง `httpd -V` หรือ `apache2ctle  -V` ได้ โดยที่อยู่ของไฟล์ configuration จะเป็น `HTTPD_ROOT` บวกกับ `SERVER_CONFIG_FILE` หากท่านใช้ `XAMPP` บน Windows ท่านสามารถหาไฟล์ configuration ได้ที่ `xampp\apache\conf\httpd.conf`

เพื่อสำเร็จขั้นตอนนี้ ท่านควรก็อปปี้คำสั่งต่อไปนี้ไปวางไว้ท้ายสุดของไฟล์ configuration ของ apache (ท่านควรเช็คว่าท่านใช้ชื่อ directory ที่ถูกต้อง) 

```
<Directory "librehealthehrwebroot">
AllowOverride FileInfo
</Directory>
<Directory "librehealthehrwebroot/sites">
AllowOverride None
</Directory>
<Directory "librehealthehrwebroot/sites/*/documents">
order deny,allow
Deny from all
</Directory>
<Directory "librehealthehrwebroot/sites/*/edi">
order deny,allow
Deny from all
</Directory>
<Directory "librehealthehrwebroot/sites/*/era">
order deny,allow
Deny from all
</Directory>
```

เพื่อให้ท่านมี access ในทุกๆเวปเพจ ท่านควรเปิด Module `mod_rewrite` โดยการใช้คำสั่ง  `a2enmod rewrite` บนเทอร์มินัล

หน้าสุดท้ายของสคริปท์ จะพูดถึงข้อมูลสำคัญอื่นๆที่ท่านควรรู้ เราแนะนำให้ท่านปรินท์คำแนะนำนี้ไว้ เผื่อท่านต้องใช้ในภายหลัง

หลังจาก setup เสร็จแล้ว ท่านสามารถ login ได้ โดยการเชื่อมต่อกับเวปเซิร์ฟเวอร์ที่มีข้อมูลทั้งหมดผ่านเวปบราวเซอร์ ท่านสามารถใช้ username และ password ที่ท่านได้ติดตั้งในขั้นตอนก่อนๆนี้ได้ (ค่าเริ่มต้นคือ `admin`) ต่อไป ท่านควรกดไปที่ `Administrations` แล้วทำการตั้งค่าตามที่ท่านต้องการ ท่านสามารถเพื่ม user และ กลุ่มผู้ใช้ในที่นี้ด้วยเช่นกัน สำหรับข้อมูลเพื่มเติมในการใช้ LibreHealthEHR ท่านสามารถเปิดดูได้ใน User documentation ใน โฟลเดอร์ `Documentation` หรือว่าบนเวปไซต์  [LibreHealth](http://librehealth.io/) 

ท่านควรอ่าน `librehealthehr/sites/default/config.php` สำหรับข้อมูลที่มีประโยชน์เพิ่มเติมด้วยเช่นกัน

ท่านสามารถสร้าง encounter form ของท่านได้ด้วย สำหรับวิธีทำ ท่านควรเช็ค documentation  บนเวปไซต์ [LibreHealth](http://librehealth.io/) ท่านสามารถหาตัวอย่างฟอร์มได้ใน `interface/forms` เช่นกัน

สำหรับ support สำหรับการส่งแฟกซ์ ท่านควรเปิดไปที่ Administration->Globals and custom/faxcover.txt แล้วทำการเปลี่ยนแปลงตามที่ท่านต้องการ นอกจากนั้นแล้ว ท่านต้องมียูทิลิตี้ดังต่อไปนี้ด้วย
* faxstat กับ sendfax จาก client package HylaFAX
* mongrify จาก package ImageMagick 
* tiff2pdf, tiffcp กับ tiffsplit จาก package libtiff-tools
* enscript


## การตั้งค่าแอคเซสคอนโทรล

ตอนท่านติดตั้ง LibreHealthEHR ระบบจะติดตั้ง phpGACL access controls ให้โดยอัตโนมัติ ท่านสามารถทำการเปลี่ยนแปลงได้ใน เมนู admin->acl 

ท่านสามารถเรียนรู้เพิ่มเติมเกี่ยวกับ phpGACL ได้ [ที่นี่]((http://phpgacl.sourceforge.net/) ท่านควรอ่าน phpGACL manual, ไฟล์ `/librehealthehr/Documentation/README.phpgacl.md`, documentation บนเวปไซต์ [LibreHealth](http://librehealth.io/) และ คอมเมนต์ใน `/librehealthehr/library/acl.inc`

## การอัพเกรด

ท่านควรทำการแบคอัปข้อมูล LibreHealthEHR และ database ก่อนจะทำการอัพเกรด

การอัพเกรด LibreHealthEHR สามารถทำได้โดยการวาง directory `librehealthehr` อันใหม่ไปทับ directory `librehealthehr` อันเก่า แล้วทำการก็อปปี้  การตั้งค่าจาก `librehealthehr/sites/default/sqlconf.php` อันเก่า ไปใส่ในไฟล์อันใหม่ด้วย (ท่านไม่ควรก็อปมาทั้งไฟล์)

และในไฟล์ `sqlconf.php` อันนี้ ท่านควรเปลี่ยน `$config = 1` ด้วย (ท่านสามารถหา `$config` ได้ตรงท้ายๆของไฟล์)

directory ต่อไปนี้ ควรถูกก็อปจากเวอร์ชั่นเก่ามาวางในเวอร์ชั่นใหม่ด้วยเช่นกัน

```
librehealthehr/sites/default/documents
librehealthehr/sites/default/era
librehealthehr/sites/default/edi
librehealthehr/sites/default/letter_templates
```

และหากท่านได้ทำการเปลี่ยนแปลงไฟล์อื่นๆได ท่านควรก็อปไฟล์พวกนั้นมาวางทับด้วยเช่นกัน

## การติดตั้งบนวินโดวส์

#### Setup
ท่านจำเป็นที่จะต้องมี  [XAMPP](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/5.6.30/) ในเวอร์ชั่นที่เรารองรับ เพื่อที่จะใช้งาน LibreEHR บน Windows

**ข้อควรระวัง** 

1. ท่านต้องมี php [7.0](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/_) หรือ [5.6](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/) ( เราแนะนำ [5.6.30](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/5.6.30/))

2. php 7.1x นั้น ไม่รองรับในขณะนี้


ท่านควรโคลน LibreEHR [repository](https://github.com/LibreHealthIO/LibreEHR) ด้วยคอนโซลของท่าน (เช่น [GitBash](https://git-for-windows.github.io/) หรือ [Cmder](http://cmder.net/))

หลังจากโคลนเสร็จแล้ว ท่านควรเปิดไฟล์ `php.ini` ใน directory `xampp\php\`  โดยใน `xampp\php` จะมีไฟล์ php อยู่ทั้งหมด 4 อัน ไฟล์ที่ท่านต้องการ จะมี File type เป็น "Configuration Settings" ท่านควรเปลี่ยนไฟล์ php.ini ดังต่อไปนี้ 

```
max_execution_time = 600
max_input_time = 600
max_input_vars = 3000
memory_limit = 512M
post_max_size = 32M
upload_max_filesize = 32M
session.gc_maxlifetime = 14400
short_open_tag = On
display_errors = Off
upload_tmp_dir ถูกตั้งไปยัง default directory ที่เวิร์คบนคอมของท่าน
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT & ~E_NOTICE
``` 
นอกจากนั้นแล้ว ท่านควรเช็คว่า Mysql Strict Mode นั้นถูกปิดไปแล้ว

**[วิธีปิด Mysql strict mode](#how-to-disable-mysql-strict-mode-?)**

#### ขั้นที่ 1
เปิดไปที่ LibreEHR Setup:
localhost/libreEHR/setup.php

**ข้อควรระวัง**
1. เช็คว่า  Apache และ MySQL ใน XAMPP Control panel นั้นเปิดอยู่
2. Apache ต้องอยู่บนพอร์ท 80 และ 443
3. MySQL ต้องอยู่บนพอร์ท 3306

####ขั้นที่ 2

ตั้ง "Site ID:" เป็น "default" แล้วกด continue

![First Step](./Documentation/1_Installing/images/windows_installation/Step_1.png)

เช็คว่าไม่มี undefined index errors ถ้ามี เช็คว่าท่านเปลี่ยน php.ini ไฟล์อย่างถูกต้อง และท่านกำลังใช้ XAMPP เวอร์ชั่นที่ถูกต้อง 

![Second Step](./Documentation/1_Installing/images/windows_installation/Step_2.png)

เลือก "Have setup create the database" และกด continue

![Third Step](./Documentation/1_Installing/images/windows_installation/Step_3.png)

ให้ใส่ "Password" และ "Initial User Password" ท่านสามารถเปลี่ยน  "Initial User" เป็นชื่อของท่าน หรือทิ้งไว้เป็น admin ก็ได้เช่นกัน แล้วกด continue

![Fourth Step](./Documentation/1_Installing/images/windows_installation/Step_4.png)

หากท่านทำตามขั้นตอนก่อนๆนี้อย่างถูกต้อง ท่านสามารถกด continue ต่อๆไปได้เลย 

![Fifth Step](./Documentation/1_Installing/images/windows_installation/Step_5.png)

![Sixth Step](./Documentation/1_Installing/images/windows_installation/Step_6.png)

![Seventh Step](./Documentation/1_Installing/images/windows_installation/Step_7.png)

![Eigth Step](./Documentation/1_Installing/images/windows_installation/Step_8.png)

![Ninth Step](./Documentation/1_Installing/images/windows_installation/Step_9.png)

![Tenth Step](./Documentation/1_Installing/images/windows_installation/Step_10.png)



## FAQ

**Q. ติดตั้ง Apache, MySQL, และ PHP บน Windows ยังไง**

วิธีที่ง่ายที่สุดคือการใช้ [XAMPP Package](https://www.apachefriends.org/index.html) ท่านจำเป็นที่จะต้องก็อปไฟล์ LibreHealthEHR ไปใส่ในโฟลเดอร์ `htdocs`


**Q. ติดตั้ง Apache, MySQL, และ PHP บน Linux ยังไง**

ให้ทำตามคำแนะนำ[นี้](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)

**Q. ฉันได้ error `table doesn't exist`**

เป็นเพราะท่านมี MySQL strict mode ที่ต้องถูกปิดก่อน โดยสามารถทำได้โดยการเปิดไปที่ MySQL configuration และนำบรรทัดต่อไปนี้ไปใส่ข้างหลังสุด  `sql_mode = ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION`

บน linux ท่านสามารถหาไฟล์ config นี้ได้ที่ `/etc/mysql/mysql.conf.d/mysqld.cnf` บน Windows ท่านสามารถหาไฟล์ config ได้ที่ `C:\ProgramData\MySQL\MySQL Server 5.7\my.ini`

**Q. รีสตาร์ท Apache Service ยังไง**

บนเทอร์มินัล linux ให้ใช้คำสั่ง `sudo apache2ctl restart`

บน Windows ให้ใช้ XAMPP control interface จากไฟล์ `xampp-control.exe` ใน `C:\xampp` นอกจากนั้น ท่านสามารถใช้ CMD ไปที่ `C:\xampp\apache\bin` แล้วใช้คำสั่ง `httpd -k restart`

**Q. ฉันต้องการความช่วยเหลือ ทำยังไงดี?**

ท่านสามารถเปิดไปที่ [LibreHealth Chat](https://chat.librehealth.io) หรือ [LibreHealth Support Forum](https://forums.librehealth.io/c/7-support) เพื่อหาความช่วยเหลือเพิ่มเติมได้


