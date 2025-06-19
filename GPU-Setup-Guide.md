# 🚀 GPU Setup Guide สำหรับ Video Factory

## 🎯 ภาพรวม
คู่มือการตั้งค่า GPU support สำหรับ Docker containers เพื่อเพิ่มประสิทธิภาพในการประมวลผล AI

## 📋 ข้อกำหนดเบื้องต้น

### 1. NVIDIA GPU ที่รองรับ
- NVIDIA GPU รุ่น GTX 1060 ขึ้นไป หรือ RTX series
- CUDA Compute Capability 6.0 ขึ้นไป

### 2. NVIDIA Driver
```bash
# ตรวจสอบ NVIDIA driver
nvidia-smi
```
ต้องมี driver version 470+ สำหรับ CUDA 11.4+

### 3. NVIDIA Container Toolkit
```bash
# Ubuntu/Debian
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
  sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# Restart Docker
sudo systemctl restart docker
```

## 🔧 Docker Compose Configuration

### Updated docker-compose.yml
```yaml
version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    container_name: n8n
    restart: always
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=password
      - TZ=Asia/Bangkok

  vdo-factory:
    image: faizalj/ai-toolstack-vdo-factory:latest
    container_name: vdo-factory
    restart: always
    ports:
      - "8000:8000"
    # GPU Configuration
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=compute,utility
      # ถ้ามี ENV อื่น เช่น API KEY ให้ใส่เพิ่มตรงนี้

volumes:
  n8n_data:
```

## 🚀 การเริ่มใช้งาน

### 1. ทดสอบ GPU Support
```bash
# ทดสอบว่า Docker รู้จัก GPU
docker run --rm --gpus all nvidia/cuda:11.8-base-ubuntu20.04 nvidia-smi
```

### 2. Start Services
```bash
cd Resource/
docker-compose up -d
```

### 3. ตรวจสอบ GPU ใน Container
```bash
# เข้าไปใน vdo-factory container
docker exec -it vdo-factory bash

# ตรวจสอบ GPU
nvidia-smi

# ตรวจสอบ CUDA
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'GPU count: {torch.cuda.device_count()}')"
```

## 📊 การตรวจสอบประสิทธิภาพ

### GPU Monitoring
```bash
# Monitor GPU usage
watch -n 1 nvidia-smi

# หรือใช้ htop กับ GPU
nvtop
```

### ใน Container
```bash
# ตรวจสอบ GPU memory
docker exec vdo-factory nvidia-smi --query-gpu=memory.used,memory.total --format=csv
```

## ⚡ ข้อดีของการใช้ GPU

### ความเร็วที่เพิ่มขึ้น
- **TTS Generation**: เร็วขึ้น 3-5 เท่า
- **Image Generation**: เร็วขึ้น 10-20 เท่า (ถ้ารองรับ)
- **Video Processing**: เร็วขึ้น 2-3 เท่า

### การใช้ Memory
- GPU memory จะถูกใช้แทน RAM
- ลด bottleneck ของระบบ

## 🛠️ Troubleshooting

### ปัญหา: nvidia-smi ไม่ทำงาน
```bash
# ตรวจสอบ driver
lsmod | grep nvidia

# ติดตั้ง driver ใหม่
sudo apt purge nvidia-*
sudo apt install nvidia-driver-515
sudo reboot
```

### ปัญหา: Docker ไม่เห็น GPU
```bash
# ตรวจสอบ nvidia-container-toolkit
dpkg -l | grep nvidia-container

# ติดตั้งใหม่
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

### ปัญหา: Container ไม่ได้ใช้ GPU
```bash
# ตรวจสอบ container runtime
docker info | grep -i runtime

# รัน container ด้วย --gpus all
docker run --rm --gpus all vdo-factory nvidia-smi
```

## 🔄 Alternative Configuration

### สำหรับ GPU หลายตัว
```yaml
deploy:
  resources:
    reservations:
      devices:
        - driver: nvidia
          device_ids: ['0']  # ใช้เฉพาะ GPU 0
          capabilities: [gpu]
```

### สำหรับจำกัด GPU Memory
```yaml
environment:
  - NVIDIA_VISIBLE_DEVICES=0
  - NVIDIA_MIG_CONFIG_DEVICES=all
```

## 📈 Performance Testing

### ทดสอบประสิทธิภาพ TTS
```bash
# ทดสอบกับ GPU
time curl -X POST "http://localhost:8000/api/v1/media/tts/generate" \
  -H "Content-Type: application/json" \
  -d '{"text": "This is a test", "engine": "kokoro", "voice": "af_heart"}'

# เปรียบเทียบกับ CPU only
```

## ⚠️ ข้อควรระวัง

### Resource Usage
- GPU จะใช้ไฟฟ้ามากขึ้น
- ความร้อนเพิ่มขึ้น
- ต้องระบายความร้อนให้ดี

### Compatibility
- ตรวจสอบว่า AI models รองรับ GPU
- บาง models อาจทำงานช้าลงกับ GPU

## 🎯 สรุป

การเพิ่ม GPU support จะช่วยให้:
- สร้างวิดีโอเร็วขึ้นมาก
- รองรับ load ที่มากขึ้น  
- ประหยัดเวลาในการรอ processing

**แนะนำ**: ถ้ามี NVIDIA GPU ให้เปิดใช้งาน เพราะจะทำให้ประสิทธิภาพดีขึ้นอย่างชัดเจน! 🚀