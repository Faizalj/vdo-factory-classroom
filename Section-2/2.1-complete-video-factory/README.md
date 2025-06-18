# 📁 Section 2.1: Complete Video Factory Workflow

## 🎯 วัตถุประสงค์
เรียนรู้การใช้งาน workflow สมบูรณ์ที่สร้างวิดีโออัตโนมัติแบบครบวงจร ตั้งแต่การดึงข้อมูลจาก Reddit ไปจนถึงการอัปโหลดวิดีโอไปยัง YouTube

## 🏭 ภาพรวม Complete Video Factory

### สิ่งที่ Workflow นี้ทำได้:
- 📥 ดึงข้อมูลจาก Reddit หรือ Google Sheets อัตโนมัติ
- 🤖 สร้าง script แบบ motivational speech ด้วย OpenAI
- 🎨 สร้างภาพประกอบด้วย AI (FLUX model)
- 🎤 แปลงข้อความเป็นเสียงพูด (TTS)
- 🎬 สร้างวิดีโอพร้อม caption อัตโนมัติ
- 🔄 รวมวิดีโอหลาย ๆ ฉากเป็นวิดีโอเดียว
- 📤 อัปโหลดไปยัง YouTube (ถ้าต้องการ)
- 🗑️ ทำความสะอาดไฟล์ temporary

## 📋 สิ่งที่ต้องเตรียม
- OpenAI credential (API key)
- Google Sheets credential 
- Together AI credential (สำหรับ FLUX image generation)
- YouTube credential (ถ้าต้องการอัปโหลด)
- Video Factory API ทำงานได้ที่ http://vdo-factory:8000

## 📂 ไฟล์ในส่วนนี้
- `complete-video-factory.json` - Workflow สมบูรณ์พร้อมใช้งาน
- `workflow-guide.md` - คำแนะนำการใช้งานทีละขั้นตอน
- `configuration-guide.md` - การตั้งค่า parameters แบบละเอียด
- `art-style-templates.md` - ตัวอย่าง Art Style prompts พร้อมใช้งาน
- `subreddit-content-ideas.md` - ไอเดีย subreddits และ content types สำหรับคลิปสั้น
- `verified-subreddits.md` - รายการ subreddits ที่ยืนยันแล้วว่าใช้ได้จริง
- `troubleshooting.md` - แก้ไขปัญหาที่พบบ่อย (Coming Soon)

## 🚀 ขั้นตอนการใช้งาน

### ขั้นตอนที่ 1: Import Workflow
1. ดาวน์โหลดไฟล์ `complete-video-factory.json`
2. เปิด n8n ที่ http://localhost:5678
3. คลิก "Import from JSON"
4. เลือกไฟล์ที่ดาวน์โหลดมา
5. คลิก "Import"

### ขั้นตอนที่ 2: ตั้งค่า Credentials
ตั้งค่า credentials ต่อไปนี้:
- **OpenAI API** - สำหรับสร้าง script
- **Google Sheets OAuth2** - สำหรับอ่าน/เขียนข้อมูล
- **Together AI** - สำหรับสร้างภาพด้วย FLUX
- **YouTube OAuth2** - สำหรับอัปโหลดวิดีโอ (optional)

### ขั้นตอนที่ 3: ปรับแต่ง Configuration
แก้ไข node "Configure me" ตามต้องการ:

```javascript
{
  "vdo_factory_api_url": "http://vdo-factory:8000",
  "subreddit": "selfimprovement",
  "content_type": "motivational speech",
  "art_style": "vibrant, emotionally uplifting illustration style...",
  "tts_engine": "kokoro",
  "background_music_id": "",
  "background_music_volume": 0.2,
  "update_reddit_stories": false,
  "kokoro_voice": "af_heart",
  "kokoro_speed": 1
}
```

## 🔧 โครงสร้าง Workflow

### 1. 🏁 Initialization Stage
- **Manual Trigger** - เริ่มต้น workflow
- **Configure me** - ตั้งค่าพารามิเตอร์ทั้งหมด
- **Health Check** - ทดสอบการเชื่อมต่อ Video Factory

### 2. 📊 Data Source Stage  
- **Get stories from Reddit** - ดึงข้อมูลจาก subreddit
- **Save to Google Sheets** - บันทึกข้อมูลลง Sheets
- **Get story** - เลือกเรื่องที่จะสร้างวิดีโอ

