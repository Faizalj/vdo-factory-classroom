# 🎬 Video Factory Classroom
## 📚 คอร์สเรียนสร้างวิดีโออัตโนมัติ

ยินดีต้อนรับสู่คอร์สเรียนออนไลน์ที่จะสอนคุณสร้างระบบผลิตวิดีโออัตโนมัติด้วย Docker และ n8n!

## 🎯 จุดประสงค์ของคอร์ส
- เรียนรู้การติดตั้งและใช้งาน Docker Desktop บน Windows
- สร้างระบบ automation ด้วย n8n
- เชื่อมต่อ AI tools เพื่อสร้างวิดีโออัตโนมัติ
- จัดการระบบผ่าน Docker Compose

## 🛠️ เครื่องมือที่ใช้
- **Docker Desktop** สำหรับ Windows
- **n8n** - เครื่องมือ automation workflow
- **AI Video Factory** - ระบบสร้างวิดีโอ AI

## 📋 ข้อกำหนดเบื้องต้น
- Windows 10/11 (64-bit)
- RAM อย่างน้อย 8GB (แนะนำ 16GB)
- Docker Desktop ติดตั้งแล้ว
- การเชื่อมต่ออินเตอร์เน็ตที่เสถียร

## 🗂️ โครงสร้างคอร์ส

### 📁 Section 0: แนะนำก่อนเรียน
- 0.1 แนะนำคอร์ส และ แนะนำตัวผู้สอน

### 📁 Section 1: เตรียมระบบก่อนเริ่ม
- 1.4.1 สร้าง Credential n8n กับ OpenAI
- 1.4.2 สร้าง Credential n8n กับ Google Service

### 📁 Section 2: ลงมือสร้าง "โรงงานคลิปอัตโนมัติ"
- การสร้างระบบผลิตวิดีโออัตโนมัติแบบครบวงจร

## 🚀 เริ่มต้นใช้งาน

### ขั้นตอนที่ 1: ดาวน์โหลดไฟล์
```bash
git clone https://github.com/Faizalj/vdo-factory-classroom.git
cd vdo-factory-classroom
```

### ขั้นตอนที่ 2: เตรียมไฟล์
นำไฟล์จากโฟลเดอร์ `Resource/` ไปไว้ในโฟลเดอร์ที่คุณต้องการใช้งาน

### ขั้นตอนที่ 3: เริ่มระบบ
```bash
docker-compose up -d
```

### ขั้นตอนที่ 4: เข้าสู่ระบบ
- **n8n**: http://localhost:5678
  - Username: `admin`
  - Password: `password`
- **Video Factory API**: http://localhost:8000

## 🔄 การอัปเดตระบบ
เมื่อมีเวอร์ชันใหม่ ให้รันคำสั่งนี้:
```bash
docker pull faizalj/ai-toolstack-vdo-factory:latest
docker-compose down && docker-compose up -d
```

## 📞 การติดต่อ
หากมีคำถามหรือปัญหา สามารถสอบถามได้ผ่านช่องทางการเรียน

## ⚠️ ข้อสำคัญ
- ตรวจสอบว่า Docker Desktop ทำงานอยู่ก่อนรันคำสั่ง
- volume ของ n8n จะเก็บข้อมูลไว้ ไม่ต้องกังวลเรื่องข้อมูลหาย
- ไม่ควรแก้ไขไฟล์ docker-compose.yml โดยไม่จำเป็น

---
*คอร์สนี้จัดทำขึ้นเพื่อการศึกษา โปรดใช้อย่างรับผิดชอบ*