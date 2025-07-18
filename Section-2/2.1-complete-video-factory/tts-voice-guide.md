# 🎤 TTS Voice Guide - คู่มือเสียงพูดสำหรับ Video Factory

## 🎯 ภาพรวม
คู่มือครอบคลุมการใช้งาน TTS (Text-to-Speech) ทั้งสองระบบ: Kokoro และ Chatterbox พร้อมตัวอย่างเสียงและการตั้งค่า

## 🔧 TTS Engine Selection

### ใน workflow สามารถเลือกได้ 2 engine:
```javascript
"tts_engine": "kokoro"     // หรือ "chatterbox"
```

---

## 🌸 Kokoro TTS Engine

### 🎯 จุดเด่นของ Kokoro
- ⚡ **ความเร็ว**: สร้างเสียงได้เร็ว (30วินาที-1นาที)
- 🎵 **คุณภาพ**: เสียงธรรมชาติ เข้าใจง่าย
- 💰 **ประหยัด**: ใช้ resource น้อยกว่า
- 🌐 **ภาษา**: รองรับหลายภาษาดี

### 🎵 Kokoro Voice Options

#### 📋 รายชื่อเสียงทั้งหมดที่ใช้ได้
**หญิง (Female)**: `af_heart`, `af_alloy`, `af_aoede`, `af_bella`, `af_jessica`, `af_kore`, `af_nicole`, `af_nova`, `af_river`, `af_sarah`, `af_sky`

**ชาย (Male)**: `am_adam`, `am_echo`, `am_eric`, `am_fenrir`, `am_liam`, `am_michael`, `am_onyx`, `am_puck`, `am_santa`

#### หญิง (Female Voices)
```javascript
"kokoro_voice": "af_heart"        // เสียงหญิงอบอุ่น, นุ่มนวล
```
**เหมาะสำหรับ**: เนื้อหา motivational, การเยียวยาใจ, เรื่องเล่าอบอุ่น

```javascript
"kokoro_voice": "af_alloy"        // เสียงหญิงเป็นกลาง, เป็นมืออาชีพ
```
**เหมาะสำหรับ**: educational content, ข่าวสาร, คำแนะนำ

```javascript
"kokoro_voice": "af_aoede"        // เสียงหญิงนุ่มนวล, เพราะพริ้ง
```
**เหมาะสำหรับ**: เรื่องเล่า, podcast, เนื้อหาเบา ๆ

```javascript
"kokoro_voice": "af_bella"        // เสียงหญิงหวาน, น่ารัก
```
**เหมาะสำหรับ**: เนื้อหาสำหรับวัยรุ่น, lifestyle, beauty content

```javascript
"kokoro_voice": "af_jessica"      // เสียงหญิงชัดเจน, เป็นมิตร
```
**เหมาะสำหรับ**: การนำเสนอ, tutorial, เนื้อหาธุรกิจ

```javascript
"kokoro_voice": "af_kore"         // เสียงหญิงมั่นใจ, แข็งแกร่ง
```
**เหมาะสำหรับ**: leadership content, การตลาด, พรีเซนเทชั่น

```javascript
"kokoro_voice": "af_nicole"       // เสียงหญิงสุภาพ, เรียบร้อย
```
**เหมาะสำหรับ**: formal content, ข่าวสาร, การศึกษา

```javascript
"kokoro_voice": "af_nova"         // เสียงหญิงทันสมัย, มีพลัง
```
**เหมาะสำหรับ**: tech content, innovation, เทรนด์ใหม่

```javascript
"kokoro_voice": "af_river"        // เสียงหญิงเย็น, สงบ
```
**เหมาะสำหรับ**: meditation, relaxation, mindfulness content

```javascript
"kokoro_voice": "af_sarah"        // เสียงหญิงอบอุ่น, เป็นกันเอง
```
**เหมาะสำหรับ**: storytelling, personal sharing, family content

```javascript
"kokoro_voice": "af_sky"          // เสียงหญิงใส, สดใส
```
**เหมาะสำหรับ**: วัยรุ่น, เนื้อหาสนุก ๆ, entertainment

#### ชาย (Male Voices)
```javascript
"kokoro_voice": "am_adam"         // เสียงชายเข้ม, มั่นคง
```
**เหมาะสำหรับ**: news, financial content, serious topics

