# ใบงานที่ 8-3  ใช้ component Sevensegment แสดงผลพร้อมกัน 2 หลัก

# 3 กลับไปแก้ไข project  `SevenSegment2Digits`

3.1 ในไฟล์ main.cpp ให้เพิ่มตัวแปร `uint8_t counter = 0` 

3.2 ใน loop while ให้แสดงเลขหลักหน่วยบน seven segment ตัวที่สอง และหลักสิบ บน seven segment ตัวที่ 1

การดึงหลักหน่วยออกจากตัวนับ ทำได้โดยการ mod ด้วย 10 `(counter % 10)`

การดึงหลักสิบออกจากตัวนับ ทำได้โดยการหารด้วย 10 `(counter / 10)`
 
![alt text](./Pictures/image-10.png)

3.3 ทดสอบ build และรันโปรแกรม จะต้องปรากฏเลข 15 บน seven segment

3.4 ถ้าปรากฏผลดังกล่าวแสดงว่า component นี้พร้อมใช้งานแล้ว ให้ push ขึ้นบน  github 


วิดิโอ ไฟล์งาน

https://vt.tiktok.com/ZS2uhEG2H/


งาน https://github.com/AnchisaPhetnoi/8-1-Seven-Segment-2-Digits.git

งาน https://github.com/AnchisaPhetnoi/Lab-07-Seven-segment-Component-develope.git
