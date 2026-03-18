# INDIVIDUAL REPORT — ENGSE207 Software Architecture
**Final Lab Set 2 (Section 2): Microservices + Activity Tracking + Cloud Deployment**

---

## ข้อมูลผู้จัดทำ
**ชื่อ:** นันทวัฒน์ แซ่ย่าง  
**รหัสนักศึกษา:** 67543210034-4  

---

## ส่วนที่รับผิดชอบ
- Auth Service (login + register + JWT)  
- Task Service (CRUD)  
- Activity Service integration (fire-and-forget logging)  
- Database (PostgreSQL)  

---

## สิ่งที่พัฒนาด้วยตนเอง
- เขียน API สำหรับ login และ register พร้อมตรวจสอบ password ด้วย bcrypt  
- สร้าง JWT token และ implement middleware สำหรับ verify  
- พัฒนา CRUD operations สำหรับ task  
- เชื่อมต่อระบบ logging กับ Activity Service  
- ออกแบบ schema และใช้งาน Database-per-Service บน PostgreSQL  

---

## ปัญหาที่พบและวิธีแก้ไข
- JWT ไม่ถูกส่งใน header → ตรวจสอบ Authorization header ก่อน validate  
- login หรือ register ไม่ผ่าน → ตรวจสอบ seed users และ validation rules  
- container connect กันไม่ได้ → แก้ด้วยปรับ docker-compose network  
- Activity Service event ไม่ถูกบันทึก → implement fire-and-forget `.catch(() => {})`  

---

## สิ่งที่ได้เรียนรู้
- การใช้ JWT สำหรับ authentication และ authorization  
- การออกแบบ Database-per-Service pattern และ denormalization  
- การ implement Service-to-Service call แบบ fire-and-forget  
- การ deploy microservices + databases บน Railway  
- การใช้ Docker Compose สำหรับ local testing  
- การทำ centralized logging และ activity tracking  

---

## แนวทางพัฒนาใน Set 2
- แยก database ต่อ service (auth-db, task-db, activity-db)  
- Deploy ทุก service บน Railway Cloud  
- ใช้ HTTPS certificate จริง (Railway จัดการให้)  
- เพิ่ม monitoring และ security  
- พัฒนา Frontend + Gateway Strategy ให้เรียก services ผ่าน config  

---

## Phase 1: ปรับ Codebase ให้พร้อม Deploy
**งานหลัก:**  
- เพิ่ม Register API ใน Auth Service  
- สร้าง Activity Service ใหม่  
- Implement logActivity() ใน Auth และ Task Services  
- ทดสอบทุก service ใน Local Docker Compose  

**เวลาโดยประมาณ:** 90 นาที  

---

## Phase 2: Deploy Auth Service + auth-db บน Railway
- สร้าง project บน Railway สำหรับ Auth Service  
- ใช้ PostgreSQL plugin สำหรับ auth-db  
- Push code และตั้ง environment variables: JWT_SECRET, DATABASE_URL  
- Test API endpoints: /register, /login, /me  

**เวลาโดยประมาณ:** 50 นาที  

---

## Phase 3: Deploy Task Service + task-db บน Railway
- สร้าง project บน Railway สำหรับ Task Service  
- ใช้ PostgreSQL plugin สำหรับ task-db  
- Push code และตั้ง environment variables  
- Test API endpoints: CRUD tasks + JWT middleware  

**เวลาโดยประมาณ:** 45 นาที  

---

## Phase 4: Deploy Activity Service + activity-db บน Railway
- สร้าง project บน Railway สำหรับ Activity Service  
- ใช้ PostgreSQL plugin สำหรับ activity-db  
- Push code และตั้ง environment variables  
- Test API endpoints: /internal, /me, /all  

**เวลาโดยประมาณ:** 45 นาที  

---

## Phase 5: Frontend + Gateway Strategy
- Configure frontend (config.js) เพื่อเรียก Auth, Task, Activity services  
- ทดสอบ End-to-End flows: Register → login → create task → check activity log  

**เวลาโดยประมาณ:** 60 นาที  

---

## Phase 6: Test Cases, Screenshots, Documentation
- เตรียม README, TEAM_SPLIT, INDIVIDUAL_REPORT, Screenshots  
- Push ทั้งหมดเข้า Git Repository ของ Set 2  

**เวลาโดยประมาณ:** 70 นาที  

---

## สรุป
Set 2 พัฒนาต่อยอดจาก Set 1 โดยเน้น:  
- Microservices ที่สามารถสื่อสารกันเอง (Service-to-Service call)  
- Database-per-Service pattern  
- Deploy บน cloud (Railway) พร้อม HTTPS อัตโนมัติ  

**วัตถุประสงค์สำคัญ (CLOs):**  
- CLO3: Database design และ denormalization  
- CLO6: การขยายระบบและ Service-to-Service call  
- CLO7 & CLO14: Deploy microservices + cloud database
