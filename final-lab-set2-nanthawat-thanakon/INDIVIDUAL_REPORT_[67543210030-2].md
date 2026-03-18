# INDIVIDUAL_REPORT_675432100401.md
## Final Lab Sec2 Set 2

## 1. ข้อมูลผู้จัดทำ

| | |
|---|---|
| **ชื่อ-นามสกุล** | นายธนกร ผดุงศิลป์ |
| **รหัสนักศึกษา** | 67543210030-2 |
| **วิชา** | ENGSE207 Software Architecture |
| **งาน** | Final Lab Sec2 Set 2 — Microservices + Activity Tracking + Cloud (Railway) |

---

## 2. ส่วนที่รับผิดชอบ

- Task Service (`task-service/`) ทั้งหมด — เพิ่ม logActivity() ทุก CRUD route
- Activity Service (`activity-service/`) ทั้งหมด — สร้างใหม่
- Frontend (`frontend/index.html`, `frontend/activity.html`)

---

## 3. สิ่งที่ลงมือทำจริง

### Task Service — เพิ่ม logActivity()
เพิ่ม logActivity() แบบ Fire-and-forget ในทุก CRUD และ Denormalize ฟิลด์ username ลงในตาราง Tasks

### Activity Service — สร้างใหม่ทั้งหมด
สร้าง 4 Endpoints (Internal, Me, All, Health) และออกแบบ Database ให้รองรับการเก็บประวัติกิจกรรมแยกอิสระ

### Frontend — ปรับจาก Set 1
ปรับปรุง index.html ให้รองรับระบบ Register/Login แยก Service และสร้าง activity.html เพื่อแสดง Timeline กิจกรรมพร้อมระบบ Filter และ Stats

---

## 4. ปัญหาที่พบและวิธีแก้ (อย่างน้อย 2 ปัญหา)

**ปัญหา 1: TASK_STATUS_CHANGED ไม่แสดงผลเมื่อแก้ไขแค่ชื่อ Task**
วิธีแก้: เพิ่มเงื่อนไขตรวจสอบการเปลี่ยนแปลงของ Status ก่อนส่ง Event เพื่อให้ Timeline แสดงผลเฉพาะเมื่อมีการเปลี่ยนสถานะจริง

**ปัญหา 2: Activity Timeline ไม่แสดงข้อมูลหลัง Deploy**
วิธีแก้: ตรวจสอบ Environment Variables พบว่าขาด https:// ใน URL ของ Service บน Railway จึงทำการแก้ไขให้ครบถ้วน

---

## 5. อธิบาย: Denormalization ใน activities table คืออะไร และทำไมต้องทำ

คือการเก็บ username ไว้ในตาราง activities ทันทีที่เกิดเหตุการณ์ แทนที่จะเก็บแค่ user_id
เหตุผล: เนื่องจากแต่ละ Service แยก Database กัน จึงไม่สามารถ JOIN ตารางข้ามกันได้
ประโยชน์: ช่วยให้ Activity Service แสดงชื่อผู้ใช้ได้ทันทีโดยไม่ต้องเรียกถาม Auth Service ลดความซับซ้อนและ Network Latency
ผลคือ activity-service แสดง username ได้โดยไม่ต้อง call กลับไปหา auth-service เลย

---

## 6. อธิบาย: ทำไม logActivity() ต้องเป็น fire-and-forget
คือการส่งข้อมูลกิจกรรมออกไปแล้วทำงานส่วนหลักต่อทันทีโดยไม่รอผลตอบกลับ
เหตุผล: เพื่อป้องกัน Cascading Failure หาก Activity Service ทำงานช้าหรือล่ม ระบบหลัก (เช่น การสร้าง Task) จะต้องยังทำงานได้ปกติ
ประโยชน์: เพิ่มความเร็ว (Response Time) ให้กับผู้ใช้งาน เพราะไม่ต้องรอผลการบันทึก Log ที่เป็นส่วนเสริม

---

## 7. ส่วนที่ยังไม่สมบูรณ์หรืออยากปรับปรุง

- Event Coverage: บันทึกกิจกรรมทุกครั้งที่มีการแก้ไขข้อมูล ไม่ใช่แค่ตอนเปลี่ยนสถานะ
- Real-time UI: ใช้ WebSockets เพื่อให้อัปเดต Timeline ได้ทันทีโดยไม่ต้องกด Refresh
- Pagination: ระบบแบ่งหน้าฝั่ง Server เพื่อรองรับข้อมูลกิจกรรมจำนวนมาก