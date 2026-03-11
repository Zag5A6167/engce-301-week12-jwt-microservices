ADR-001: เพิ่ม Auth Service แยกจาก Task Service
Status
Accepted

Context
เดิมโปรเจกต์ Task Board ในสัปดาห์ที่ 6-7 เป็นสถาปัตยกรรมแบบ N-Tier ที่เน้นความง่ายในการเชื่อมต่อ โดย Task Service ต้องรับหน้าที่จัดการทั้งธุรกิจหลัก (Task Management) และความปลอดภัย (Authentication) ภายในตัวเดียว (Monolithic-like approach)

ปัญหาที่พบคือ:

Scalability: หากมีการ Login เข้ามาพร้อมกันจำนวนมาก จะทำให้ประสิทธิภาพการดึงข้อมูล Task ช้าลงไปด้วย

Security Risk: หากโค้ดส่วน Task มีช่องโหว่ อาจส่งผลกระทบโดยตรงต่อฐานข้อมูลรหัสผ่านของ User

Tight Coupling: การแก้ไขระบบ Login ทำได้ยากเพราะผูกติดกับ Business Logic ของ Task

Decision
ตัดสินใจแยก Auth Service ออกมาเป็น Microservice อิสระ และเปลี่ยนมาใช้ระบบ JWT (JSON Web Token) ในการสื่อสารระหว่าง Service แทน โดยมีรายละเอียดดังนี้:

สร้าง Auth Service เพื่อจัดการ Register, Login และการออก Token โดยเฉพาะ

ใช้ API Gateway (Nginx) ทำหน้าที่เป็นด่านหน้าในการทำ Rate Limiting และเบื้องต้นในการตรวจ Token

ใช้กลยุทธ์ Database-per-Service โดยแยก auth_db ออกจาก task_db อย่างเด็ดขาด

Consequences
Positive:

Separation of Concerns: แต่ละ Service มีหน้าที่ชัดเจน ง่ายต่อการพัฒนาและ Debug (Senior Developer Style)

Independent Scaling: สามารถเพิ่มจำนวน Container เฉพาะ Auth Service ได้เมื่อมีปริมาณ User สูงขึ้น

Stronger Security: ฐานข้อมูลรหัสผ่านถูกแยกออกจากฐานข้อมูลงาน (Task) ลดความเสี่ยงจากการถูกโจมตีแบบเจาะจง

Negative:

Increased Complexity: ต้องจัดการ Docker Compose และ Network ระหว่าง Service ที่ซับซ้อนขึ้น

Operational Overhead: ต้องดูแลรักษา Service และ Database เพิ่มขึ้นเป็นเท่าตัว

Trade-offs:

Latancy vs Security: การต้องตรวจสอบ Token ทุกครั้งที่เรียก API อาจทำให้เกิดความล่าช้า (Latency) เล็กน้อย แต่แลกมาด้วยความปลอดภัยระดับ Zero-Trust ที่เข้มงวดกว่าเดิม

Development Speed: ในช่วงแรกจะใช้เวลา Setup นานขึ้น แต่ในระยะยาวการขยายระบบ (Maintainability) จะทำได้ดีกว่าเดิมมากz