# 🎤 Kokoro Voice Troubleshooting Guide

## 🚨 ปัญหาที่พบบ่อย

### ปัญหา: เสียงอื่นนอกจาก `af_heart` ไม่ทำงาน

**อาการ**: นักเรียนลองใช้เสียงอื่น แต่ได้เฉพาะ `af_heart` เท่านั้น

## ✅ เสียงที่ยืนยันแล้วว่าใช้ได้

- **`af_heart`** - เสียงหญิงอบอุ่น ✅

## ⚠️ เสียงที่อาจมีปัญหา

- `af_neutral` - เสียงหญิงเป็นกลาง
- `af_sarah` - เสียงหญิงชัดเจน  
- `am_casual` - เสียงชายสบาย ๆ
- `am_formal` - เสียงชายเป็นทางการ
- `am_david` - เสียงชายมีพลัง

## 🔍 สาเหตุที่เป็นไปได้

### 1. Voice Models ไม่ได้ติดตั้งครบ
Backend อาจมี voice models เฉพาะ `af_heart` เท่านั้น

### 2. ชื่อเสียงไม่ถูกต้อง
ต้องใช้ชื่อที่ถูกต้องตรงตัว:
```javascript
// ✅ ถูกต้อง
"kokoro_voice": "af_heart"

// ❌ ผิด
"kokoro_voice": "af_Heart"  // ตัวใหญ่ผิด
"kokoro_voice": "af heart"  // มีช่องว่าง
"kokoro_voice": "female"    // ชื่อไม่ถูก
```

### 3. Backend Configuration ไม่ครบ
Voice Factory อาจตั้งค่าให้รองรับเฉพาะบางเสียง

## 🧪 วิธีทดสอบเสียงอื่น ๆ

### ขั้นตอนที่ 1: ทดสอบทีละเสียง
```javascript
// ใน Configure me node
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_neutral",  // เปลี่ยนทีละเสียง
  "kokoro_speed": 1.0
}
```

### ขั้นตอนที่ 2: ใช้ข้อความสั้น ๆ
ทดสอบด้วยข้อความสั้น เช่น "Hello, this is a test."

### ขั้นตอนที่ 3: ตรวจสอบ Error Messages
ดู error ใน n8n execution log เมื่อใช้เสียงอื่น

## 💡 คำแนะนำสำหรับนักเรียน

### แนะนำระยะสั้น
**ใช้ `af_heart` เป็นหลัก** เพราะยืนยันแล้วว่าใช้ได้และมีคุณภาพดี

### การปรับแต่ง af_heart
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_heart",
  "kokoro_speed": 0.9  // ช้าลง เหมาะกับเนื้อหาสงบ
}
```

```javascript
{
  "tts_engine": "kokoro", 
  "kokoro_voice": "af_heart",
  "kokoro_speed": 1.2  // เร็วขึ้น เหมาะกับเนื้อหาที่มีพลัง
}
```

## 🔧 สำหรับผู้ดูแลระบบ

### ตรวจสอบ Voice Models
```bash
# เข้าไปใน Video Factory container
docker exec -it vdo-factory bash

# ตรวจสอบไฟล์ voice models
ls -la /app/models/kokoro/
```

### ตรวจสอบ Configuration
```bash
# ดู configuration ของ TTS
cat /app/config/tts_config.json
```

### ตรวจสอบ Logs
```bash
# ดู logs เมื่อใช้เสียงที่ไม่ทำงาน
docker logs vdo-factory | grep -i "voice\|tts\|kokoro"
```

## 📋 Checklist การแก้ปัญหา

- [ ] ตรวจสอบชื่อเสียงว่าสะกดถูกต้อง
- [ ] ลองใช้ `af_heart` ก่อนเป็น baseline
- [ ] ตรวจสอบ error messages ใน n8n
- [ ] ทดสอบด้วยข้อความสั้น ๆ
- [ ] ตรวจสอบ Voice Factory logs
- [ ] ติดต่อผู้ดูแลระบบหากจำเป็น

## 🎯 สรุป

**สำหรับตอนนี้**: ให้นักเรียนใช้ `af_heart` เป็นหลัก เพราะยืนยันแล้วว่าใช้ได้

**สำหรับอนาคต**: ตรวจสอบและติดตั้ง voice models เพิ่มเติมใน backend

การมี voice หลากหลายจะช่วยให้สร้างเนื้อหาที่น่าสนใจมากขึ้น แต่ `af_heart` ก็เพียงพอสำหรับการเรียนรู้และสร้างคอนเทนต์คุณภาพได้! 🎬