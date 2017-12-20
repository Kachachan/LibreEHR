# ยินดีต้อนรับสู่ LibreHealth EHR

LibreHealth EHR เป็นโปรแกรมเก็บข้อมูลผู้ป่วยและบริหารโรงพยาบาล ที่ฟรีและโอเพนซอร์ซ

จุดประสงค์ของ LibreHealth คือการช่วยส่งเสริมการดูแลรักษาทางการแพทย์ที่มีคุณภาพให้แก่ทุกๆ คน โดยไม่คำนึงถึง เชื้อชาติ สถานะความเป็นอยู่ หรือ ที่อยู่ ผ่านการมอบซอฟต์แวร์ช่วยเหลือทางแพทย์ให้แก่คลินิกและหมอทั่วโลกโดยไม่คิดค่าใช้จ่าย

ซึ่งซอฟต์แวร์ดังกล่าวนี้ ได้ถูกดีไซน์ให้ช่วยคลินิกประหยัดเงินและเวลา ทำให้แพทย์มีเวลากับคนไข้มากขึ้นและสามารถมอบการรักษาที่มีคุณภาพมากกว่าได้

พวกเราหลายคนเคยทำงานร่วมกับ OpenEMR และปลื้มในผลงานที่ OpenEMR เคยทำในปีที่ผ่านๆมา มันเป็นเกียรติของพวกเรา ที่จะส่งเสริมสิ่งที่ดีของ OpenEMR และแบ่งปันผลงานของพวกเรา เพื่อเปลี่ยนแปลงระบบเก่าให้ออกมาดียิ่งกว่าเดิม

พวกเรากำลังทำงานอย่างใกล้ชิดกับ โปรเจ็ค LibreHealth [LibreHealth.io](http://LibreHealth.io) ซึ่งเป็นองค์กรที่รวมโปแกรมทางสุขภาพที่มีจุดประสงค์คล้ายๆกันไว้ด้วยกัน 

โปรเจ็คของเรา อยูภายใต้สัญญาอนุญาต  Mozilla Public License Version 2

โค้ดที่มาจาก โปรเจ็ค OpenEMR นั้น อยู่ภายใต้สัญญาอนุญาต  GPL 2 หรือ มากกว่า

โปรเจ็คนี้ เป็นส่วนหนึ่งของ Software Freedom Conservancy family [sfconservancy.org](http://sfconservancy.org)

*** ขอขอบคุณการสนับสนุนของท่าน ***

# การช่วยเขียนโค้ด

เรายินดีมาก หากท่านต้องการที่จะช่วยเขียนโค้ด ท่านสามารถเปิด [Issue tracker](https://github.com/LibreHealthIO/LibreEHR/issues) เพื่อช่วยในปัญหาที่เราค้นพบแล้ว หรือท่านก็สามารถคิดไอเดียและโค้ดของท่านขึ้นมาเองก็ได้ เมื่อท่านเขียนโค้ดเสร็จแล้ว กรุณาเปิด [Pull Request](https://github.com/LibreHealthIO/LibreEHR/pulls) เพื่อที่จะส่งโค้ดมาให้พวกเรา

## การพัฒนาบนคอมฯของท่าน 

สำหรับรายละเอียดและคำแนะนำในการติดตั้งทีละขั้นตอนโปรดเปิด [คำแนะนำในการติดตั้ง](/INSTALL.TH.md)

## Windows ::

ก่อนอื่นเลย เช็คให้ดีว่าท่านมีเซิร์ฟเวอร์ [WAMP](http://www.wampserver.com/en/) หรือ [XAMPP](https://www.apachefriends.org/index.html) บนเครื่องของท่าน และ โซนเวลาของท่านถูกตั้งอย่างถูกต้อง

เปิดไปที่ `php.ini` ท่านสามารถหาไฟล์ `php.ini` ได้ดังต่อไปนี้

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
	   sql-mode="" 
           (ไม่ต้องไส่อะไรข้างใน)

	3. Restart ระบบ MySQL


	4. Restart เซิร์ฟเวอร์ WAMPP/XAMPP


  


(XAMPP)

หากท่านไม่สามารถหา parameter นี้ในไฟล์ my.ini ท่านควรเปิดเซิร์ฟเวอร์ แล้วไปที่ http://localhost/phpmyadmin/ คลิกบน "variables" เซิรชหา "sql mode" และตั้งมันเป็น ""

ท่านสามารถ fork กับ clone repository นี้บนคอมของท่านได้ ท่านควร
- clone repository นี้ก่อน
- เปิด index.php แล้วทำตามคำแนะนำบนหน้า setup

การติดตั้งอาจใช้เวลานานกว่าปกติบนคอมบางเครื่อง หากเป็นเช่นนั้น ท่านควรเปิดไปที่ php.ini เพื่อเพื่ม `max_execution_time` รีสตาร์ทเซิร์ฟเวอร์ของท่าน แล้วลองใหม่อีกครั้ง







