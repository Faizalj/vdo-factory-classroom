# 📁 Section 1: เตรียมระบบก่อนเริ่ม

## 🎯 วัตถุประสงค์
เตรียมและตั้งค่า Credentials ที่จำเป็นสำหรับการเชื่อมต่อ n8n กับ external services

## 📚 หัวข้อที่เรียน

### 1.4.1 สร้าง Credential n8n กับ OpenAI
- การสมัครและรับ API Key จาก OpenAI
- การตั้งค่า OpenAI Credential ใน n8n
- การทดสอบการเชื่อมต่อ
- แนวทางการรักษาความปลอดภัยของ API Key

### 1.4.2 สร้าง Credential n8n กับ Google Service
- การเปิดใช้งาน Google Cloud Platform
- การสร้าง Service Account และ credentials
- การตั้งค่า Google Service Credential ใน n8n
- การกำหนดสิทธิการเข้าถึง Google Services

## 📋 สิ่งที่ต้องเตรียม
- บัญชี OpenAI (หรือเตรียมสมัครใหม่)
- บัญชี Google (สำหรับ Google Cloud Platform)
- n8n ทำงานได้แล้วที่ http://localhost:5678
- การเชื่อมต่ออินเตอร์เน็ตที่เสถียร

## 📂 ไฟล์และ Resource

### สำหรับ OpenAI
- `openai-setup-guide.md` - คู่มือการสมัครและรับ API Key
- `openai-credential-n8n.md` - วิธีตั้งค่าใน n8n
- `openai-testing.md` - การทดสอบการเชื่อมต่อ

### สำหรับ Google Services
- `google-cloud-setup.md` - การตั้งค่า Google Cloud Platform
- `service-account-creation.md` - การสร้าง Service Account
- `google-credential-n8n.md` - วิธีตั้งค่าใน n8n
- `google-permissions.md` - การจัดการสิทธิการเข้าถึง

## 🔑 ขั้นตอนการตั้งค่า OpenAI (1.4.1)

### ขั้นตอนที่ 1: สมัครและรับ API Key
1. เข้าไปที่ https://platform.openai.com/
2. สมัครสมาชิกหรือล็อกอินด้วยบัญชีที่มีอยู่
3. ไปที่ "API Keys" ในเมนู
4. สร้าง API Key ใหม่และคัดลอกเก็บไว้

### ขั้นตอนที่ 2: ตั้งค่าใน n8n
1. เข้าสู่ n8n ที่ http://localhost:5678
2. ไปที่ "Settings" → "Credentials"
3. คลิก "Add Credential" → เลือก "OpenAI"
4. ใส่ API Key ที่ได้รับมา
5. บันทึกและทดสอบการเชื่อมต่อ

### ขั้นตอนที่ 3: ทดสอบ
1. สร้าง workflow ใหม่
2. เพิ่ม OpenAI node
3. เลือก credential ที่สร้างไว้
4. ทดสอบส่งคำขอไปยัง OpenAI

## 🔧 ขั้นตอนการตั้งค่า Google Services (1.4.2)

### ขั้นตอนที่ 1: เตรียม Google Cloud Platform
1. เข้าไปที่ https://console.cloud.google.com/
2. สร้างโครงการใหม่ (Project)
3. เปิดใช้งาน APIs ที่ต้องการ (เช่น Drive, Sheets, Gmail)

### ขั้นตอนที่ 2: สร้าง Service Account
1. ไปที่ "IAM & Admin" → "Service Accounts"
2. คลิก "Create Service Account"
3. ใส่ชื่อและคำอธิบาย
4. กำหนด roles ที่เหมาะสม
5. สร้าง JSON key file

### ขั้นตอนที่ 3: ตั้งค่าใน n8n
1. ใน n8n ไปที่ "Settings" → "Credentials"
2. คลิก "Add Credential" → เลือก "Google Service Account"
3. Upload JSON file หรือ copy-paste เนื้อหา
4. บันทึกและทดสอบการเชื่อมต่อ

## ⚠️ ข้อควรระวัง

### สำหรับ OpenAI
- เก็บ API Key อย่างปลอดภัย อย่าแชร์ให้ผู้อื่น
- ตรวจสอบการใช้งานและค่าใช้จ่ายเป็นประจำ
- ตั้งค่า usage limits เพื่อควบคุมค่าใช้จ่าย

### สำหรับ Google Services
- ระวังการให้สิทธิมากเกินไป
- เก็บ JSON credential file อย่างปลอดภัย
- ใช้งาน least privilege principle

## 🧪 การทดสอบ Credentials

### ทดสอบ OpenAI
```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {"role": "user", "content": "สวัสดี"}
  ]
}
```

### ทดสอบ Google Services
- ทดสอบอ่านไฟล์จาก Google Drive
- ทดสอบการเขียนข้อมูลลง Google Sheets
- ทดสอบการส่งอีเมลผ่าน Gmail API

## 🔍 การแก้ไขปัญหา

### OpenAI Issues
- **Invalid API Key**: ตรวจสอบ API Key ถูกต้อง
- **Quota Exceeded**: ตรวจสอบ usage และ billing
- **Model Not Available**: เลือก model ที่รองรับ

### Google Services Issues
- **Authentication Failed**: ตรวจสอบ JSON credential
- **Permission Denied**: ตรวจสอบ roles และ permissions
- **API Not Enabled**: เปิดใช้งาน APIs ที่จำเป็น

## ⏱️ เวลาที่ใช้โดยประมาณ
- OpenAI setup: 20-30 นาที
- Google Services setup: 30-45 นาที
- การทดสอบ: 15-20 นาที

**รวมทั้งหมด: 1-1.5 ชั่วโมง**

## ✅ เป้าหมายการเรียนรู้
เมื่อจบ Section นี้ นักเรียนจะสามารถ:
- ตั้งค่า OpenAI API credentials ใน n8n ได้
- ตั้งค่า Google Service credentials ใน n8n ได้
- เข้าใจการรักษาความปลอดภัยของ API credentials
- ทดสอบการเชื่อมต่อกับ external services ได้
- พร้อมสำหรับการสร้าง automation workflows

---
[← Section 0](../Section-0/README.md) | [กลับไปหน้าหลัก](../README.md) | [ไปยัง Section 2 →](../Section-2/README.md)