```javascript
"kokoro_voice": "am_echo"         // เสียงชายลึก, น่าเชื่อถือ
```
**เหมาะสำหรับ**: documentary, educational, authority content

```javascript
"kokoro_voice": "am_eric"         // เสียงชายเป็นกันเอง, สบาย ๆ
```
**เหมาะสำหรับ**: lifestyle content, เรื่องเล่าตลก, ความบันเทิง

```javascript
"kokoro_voice": "am_fenrir"       // เสียงชายแข็งแกร่ง, มีพลัง
```
**เหมาะสำหรับ**: motivational speech, sports, action content

```javascript
"kokoro_voice": "am_liam"         // เสียงชายหนุ่ม, ทันสมัย
```
**เหมาะสำหรับ**: tech, gaming, millennial content

```javascript
"kokoro_voice": "am_michael"      // เสียงชายเป็นมืออาชีพ, เป็นทางการ
```
**เหมาะสำหรับ**: business, corporate, professional content

```javascript
"kokoro_voice": "am_onyx"         // เสียงชายเท่, มีสไตล์
```
**เหมาะสำหรับ**: fashion, design, creative content

```javascript
"kokoro_voice": "am_puck"         // เสียงชายซุกซน, สนุกสนาน
```
**เหมาะสำหรับ**: comedy, entertainment, light content

```javascript
"kokoro_voice": "am_santa"        // เสียงชายอบอุ่น, ใจดี
```
**เหมาะสำหรับ**: family content, holiday content, wholesome topics

### ⚙️ Kokoro Parameters

#### ความเร็วการพูด
```javascript
"kokoro_speed": 1.0    // ความเร็วปกติ
"kokoro_speed": 0.8    // ช้าลง (เหมาะการศึกษา)
"kokoro_speed": 1.2    // เร็วขึ้น (เหมาะคลิปสั้น)
"kokoro_speed": 0.6    // ช้ามาก (เหมาะผู้สูงอายุ)
"kokoro_speed": 1.5    // เร็วมาก (เหมาะข้อมูลเร็ว ๆ)
```

### 🎯 Kokoro Configuration Examples

#### Motivational Content
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_heart",
  "kokoro_speed": 1.0
}
```

#### Educational Content
```javascript
{
  "tts_engine": "kokoro", 
  "kokoro_voice": "af_alloy",
  "kokoro_speed": 0.9
}
```

#### Business Content
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "am_michael", 
  "kokoro_speed": 1.0
}
```

#### Fun/Casual Content
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "am_eric",
  "kokoro_speed": 1.1
}
```

---

## 🎭 Chatterbox TTS Engine

### 🎯 จุดเด่นของ Chatterbox
- 🎨 **Customization**: ปรับแต่งได้มาก
- 🎭 **Expression**: เสียงมีอารมณ์และนิสัย
- 🔊 **Voice Cloning**: ทำ voice clone ได้
- 🎵 **Variety**: ความหลากหลายของเสียง

### ⚙️ Chatterbox Parameters

#### Exaggeration (ความเน้นเสียง)
```javascript
"chatterbox_exaggeration": 0.0    // เสียงเรียบ, เป็นกลาง
"chatterbox_exaggeration": 0.3    // เน้นเล็กน้อย
"chatterbox_exaggeration": 0.5    // เน้นปานกลาง (แนะนำ)
"chatterbox_exaggeration": 0.7    // เน้นชัด, มีอารมณ์
"chatterbox_exaggeration": 1.0    // เน้นมาก, เหมาะคอนเทนต์สนุก ๆ
```

#### CFG Weight (การควบคุมโทนเสียง)
```javascript
"chatterbox_cfg_weight": 0.0      // เสียงธรรมชาติ, ปล่อยตาม
"chatterbox_cfg_weight": 0.3      // ควบคุมเล็กน้อย
"chatterbox_cfg_weight": 0.5      // ควบคุมปานกลาง (แนะนำ)
"chatterbox_cfg_weight": 0.7      // ควบคุมชัด
"chatterbox_cfg_weight": 1.0      // ควบคุมสูงสุด, เสียงคงที่
```

#### Temperature (ความหลากหลายเสียง)
```javascript
"chatterbox_temperature": 0.2     // เสียงคงที่, เหมือนเดิม
"chatterbox_temperature": 0.5     // ความหลากหลายเล็กน้อย
"chatterbox_temperature": 0.8     // ความหลากหลายปานกลาง (แนะนำ)
"chatterbox_temperature": 1.0     // หลากหลายมาก
"chatterbox_temperature": 1.2     // หลากหลายมากที่สุด (อาจจะแปลง ๆ)
```

### 🎯 Chatterbox Configuration Examples

#### Professional Narration
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.3,
  "chatterbox_cfg_weight": 0.6,
  "chatterbox_temperature": 0.5
}
```

