# 🛠️ GPU Troubleshooting Guide - Video Factory

## 🚨 ปัญหาที่พบบ่อยและการแก้ไข

### 1. GPU ไม่ถูกตรวจพบ

#### อาการ:
```
Error: No CUDA-capable device is detected
RuntimeError: CUDA unavailable
```

#### สาเหตุและการแก้ไข:

**🔍 ตรวจสอบ NVIDIA Driver**
```bash
# ตรวจสอบ driver version
nvidia-smi

# ถ้าไม่มี output = driver ไม่ได้ติดตั้ง
sudo apt update
sudo apt install nvidia-driver-515
sudo reboot
```

**🔍 ตรวจสอบ Docker GPU Support**
```bash
# ทดสอบ GPU ใน Docker
docker run --rm --gpus all nvidia/cuda:11.8-base-ubuntu20.04 nvidia-smi

# ถ้า error = NVIDIA Container Toolkit ไม่ได้ติดตั้ง
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

**🔍 ตรวจสอบ Container Configuration**
```bash
# ดู docker-compose.yml มี GPU config หรือไม่
grep -A 10 \"deploy:\" docker-compose.yml

# ถ้าไม่มี = เพิ่ม GPU configuration
```

---

### 2. Out of Memory (OOM) Errors

#### อาการ:
```
CUDA out of memory. Tried to allocate 2.00 GiB
RuntimeError: CUDA out of memory
```

#### สาเหตุและการแก้ไข:

**📊 ตรวจสอบ Memory Usage**
```bash
# ดู GPU memory
nvidia-smi --query-gpu=memory.used,memory.total --format=csv

# Monitor real-time
watch -n 1 nvidia-smi
```

**⚙️ ปรับ Configuration**
```javascript
// ใน \"Configure me\" node - ลด memory usage
{
  \"batch_size\": 1,          // ลดจาก 4 เป็น 1
  \"image_batch_size\": 1,    // ลดการสร้างภาพพร้อมกัน
  \"gpu_memory_fraction\": 0.6, // ใช้ GPU memory แค่ 60%
  \"clear_cache_interval\": 2   // ล้าง cache บ่อยขึ้น
}
```

**🔧 Container Memory Settings**
```yaml
# ใน docker-compose.yml
vdo-factory:
  shm_size: 4gb  # เพิ่ม shared memory
  deploy:
    resources:
      limits:
        memory: 16g  # จำกัด system memory
```

---

### 3. GPU Utilization ต่ำ

#### อาการ:
```
GPU utilization < 30%
ความเร็วไม่เพิ่มขึ้นจาก CPU
```

#### สาเหตุและการแก้ไข:

**📈 เพิ่ม Batch Size**
```javascript
{
  \"batchSize\": 3,           // เพิ่มจาก 1
  \"concurrent_scenes\": 2,   // ทำหลาย scenes พร้อมกัน
  \"image_batch_size\": 2     // สร้างภาพหลายภาพพร้อมกัน
}
```

**🔄 ตรวจสอบ Bottlenecks**
```bash
# CPU usage
htop

# Disk I/O
iotop

# Network
nethogs

# ถ้า CPU หรือ Disk เป็น bottleneck = GPU ไม่ได้ใช้เต็มที่
```

---

### 4. Container ไม่ Start หรือ Crash

#### อาการ:
```
Container vdo-factory exited with code 1
GPU driver version mismatch
```

#### สาเหตุและการแก้ไข:

**🔍 ตรวจสอบ Logs**
```bash
# ดู container logs
docker logs vdo-factory

# ดู detailed logs
docker logs --details vdo-factory
```

**🔄 Driver Version Mismatch**
```bash
# ตรวจสอบ driver version
nvidia-smi | grep \"Driver Version\"

# อัปเดต driver
sudo apt update && sudo apt upgrade nvidia-driver-*
sudo reboot
```

**🐳 Container Runtime Issues**
```bash
# ตรวจสอบ Docker daemon
sudo systemctl status docker

# Restart Docker service
sudo systemctl restart docker

# Recreate containers
docker-compose down
docker-compose up -d
```

---

### 5. Performance Issues

#### อาการ:
```
TTS generation ช้ากว่าที่คาดหวัง
Video processing ไม่เร็วขึ้น
```

#### สาเหตุและการแก้ไข:

**🌡️ ตรวจสอบ Temperature**
```bash
# GPU temperature
nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader,nounits

