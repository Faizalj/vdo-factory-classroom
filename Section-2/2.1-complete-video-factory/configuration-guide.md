# ⚙️ Configuration Guide - Complete Video Factory

## 🎯 ภาพรวม
คู่มือนี้จะแนะนำการตั้งค่าและปรับแต่ง workflow ให้เหมาะกับความต้องการของคุณ

## 🔧 หัวใจของการตั้งค่า: "Configure me" Node

### พารามิเตอร์หลัก
```javascript
{
  // Video Factory API Configuration
  "vdo_factory_api_url": "http://vdo-factory:8000",
  
  // Reddit Content Source
  "subreddit": "selfimprovement",
  "update_reddit_stories": false,
  
  // Content Generation
  "content_type": "motivational speech",
  "art_style": "vibrant, emotionally uplifting illustration style...",
  
  // Text-to-Speech Settings
  "tts_engine": "kokoro",
  "kokoro_voice": "af_heart",
  "kokoro_speed": 1,
  
  // Chatterbox TTS (Alternative)
  "chatterbox_exaggeration": 0.5,
  "chatterbox_cfg_weight": 0.5,
  "chatterbox_temperature": 0.8,
  "chatterbox_clone_voice_id": "",
  
  // Audio Settings
  "background_music_id": "",
  "background_music_volume": 0.2
}
```

## 🔗 API Configuration

### Video Factory API URL
```javascript
"vdo_factory_api_url": "http://vdo-factory:8000"
```
**สำคัญ**: ใช้ชื่อ container `vdo-factory` เพื่อให้ n8n เชื่อมต่อได้ในเครือข่าย Docker

### Health Check Endpoint
Workflow จะทดสอบการเชื่อมต่อที่:
```
GET http://vdo-factory:8000/health
```

## 📊 Reddit Data Source

### Subreddit Selection
```javascript
"subreddit": "selfimprovement"  // เปลี่ยนตามที่ต้องการ
```

**📋 ดู Subreddit Ideas**: [subreddit-content-ideas.md](./subreddit-content-ideas.md)

**ตัวอย่าง subreddits ยอดนิยม**:
- `selfimprovement` - การพัฒนาตนเอง
- `getmotivated` - แรงบันดาลใจ
- `todayilearned` - ความรู้ใหม่
- `explainlikeimfive` - คำอธิบายเรื่องซับซ้อน
- `showerthoughts` - ความคิดน่าสนใจ
- `tifu` - เรื่องเล่าตลก ๆ
- `lifeprotips` - เทคนิคใช้ชีวิต

### Update Behavior
```javascript
"update_reddit_stories": false  // true = ดึงข้อมูลใหม่จาก Reddit
```
- `false`: ใช้ข้อมูลที่มีใน Google Sheets
- `true`: ดึงข้อมูลใหม่ 100 posts จาก Reddit

## 🎨 Content Generation

### Content Type
```javascript
"content_type": "motivational speech"  // เปลี่ยนตามประเภทเนื้อหา
```

**📋 ดู Content Type Ideas**: [subreddit-content-ideas.md](./subreddit-content-ideas.md)

**ตัวอย่าง content types ยอดนิยม**:
- `motivational speech` - คำพูดสร้างแรงบันดาลใจ (default)
- `engaging story narration` - เล่าเรื่องให้น่าฟัง
- `educational explanation` - การอธิบายเชิงการศึกษา
- `fascinating quick facts` - ข้อเท็จจริงสั้น ๆ น่าสนใจ
- `practical life advice` - คำแนะนำใช้ชีวิต
- `mind-bending perspective` - มุมมองกระตุ้นความคิด

### Art Style Configuration
```javascript
"art_style": "ใส่ Art Style prompt ที่เลือกจาก art-style-templates.md"
```

**📋 ดู Art Style Templates**: [art-style-templates.md](./art-style-templates.md)

