# 📖 Complete Video Factory - คู่มือการใช้งานทีละขั้นตอน

## 🎯 ภาพรวม
คู่มือนี้จะพาคุณทำความเข้าใจและใช้งาน Complete Video Factory workflow ทีละขั้นตอน

## 📋 Pre-requisites Checklist

### ✅ Credentials ที่ต้องมี
- [ ] **OpenAI API Key** - สำหรับสร้าง content
- [ ] **Google Sheets OAuth2** - สำหรับจัดการข้อมูล  
- [ ] **Together AI API Key** - สำหรับ FLUX image generation
- [ ] **YouTube OAuth2** - สำหรับอัปโหลดวิดีโอ (optional)

### ✅ Services ที่ต้องรัน
- [ ] **Video Factory API** - http://vdo-factory:8000
- [ ] **n8n** - http://localhost:5678
- [ ] **Docker Compose** - containers ทำงานปกติ

## 🚀 ขั้นตอนการเริ่มต้น

### Step 1: Import Workflow
1. ดาวน์โหลด `complete-video-factory.json`
2. เปิด n8n → คลิก menu (≡) → Templates
3. คลิก "Import from JSON"
4. Select file และ Import

### Step 2: ตั้งค่า Credentials
ไปที่ Settings → Credentials และเพิ่ม:

#### OpenAI API
```
Name: OpenAI account
Type: OpenAI
API Key: sk-xxxxxxxxxxxxxxxx
```

#### Google Sheets OAuth2
```  
Name: Google Sheets account
Type: Google Sheets OAuth2 API
- ทำการ OAuth authentication
```

#### Together AI (HTTP Bearer Auth)
```
Name: Together AI test  
Type: HTTP Bearer Auth
Token: xxxxxxxxxxxxxxxx
```

#### YouTube OAuth2 (Optional)
```
Name: YouTube account
Type: YouTube OAuth2 API  
- ทำการ OAuth authentication
```

### Step 3: สร้าง Google Sheets
สร้าง 2 Google Sheets:

#### Sheet 1: Reddit Stories (เก็บข้อมูลจาก Reddit)
Columns: `id`, `title`, `content`, `video_id`

#### Sheet 2: Video Processing (ติดตามสถานะวิดีโอ)  
Columns: `id`, `title`, `content`, `status`, `video_id`

## ⚙️ การตั้งค่า Configuration

### แก้ไข "Configure me" Node
```javascript
{
  // Video Factory API URL  
  "vdo_factory_api_url": "http://vdo-factory:8000",
  
  // Reddit Settings
  "subreddit": "selfimprovement",        // เปลี่ยนตาม subreddit ที่ต้องการ
  "update_reddit_stories": false,       // true = ดึงข้อมูลใหม่จาก Reddit
  
  // Content Settings  
  "content_type": "motivational speech", // ประเภทเนื้อหา
  
  // TTS Settings
  "tts_engine": "kokoro",               // หรือ "chatterbox"
  "kokoro_voice": "af_heart",           // เสียงที่ใช้
  "kokoro_speed": 1,                    // ความเร็วการพูด
  
  // Chatterbox TTS (ถ้าใช้)
  "chatterbox_exaggeration": 0.5,
  "chatterbox_cfg_weight": 0.5, 
  "chatterbox_temperature": 0.8,
  "chatterbox_clone_voice_id": "",      // สำหรับ voice cloning
  
  // Background Music
  "background_music_id": "",            // ID ของเพลงพื้นหลัง 
  "background_music_volume": 0.2,       // ระดับเสียงเพลง
  
  // Art Style สำหรับสร้างภาพ
  "art_style": "Create an artistic image in a vibrant, emotionally uplifting illustration style..."
}
```

## 🔄 การทำงานของ Workflow

### Phase 1: Initialization (1-3 นาที)
```
1. Manual Trigger → เริ่มต้น
2. Configure me → โหลดการตั้งค่า
3. Health Check → ทดสอบ Video Factory API
4. Get Data → ดึงข้อมูลจาก Reddit/Sheets
```

### Phase 2: Content Creation (2-5 นาที)
```
5. Create motivational speech → OpenAI สร้าง script
6. Cleanup text → ทำความสะอาดข้อความ
7. Create the script → แบ่งเป็น scenes + image prompts
8. Split Out → แยกแต่ละ scene
```

### Phase 3: Media Generation Loop (10-20 นาที)
สำหรับแต่ละ scene:
```
9. Loop Over Items → วนลูปแต่ละ scene
10. Generate AI image → FLUX สร้างภาพ (1-2 นาที/ภาพ)
11. Upload image → อัปโหลดไปเซิร์ฟเวอร์
12. TTS Generation → สร้างเสียงพูด (30วินาที-2นาที)
13. Create Video → สร้างวิดีโอพร้อม caption (1-3 นาที)
```

### Phase 4: Post-production (3-7 นาที)
```
14. Combine → รวมข้อมูลทุก scenes
15. Merge Videos → รวมเป็นวิดีโอเดียว (2-5 นาที)
16. Save Status → บันทึกสถานะใน Sheets
```

### Phase 5: Distribution (Optional)
```
17. Download Video → ดาวน์โหลดวิดีโอ
18. YouTube Upload → อัปโหลดไปยัง YouTube
19. Cleanup → ลบไฟล์ temporary
```

## 🎛️ การควบคุมและ Monitoring

### การดู Progress
- **n8n Executions**: ดูสถานะการทำงาน real-time
- **Node Data**: คลิก node เพื่อดูข้อมูลที่ผ่าน
- **Logs**: ตรวจสอบ errors ใน execution logs