#### Storytelling
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.7,
  "chatterbox_cfg_weight": 0.4,
  "chatterbox_temperature": 0.8
}
```

#### News/Information
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.2,
  "chatterbox_cfg_weight": 0.7,
  "chatterbox_temperature": 0.3
}
```

#### Entertainment/Fun
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.8,
  "chatterbox_cfg_weight": 0.3,
  "chatterbox_temperature": 1.0
}
```

---

## 🎤 Voice Cloning ด้วย Chatterbox

### 📋 ขั้นตอนการทำ Voice Clone

#### 1. เตรียม Audio Sample
- **ความยาว**: 30วินาที - 2นาที
- **คุณภาพ**: 44.1kHz, 16-bit หรือสูงกว่า
- **เนื้อหา**: พูดชัดเจน, หลากหลายโทน
- **สิ่งแวดล้อม**: เงียบ, ไม่มีเสียงรบกวน

#### 2. Upload ไปยัง Video Factory
```bash
curl -X POST "http://vdo-factory:8000/api/v1/media/storage" \
  -F "file=@voice_sample.wav" \
  -F "media_type=audio"
```

#### 3. ใช้ในการตั้งค่า
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_clone_voice_id": "FILE_ID_ที่ได้จากการอัปโหลด",
  "chatterbox_exaggeration": 0.5,
  "chatterbox_cfg_weight": 0.5,
  "chatterbox_temperature": 0.6
}
```

### 💡 เทคนิคการทำ Voice Clone ที่ดี

#### เนื้อหา Sample ที่ดี
```
"สวัสดีครับ ผมชื่อ [ชื่อ] วันนี้เราจะมาเรียนรู้เรื่องใหม่ ๆ กันนะครับ 
การที่เราได้ศึกษาเรื่องราวต่าง ๆ จะช่วยให้เราเติบโตและพัฒนาตัวเองได้ดีขึ้น 
ผมหวังว่าเนื้อหาวันนี้จะเป็นประโยชน์กับทุกคนนะครับ ขอบคุณครับ"
```

#### คำแนะนำการบันทึก
- 🎤 **ไมโครโฟนใกล้ปาก** 15-20 ซม.
- 🔇 **สิ่งแวดล้อมเงียบ** ไม่มีเสียงแอร์, รถ, คน
- 📢 **พูดชัดเจน** ไม่เร็วไม่ช้าเกินไป
- 🎭 **หลายอารมณ์** มีทั้งขึ้น-ลง, เน้น-ไม่เน้น

---

## 🆚 Kokoro vs Chatterbox Comparison

| Feature | Kokoro | Chatterbox |
|---------|--------|------------|
| **ความเร็ว** | ⚡⚡⚡ เร็วมาก | ⚡⚡ ปานกลาง |
| **คุณภาพ** | ⭐⭐⭐ ดี | ⭐⭐⭐⭐ ดีมาก |
| **Customization** | ⚙️ พื้นฐาน | ⚙️⚙️⚙️ สูง |
| **Voice Cloning** | ❌ ไม่ได้ | ✅ ได้ |
| **ความหลากหลาย** | 👥 20 เสียง | 👥 ไม่จำกัด |
| **Resource Usage** | 💚 น้อย | 💛 ปานกลาง |
| **เหมาะสำหรับ** | Production เร็ว | Quality สูง |

---

## 🎯 การเลือกใช้ TTS ตามประเภทเนื้อหา

### 📚 Educational Content
**แนะนำ**: Kokoro `af_alloy` หรือ `am_michael`
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_alloy",
  "kokoro_speed": 0.9
}
```

### 💪 Motivational Content  
**แนะนำ**: Kokoro `af_heart` หรือ Chatterbox emotional
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_heart", 
  "kokoro_speed": 1.0
}
```