**ตัวอย่าง Art Styles ที่มีให้เลือก**:
1. 🌸 **Soft Watercolor Inspiration** - อบอุ่น สงบ เหมือนหนังสือภาพ
2. 🌈 **Vibrant Digital Pop Art** - สดใส แนวเยียวยาหัวใจ  
3. ✨ **Magical Realism** - ฝัน ๆ เหมือนความหวังในโลกแฟนตาซี
4. 🏠 **Cozy Slice of Life** - ธรรมดาที่แสนอบอุ่น
5. 🎯 **Hopeful Minimalist** - น้อยแต่มาก อารมณ์ลึก

**วิธีใช้งาน**:
1. เปิดไฟล์ `art-style-templates.md`
2. เลือก style ที่ต้องการ
3. Copy ข้อความใน code block ทั้งหมด
4. Paste ลงในช่อง `art_style` ของ "Configure me" node

## 🎤 Text-to-Speech Configuration

### Engine Selection
```javascript
"tts_engine": "kokoro"  // หรือ "chatterbox"
```

### Kokoro TTS Settings
```javascript
{
  "kokoro_voice": "af_heart",     // เสียงที่ใช้
  "kokoro_speed": 1               // ความเร็วการพูด (0.5-2.0)
}
```

**Available Kokoro Voices**:
- `af_heart` - เสียงหญิงอบอุ่น ✅ **ยืนยันว่าใช้ได้**
- `af_neutral` - เสียงหญิงเป็นกลาง ⚠️ **ต้องทดสอบ**
- `af_sarah` - เสียงหญิงชัดเจน ⚠️ **ต้องทดสอบ**
- `am_casual` - เสียงชายสบาย ๆ ⚠️ **ต้องทดสอบ**
- `am_formal` - เสียงชายเป็นทางการ ⚠️ **ต้องทดสอบ**  
- `am_david` - เสียงชายมีพลัง ⚠️ **ต้องทดสอบ**

**หมายเหตุ**: ปัจจุบันมีการยืนยันว่า `af_heart` ใช้ได้แน่นอน เสียงอื่น ๆ อาจต้องการการตั้งค่าเพิ่มเติมใน backend

### Chatterbox TTS Settings
```javascript
{
  "chatterbox_exaggeration": 0.5,    // ความเน้นเสียง (0.0-1.0)
  "chatterbox_cfg_weight": 0.5,      // การควบคุมโทนเสียง (0.0-1.0)
  "chatterbox_temperature": 0.8,     // ความหลากหลายเสียง (0.0-1.0)
  "chatterbox_clone_voice_id": ""     // ID ของ voice sample
}
```

**Chatterbox Voice Cloning**:
1. อัปโหลด audio sample ไปยัง Video Factory
2. ใส่ `file_id` ที่ได้รับใน `chatterbox_clone_voice_id`
3. เปลี่ยน `tts_engine` เป็น `"chatterbox"`

## 🎵 Audio Settings

### Background Music
```javascript
{
  "background_music_id": "",        // ID ของไฟล์เพลง
  "background_music_volume": 0.2    // ระดับเสียงเพลง (0.0-1.0)
}
```

**การเพิ่มเพลงพื้นหลัง**:
1. อัปโหลดไฟล์เพลงไปยัง Video Factory API
2. ใส่ `file_id` ที่ได้รับใน `background_music_id`
3. ปรับ volume ให้เหมาะสม (แนะนำ 0.1-0.3)

## 🎛️ Advanced Configuration

### Google Sheets Setup
workflow ต้องการ Google Sheets 2 อัน:

#### Sheet 1: Reddit Stories Storage
```
Document ID: ใส่ ID ของ Google Sheet ที่สร้าง
Columns: id, title, content, video_id
```

#### Sheet 2: Video Processing Tracking  
```
Document ID: ใส่ ID ของ Google Sheet ที่สร้าง
Columns: id, title, content, status, video_id
```

### Credentials Configuration
ตั้งค่า credentials เหล่านี้ใน n8n:

#### OpenAI API
```
Credential Name: OpenAi account
Type: OpenAI
API Key: sk-xxxxxxxxxxxxxxxx
```

