# Modulux CPQ — GitHub Pages + Google Sheets

## โครงสร้าง
- `index.html` — เว็บไซต์ลูกค้าและแอดมิน (เดิมจาก Index.html)
- `config.js` — URL ของ Google Apps Script API
- `Code.gs` — Backend API ที่อ่าน/เขียน Google Sheets และอัปโหลดรูปไป Google Drive

> GitHub Pages เป็น Frontend เท่านั้น ส่วน Code.gs ต้อง Deploy เป็น Google Apps Script Web App แยกต่างหาก

## 1) ตั้งค่า Google Apps Script
1. เปิด Google Sheet เดิม: https://docs.google.com/spreadsheets/d/1HHN0p3F7zK6vzRCDnfMlI1Owkvw_Bl32-Hg2zjw6fik/edit
2. ไปที่ Extensions → Apps Script
3. สำรอง Code.gs เดิม แล้วแทนที่ด้วย `Code.gs` ในโฟลเดอร์นี้
4. แก้รหัสในฟังก์ชัน `setAdminKey()` เป็นรหัสสุ่มยาวอย่างน้อย 16 ตัวอักษร (ห้ามใช้รหัสที่เดาง่าย) แล้วกด Save
5. เลือกฟังก์ชัน `setupSheets` แล้วกด Run เพื่ออนุญาตสิทธิ์ Sheets และ Drive
6. เลือกฟังก์ชัน `setAdminKey` แล้วกด Run หนึ่งครั้ง
7. Deploy → New deployment → Select type: Web app
8. Execute as: Me; Who has access: Anyone; กด Deploy และอนุญาตสิทธิ์
9. คัดลอก URL ที่ลงท้ายด้วย `/exec`

## 2) ตั้งค่าไฟล์เว็บ
1. เปิด `config.js`
2. เปลี่ยน `PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE` เป็น URL `/exec` ที่ได้จากขั้นตอนก่อนหน้า
3. บันทึกไฟล์

## 3) อัปโหลดขึ้น GitHub
1. สร้าง Repository ใหม่ใน GitHub
2. เลือก Public (จำเป็นสำหรับ GitHub Pages ฟรี)
3. กด Add file → Upload files
4. ลาก `index.html`, `config.js` และไฟล์อื่นในโฟลเดอร์นี้ลงไป (ไม่ต้องอัปโหลด Code.gs ไปหน้าเว็บ; เก็บไว้ใน repo ได้เพื่อสำรอง แต่ต้องนำไปวางใน Apps Script ด้วย)
5. กด Commit changes
6. ไปที่ Settings → Pages
7. Build and deployment: Deploy from a branch
8. Branch: `main`, Folder: `/ (root)` แล้วกด Save
9. รอ GitHub แสดง URL เว็บไซต์

## 4) ลิงก์ใช้งาน
- หน้าลูกค้า: `https://ชื่อผู้ใช้.github.io/ชื่อ-repository/`
- หน้าแอดมิน: `https://ชื่อผู้ใช้.github.io/ชื่อ-repository/?page=admin`
- หน้าแอดมินจะถามรหัสที่ตั้งไว้ใน `setAdminKey()` (เก็บไว้ใน session ของเบราว์เซอร์)

## ข้อควรทราบ
- การอ่านข้อมูลใช้ JSONP; การบันทึก/ลบ/อัปโหลดใช้ POST ผ่าน iframe เพื่อหลีกเลี่ยงข้อจำกัด CORS ของ Apps Script
- ข้อมูลเดิมใน Google Sheets ไม่ถูกลบหรือย้าย
- รูปภาพยังถูกเก็บใน Google Drive โฟลเดอร์ `Modulux_Images` และสร้างลิงก์แชร์ Anyone with link ตามพฤติกรรมเดิม
- Apps Script Web App ถูกตั้งให้เข้าถึงได้จาก Anyone เพื่อให้ GitHub Pages เรียก API ได้ แต่การอ่าน/เขียนข้อมูลแอดมินมีการตรวจ ADMIN_KEY ฝั่งเซิร์ฟเวอร์
- ลูกค้าสามารถบันทึกประวัติรายการของตนเองผ่าน `saveCustomerRecord` ตามระบบเดิม
- ก่อนเปิดใช้งานจริง ควรทดสอบการโหลดข้อมูล, เพิ่ม/แก้ไข/ลบ Set และ Option, บันทึกลูกค้า และอัปโหลดรูปภาพ
- หากแก้ไข Code.gs ภายหลัง ต้อง Deploy → Manage deployments → Edit → New version → Deploy เพื่อให้ API ใช้โค้ดล่าสุด