### 3. 📝 Content Creation Stage
- **Create motivational speech** - สร้าง script ด้วย OpenAI
- **Cleanup text** - ทำความสะอาดข้อความ
- **Create the script** - แบ่งเนื้อหาเป็น scenes พร้อม image prompts

### 4. 🎨 Media Generation Stage
#### Image Generation Loop:
- **Loop Over Items** - วนลูปแต่ละ scene
- **Generate AI image** - สร้างภาพด้วย FLUX
- **Upload image to server** - อัปโหลดภาพไปเซิร์ฟเวอร์

#### TTS Generation:
- **TTS switch** - เลือกระหว่าง Kokoro หรือ Chatterbox
- **Start generating TTS** - สร้างเสียงพูด
- **Get status of TTS** - ตรวจสอบสถานะ

#### Video Generation:
- **Start generating captioned TTS video** - สร้างวิดีโอพร้อม caption
- **Get video generation status** - ตรวจสอบสถานะการสร้างวิดีโอ

### 5. 🎬 Post-production Stage
- **Combine loop items** - รวมข้อมูลจากทุก scenes
- **Start merging videos** - รวมวิดีโอทั้งหมดเป็นเรื่องเดียว
- **Get status of video merge** - ตรวจสอบสถานะการรวมวิดีโอ

### 6. 📤 Distribution Stage
- **Setup final video download URL** - เตรียม URL สำหรับดาวน์โหลด
- **Download the video** - ดาวน์โหลดวิดีโอสำเร็จรูป
- **Start YouTube upload** - เริ่มอัปโหลดไปยัง YouTube (optional)
- **Upload video to YouTube** - อัปโหลดจริง

### 7. 🗑️ Cleanup Stage
- **Set file ids to delete** - เตรียมรายการไฟล์ที่จะลบ
- **Delete tmp files** - ลบไฟล์ temporary

## ⚙️ การตั้งค่าสำคัญ

### Art Style Configuration
```javascript
"art_style": "Use the following prompt as a template for the image generation prompt, use the content to create a unique image for the scene.\n\nCreate an artistic image in a vibrant, emotionally uplifting illustration style, with warm sunlight and a soft pastel or golden color palette..."
```

### TTS Settings
```javascript
{
  "tts_engine": "kokoro",           // หรือ "chatterbox"
  "kokoro_voice": "af_heart",       // เสียงที่ใช้
  "kokoro_speed": 1,                // ความเร็วการพูด
  "chatterbox_exaggeration": 0.5,   // ความเน้นเสียง
  "chatterbox_cfg_weight": 0.5,     // น้ำหนักการควบคุม
  "chatterbox_temperature": 0.8     // ความหลากหลายเสียง
}
```

### Content Settings
```javascript
{
  "subreddit": "selfimprovement",    // subreddit ที่ดึงข้อมูล
  "content_type": "motivational speech", // ประเภทเนื้อหา
  "update_reddit_stories": false    // อัปเดตข้อมูลจาก Reddit หรือไม่
}
```

## 🔄 Data Flow

```
Manual Trigger → Configure → Health Check
    ↓
Reddit API → Google Sheets → Get Story
    ↓
OpenAI → Create Script → Split to Scenes
    ↓
[Loop] → Generate Image → Upload Image → Generate TTS → Create Video
    ↓
Merge Videos → Download → YouTube Upload → Cleanup
```

## 🎛️ การควบคุม Workflow

### Wait Nodes
- **Wait for error/rate limit** - รอเมื่อเกิด error หรือ rate limit
- **Wait until TTS generated** - รอการสร้างเสียงเสร็จ
- **Wait until video generated** - รอการสร้างวิดีโอเสร็จ
- **Wait until videos merged** - รอการรวมวิดีโอเสร็จ

### Switch Nodes
- **TTS status switch** - ตรวจสอบสถานะ TTS (ready/processing/failed)
- **Video generation switch** - ตรวจสอบสถานะวิดีโอ
- **Video merge status switch** - ตรวจสอบสถานะการรวม
- **TTS switch** - เลือกระหว่าง TTS engines

## 🧪 การทดสอบ