#### Google Sheets OAuth2
```
Credential Name: Google Sheets account  
Type: Google Sheets OAuth2 API
```

#### Together AI (FLUX)
```
Credential Name: Together AI test
Type: HTTP Bearer Auth
Token: xxxxxxxxxxxxxxxx
```

#### YouTube OAuth2 (Optional)
```
Credential Name: YouTube account
Type: YouTube OAuth2 API
```

## 🔧 Performance Tuning

### Image Generation
```javascript
// ใน "Generate AI image" node
{
  "width": 720,     // ลดเป็น 512 เพื่อความเร็ว
  "height": 1280,   // ลดเป็น 512 เพื่อความเร็ว
  "model": "black-forest-labs/FLUX.1-schnell-Free"
}
```

### Wait Times
```javascript
// ปรับ wait times ตามความต้องการ
"Wait until TTS generated": 60000,      // 1 นาที
"Wait until video generated": 60000,    // 1 นาที  
"Wait until videos merged": 60000,      // 1 นาที
"Wait for rate limit": 10000            // 10 วินาที
```

### Batch Processing
```javascript
// ใน "Loop Over Items" node
{
  "batchSize": 1,     // เพิ่มเป็น 2-3 เพื่อความเร็ว
  "options": {}
}
```

## 🎯 Use Case Configurations

### Configuration 1: Educational Content
```javascript
{
  "vdo_factory_api_url": "http://vdo-factory:8000",
  "subreddit": "explainlikeimfive",
  "content_type": "educational explanation", 
  "kokoro_voice": "af_neutral",
  "kokoro_speed": 0.9,
  "art_style": "clean, educational illustration style...",
  "background_music_volume": 0.1
}
```

### Configuration 2: News Content
```javascript
{
  "vdo_factory_api_url": "http://vdo-factory:8000",
  "subreddit": "worldnews",
  "content_type": "news summary",
  "kokoro_voice": "am_formal", 
  "kokoro_speed": 1.0,
  "art_style": "modern, journalistic style...",
  "background_music_id": "news_intro_music_id"
}
```

### Configuration 3: Entertainment Content
```javascript
{
  "vdo_factory_api_url": "http://vdo-factory:8000",
  "subreddit": "tifu",
  "content_type": "entertaining story",
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.7,
  "art_style": "colorful, fun cartoon style...",
  "background_music_volume": 0.3
}
```

## 🔍 Troubleshooting Configuration

### Common Configuration Issues

#### Issue: Health Check Fails
```
Error: Server isn't ready
Solution: ตรวจสอบ vdo_factory_api_url และ Docker containers
```

#### Issue: TTS Generation Fails
```
Error: TTS failed
Solutions:
- ตรวจสอบ tts_engine setting
- ลองเปลี่ยน voice
- ลดความยาวข้อความ
```

#### Issue: Image Generation Rate Limited
```
Error: Rate limit exceeded  
Solutions:
- เพิ่ม wait time
- ลดจำนวน scenes
- ใช้ Together AI account ที่มี quota มากขึ้น
```

#### Issue: Google Sheets Permission Denied
```
Error: Permission denied
Solutions:
- Re-authenticate Google Sheets credential
- ตรวจสอบ document ID
- ตรวจสอบ sharing permissions ของ sheets
```

## 💡 Best Practices

### Content Quality
- ใช้ข้อความที่เหมาะสมกับ TTS (หลีกเลี่ยงอักขระพิเศษ)
- ปรับ content_type ให้เข้ากับข้อมูลจาก subreddit
- ทดสอบ art_style กับเนื้อหาจริงก่อน

### Performance
- เริ่มต้นด้วย 2-3 scenes สำหรับทดสอบ
- ใช้ image size เล็กสำหรับ development
- Monitor API usage และ costs

### Reliability  
- ตั้งค่า wait times ให้เพียงพอ
- ใช้ retry logic สำหรับ API calls
- สำรอง configuration settings

การตั้งค่าที่ดีจะทำให้ workflow ทำงานได้อย่างมั่นคงและให้ผลลัพธ์ที่ต้องการ!