### 📖 Storytelling
**แนะนำ**: Chatterbox ปรับแต่งสูง
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.7,
  "chatterbox_cfg_weight": 0.4,
  "chatterbox_temperature": 0.9
}
```

### 💰 Business/Finance
**แนะนำ**: Kokoro `am_michael` หรือ `am_adam`
```javascript
{
  "tts_engine": "kokoro",
  "kokoro_voice": "am_michael",
  "kokoro_speed": 1.0
}
```

### 🎮 Entertainment/Fun
**แนะนำ**: Kokoro `am_eric` หรือ `am_puck` หรือ Chatterbox สนุก
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.8,
  "chatterbox_cfg_weight": 0.3,
  "chatterbox_temperature": 1.0
}
```

---

## 🧪 การทดสอบและปรับแต่ง

### ขั้นตอนการหาเสียงที่เหมาะสม

#### 1. เริ่มด้วย Kokoro
```javascript
// ทดสอบพื้นฐานก่อน
{
  "tts_engine": "kokoro",
  "kokoro_voice": "af_alloy",
  "kokoro_speed": 1.0
}
```

#### 2. ลองเสียงต่าง ๆ
```javascript
// ทดสอบเสียงที่เหมาะกับเนื้อหา
"kokoro_voice": "af_heart"    // อบอุ่น
"kokoro_voice": "am_eric"     // สบาย ๆ
"kokoro_voice": "am_michael"  // เป็นทางการ
"kokoro_voice": "af_nova"     // ทันสมัย
"kokoro_voice": "am_fenrir"   // มีพลัง
```

#### 3. ปรับความเร็ว
```javascript
"kokoro_speed": 0.8    // สำหรับเนื้อหายาก
"kokoro_speed": 1.2    // สำหรับคลิปสั้น
```

#### 4. ถ้าต้องการมากกว่า → Chatterbox
```javascript
{
  "tts_engine": "chatterbox",
  "chatterbox_exaggeration": 0.5,
  "chatterbox_cfg_weight": 0.5, 
  "chatterbox_temperature": 0.8
}
```

### 🔊 การทดสอบคุณภาพเสียง

#### เกณฑ์การประเมิน
1. **ความชัดเจน** - ได้ยินคำศัพท์ชัดเจน
2. **ความเป็นธรรมชาติ** - ฟังแล้วไม่แปลก
3. **ความเหมาะสม** - เข้ากับเนื้อหา
4. **จังหวะ** - ไม่เร็วไม่ช้าเกินไป
5. **อารมณ์** - สื่ออารมณ์ได้ถูกต้อง

#### วิธีทดสอบ
1. สร้างคลิปทดสอบสั้น ๆ (30 วินาที)
2. ฟังใน speaker และ หูฟัง
3. ให้คนอื่นฟังและให้ feedback
4. เทียบกับคู่แข่งหรือเสียงมาตรฐาน

---

## ⚠️ ข้อควรระวัง

### การใช้ Kokoro
- เสียงบางตัวอาจไม่เหมาะกับเนื้อหาบางประเภท
- ความเร็วเกิน 1.5 อาจทำให้ไม่ชัดเจน
- เสียงชายและหญิงให้ feeling แตกต่างกัน

### การใช้ Chatterbox  
- ใช้เวลาสร้างนานกว่า Kokoro
- Parameter ที่สูงเกินไปอาจทำให้เสียงแปลง ๆ
- Voice cloning ต้องใช้ sample คุณภาพดี

### ข้อกฎหมาย
- ระวังเรื่อง voice cloning ของคนดัง
- ใช้เฉพาะเสียงตัวเองหรือได้รับอนุญาต
- ระบุแหล่งที่มาถ้าใช้เสียงคนอื่น

---

## 💡 Pro Tips

### เพิ่มความน่าสนใจ
1. **ใช้เสียงที่ต่างกัน** สำหรับ series ต่าง ๆ
2. **ปรับความเร็ว** ตามประเภทเนื้อหา  
3. **เลือกเสียง** ให้เข้ากับ target audience
4. **ทดสอบ A/B** ระหว่างเสียงต่าง ๆ

### การประหยัด Cost & Time
1. **ใช้ Kokoro** สำหรับ bulk production
2. **ใช้ Chatterbox** สำหรับคอนเทนต์สำคัญ
3. **เก็บ setting** ที่ใช้ได้ดีไว้
4. **Voice clone** เฉพาะเมื่อจำเป็น

การเลือกใช้ TTS ที่เหมาะสมจะทำให้วิดีโอมีคุณภาพและความน่าสนใจมากขึ้น! 🎤✨