### การหยุด/ยกเลิก
- **Stop Execution**: หยุดการทำงานทันที
- **Manual Resume**: ต่อจากจุดที่หยุด (บาง nodes)

### Wait Nodes และ Timing
- **TTS Wait**: รอการสร้างเสียงเสร็จ (retry ทุก 1 นาที)
- **Video Wait**: รอการสร้างวิดีโอเสร็จ (retry ทุก 1 นาที)  
- **Merge Wait**: รอการรวมวิดีโอเสร็จ (retry ทุก 1 นาที)
- **Rate Limit Wait**: รอ 10 วินาทีเมื่อ rate limit

## 🧪 การทดสอบเบื้องต้น

### Test 1: Health Check
1. รัน workflow จนถึง "HTTP Request" node
2. ควรได้ response 200 จาก `/health`

### Test 2: Content Generation Only  
1. ปิด nodes หลังจาก "Split Out"
2. ทดสอบการสร้าง content และ scenes

### Test 3: Single Scene
1. Edit "Loop Over Items" ให้ process เฉพาะ 1 item
2. ทดสอบการสร้างภาพ + เสียง + วิดีโอ

### Test 4: Full Pipeline (Small)
1. ใช้ข้อมูลสั้น ๆ 2-3 scenes
2. รัน workflow เต็มรูปแบบ

## ⚠️ Common Issues และการแก้ไข

### Issue: "Server isn't ready"
```
Cause: Video Factory API ไม่ทำงาน
Fix: 
- ตรวจสอบ docker-compose ps
- รีสตาร์ท docker-compose up -d
```

### Issue: "Rate limit exceeded" (Together AI)
```
Cause: เรียก FLUX API บ่อยเกินไป
Fix:
- รอสักครู่แล้วลองใหม่
- ลดจำนวน scenes
```

### Issue: "TTS generation failed"
```
Cause: ข้อความยาวเกินไป หรือมีอักขระพิเศษ
Fix:
- ตรวจสอบข้อความใน scene
- ลดความยาวข้อความ
```

### Issue: "Video merge timeout"
```
Cause: วิดีโอมีขนาดใหญ่ หรือ scenes เยอะ
Fix:
- เพิ่ม wait time
- ลดความยาววิดีโอ
```

### Issue: YouTube upload failed
```
Cause: ไฟล์ใหญ่เกินไป หรือ credential ผิด
Fix:
- ตรวจสอบ YouTube quota
- Re-authenticate YouTube credential
```

## 📊 Performance Optimization

### การลดเวลาประมวลผล
1. **ลดจำนวน scenes**: 3-5 scenes สำหรับทดสอบ
2. **ปรับ image size**: ลดเป็น 512x512 เพื่อความเร็ว
3. **ใช้ Kokoro TTS**: เร็วกว่า Chatterbox
4. **ปิด background music**: ลดขั้นตอนการ merge

### การลด API Costs
1. **Test โหมด**: ใช้ข้อความสั้น ๆ
2. **Batch processing**: รวมหลาย scenes
3. **Cache results**: เก็บผลลัพธ์ที่ดีไว้
4. **Monitor usage**: ตรวจสอบ API usage เป็นประจำ

## 💡 Best Practices

### สำหรับ Content
- ใช้ข้อความที่ชัดเจน ไม่ซับซ้อน
- หลีกเลี่ยงอักขระพิเศษใน TTS
- กำหนดความยาว scene 10-30 วินาที

### สำหรับ Images  
- ใช้ prompts ที่ชัดเจน มีรายละเอียด
- หลีกเลี่ยงเนื้อหาที่อาจถูกบล็อก
- ทดสอบ art style ก่อนรันจริง

### สำหรับ Videos
- ตั้งค่า background music ให้เหมาะสม
- ใช้ TTS voice ที่เข้ากับเนื้อหา
- ทดสอบ TTS ก่อนสร้างวิดีโอ

### การจัดการไฟล์
- เปิด cleanup เพื่อประหยัดพื้นที่
- สำรองไฟล์สำคัญก่อนลบ
- Monitor disk space บนเซิร์ฟเวอร์

## 🔄 Workflow Variations

### สำหรับ Educational Content
```javascript
{
  "content_type": "educational explanation",
  "subreddit": "explainlikeimfive", 
  "art_style": "clean, educational illustration style..."
}
```

### สำหรับ News Summary
```javascript
{
  "content_type": "news summary",
  "subreddit": "worldnews",
  "kokoro_voice": "af_neutral"
}
```

### สำหรับ Entertainment
```javascript
{
  "content_type": "entertaining story",
  "subreddit": "tifu",
  "art_style": "colorful, fun cartoon style..."
}
```

## 📈 Advanced Features

### Custom Voice Cloning (Chatterbox)
1. อัปโหลด sample audio ไปยัง Video Factory
2. ใส่ `file_id` ใน `chatterbox_clone_voice_id`
3. เปลี่ยน `tts_engine` เป็น `"chatterbox"`

### Background Music
1. อัปโหลดเพลงไปยัง Video Factory
2. ใส่ `file_id` ใน `background_music_id`  
3. ปรับ `background_music_volume` (0.1-0.5)

### Multiple Subreddits
1. แก้ไข "Get stories from reddit" node
2. ใช้ loop เพื่อดึงจากหลาย subreddits
3. Aggregate ข้อมูลก่อนประมวลผล

ตัวอย่าง workflow นี้แสดงให้เห็นถึงศักยภาพของการสร้างวิดีโออัตโนมัติที่สมบูรณ์ สามารถนำไปปรับใช้กับความต้องการที่หลากหลายได้!