# ถ้า > 80°C = thermal throttling
# แก้ไข: ปรับ fan curve, ลด clock speed
```

**⚡ Power Limit**
```bash
# ตรวจสอบ power usage
nvidia-smi --query-gpu=power.draw,power.limit --format=csv

# ถ้าใกล้ power limit = เพิ่ม power limit (ถ้าทำได้)
sudo nvidia-smi -pl 250  # เพิ่มเป็น 250W
```

**💾 Storage Performance**
```bash
# ทดสอบ disk speed
sudo hdparm -Tt /dev/sda1

# ถ้าช้า < 100 MB/s = ใช้ SSD
# Mount SSD สำหรับ temp files
```

---

### 6. Model Loading Issues

#### อาการ:
```
Failed to load model
Model not found on GPU
```

#### สาเหตุและการแก้ไข:

**📁 Model Path Issues**
```bash
# เข้าไป container
docker exec -it vdo-factory bash

# ตรวจสอบ model files
ls -la /app/models/
ls -la /app/models/kokoro/

# ถ้าไม่มี models = ดาวน์โหลดใหม่
```

**🔄 Model Loading Configuration**
```javascript
{
  \"preload_models\": true,      // โหลด models เมื่อเริ่มต้น
  \"model_cache_size\": \"4GB\",   // cache size
  \"use_cpu_fallback\": true     // ใช้ CPU ถ้า GPU ไม่ได้
}
```

---

## 🔧 Diagnostic Commands

### GPU Health Check
```bash
#!/bin/bash
echo \"=== GPU Diagnostic ===\"

echo \"1. NVIDIA Driver:\"
nvidia-smi --query-gpu=driver_version --format=csv,noheader

echo \"2. GPU Status:\"
nvidia-smi --query-gpu=name,memory.total,memory.used,temperature.gpu,utilization.gpu --format=csv

echo \"3. Docker GPU Test:\"
docker run --rm --gpus all nvidia/cuda:11.8-base-ubuntu20.04 nvidia-smi --list-gpus

echo \"4. Container GPU Access:\"
docker exec vdo-factory nvidia-smi -L

echo \"5. PyTorch GPU Test:\"
docker exec vdo-factory python -c \"import torch; print(f'CUDA: {torch.cuda.is_available()}')\"
```

### Performance Monitoring
```bash
#!/bin/bash
echo \"=== Performance Monitoring ===\"

# GPU utilization
nvidia-smi dmon -s pucvmet -d 5 -c 12 &

# Container stats
docker stats vdo-factory &

# System resources
htop &

echo \"Monitoring started. Press Ctrl+C to stop.\"
wait
```

---

## 📋 Quick Fix Checklist

### ก่อนเริ่มใช้งาน
- [ ] `nvidia-smi` ทำงานได้
- [ ] Docker GPU test ผ่าน
- [ ] Container เห็น GPU
- [ ] Model files ครบถ้วน

### เมื่อเกิดปัญหา
- [ ] ตรวจสอบ error logs
- [ ] ดู GPU memory usage
- [ ] ตรวจสอบ temperature
- [ ] Restart container
- [ ] ลด batch size

### Performance Issues
- [ ] เพิ่ม batch size
- [ ] ใช้ SSD storage
- [ ] ตรวจสอบ CPU bottleneck
- [ ] Monitor GPU utilization

---

## 💡 Prevention Tips

### 1. Regular Maintenance
```bash
# อัปเดต driver เดือนละครั้ง
sudo apt update && sudo apt upgrade nvidia-driver-*

# ล้าง Docker cache
docker system prune -f

# ตรวจสอบ disk space
df -h
```

### 2. Monitoring Setup
```bash
# ติดตั้ง monitoring tools
sudo apt install nvtop htop iotop

# สร้าง monitoring script
cat > gpu_monitor.sh << 'EOF'
#!/bin/bash
while true; do
  echo \"$(date): GPU $(nvidia-smi --query-gpu=utilization.gpu --format=csv,noheader,nounits)% Memory $(nvidia-smi --query-gpu=memory.used --format=csv,noheader) Temp $(nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader)°C\"
  sleep 5
done
EOF
chmod +x gpu_monitor.sh
```

### 3. Configuration Backup
```bash
# สำรอง working configuration
cp docker-compose.yml docker-compose.yml.backup
cp Resource/configuration.json Resource/configuration.json.backup
```

การแก้ปัญหา GPU อย่างเป็นระบบจะช่วยให้ระบบทำงานได้อย่างมั่นคงและมีประสิทธิภาพสูงสุด! 🚀