### ทดสอบขั้นพื้นฐาน
1. รัน workflow ด้วย `update_reddit_stories: false`
2. ตรวจสอบว่ามีข้อมูลใน Google Sheets
3. รันเฉพาะส่วน content creation ก่อน

### ทดสอบ Media Generation
1. ทดสอบสร้างภาพ 1 ภาพก่อน
2. ทดสอบ TTS ด้วยข้อความสั้น ๆ
3. ทดสอบสร้างวิดีโอ 1 scene

### ทดสอบ Full Pipeline
1. ใช้ข้อมูลที่มีอยู่แล้วใน Sheets
2. รัน workflow เต็มรูปแบบ
3. ตรวจสอบผลลัพธ์ในแต่ละขั้นตอน

## ⚠️ ข้อควรระวัง

### Rate Limits และ Costs
- **OpenAI**: มี rate limit และคิดค่าใช้จ่ายตาม usage
- **Together AI**: มี rate limit สำหรับ FLUX model
- **Video Factory**: อาจใช้เวลานานในการประมวลผล

### Error Handling
- Workflow มี retry logic สำหรับ API calls
- ใช้ Wait nodes เพื่อรอการประมวลผล
- ตรวจสอบสถานะก่อนไปขั้นตอนถัดไป

### File Management
- ไฟล์ temporary จะถูกลบอัตโนมัติ
- สามารถปรับแต่งการลบไฟล์ใน cleanup stage

## 🔍 การแก้ไขปัญหา

### ปัญหาการเชื่อมต่อ
```
Error: Server isn't ready
```
**แก้ไข**: ตรวจสอบว่า Video Factory API ทำงานที่ port 8000

### ปัญหา Image Generation
```
Error: Rate limit exceeded
```
**แก้ไข**: Wait node จะรอและลองใหม่อัตโนมัติ

### ปัญหา TTS Generation
```
Status: failed
```
**แก้ไข**: ตรวจสอบข้อความที่ส่งไป และ TTS engine settings

### ปัญหา Video Merge
```
Error: Video merge failed
```
**แก้ไข**: ตรวจสอบว่าวิดีโอทุกอันสร้างเสร็จแล้ว

## 📊 Performance Tips

### การปรับแต่งประสิทธิภาพ
1. **ลดจำนวน scenes** - สำหรับทดสอบใช้ 2-3 scenes
2. **ปรับ batch size** - ใน Loop Over Items
3. **เพิ่ม wait time** - ถ้าการประมวลผลช้า
4. **ปิด YouTube upload** - เมื่อทดสอบ

### การ Monitor
- ดู execution logs ใน n8n
- ตรวจสอบ API usage ใน platforms ต่าง ๆ
- Monitor disk space บนเซิร์ฟเวอร์

## ⏱️ เวลาที่ใช้โดยประมาณ

### ขั้นตอนการเรียนรู้
- Import และ setup: 30 นาที
- ทำความเข้าใจ workflow: 1 ชั่วโมง
- ทดสอบทีละส่วน: 1 ชั่วโมง
- รัน full workflow: 30 นาที

### ขั้นตอนการทำงาน
- Content creation: 2-5 นาที
- Image generation: 1-2 นาที/ภาพ
- TTS generation: 30 วินาที - 2 นาที
- Video creation: 1-3 นาที/scene
- Video merging: 2-5 นาที

**รวมทั้งหมด (เรียนรู้): 3 ชั่วโมง**
**รวมทั้งหมด (การใช้งาน): 10-20 นาที สำหรับวิดีโอ 5 scenes**

## ✅ เป้าหมายการเรียนรู้
เมื่อจบ Section นี้ นักเรียนจะสามารถ:
- Import และใช้งาน complex workflow ได้
- เข้าใจ data flow ของการสร้างวิดีโออัตโนมัติ
- ปรับแต่ง parameters สำหรับผลลัพธ์ที่ต้องการ
- แก้ไขปัญหาที่เกิดขึ้นในแต่ละขั้นตอน
- สร้างวิดีโอจาก Reddit content แบบอัตโนมัติ
- เชื่อมต่อกับ external services หลาย ๆ ตัว

---
[← กลับไป Section 2](../README.md) | [กลับไปหน้าหลัก](../../README.md)