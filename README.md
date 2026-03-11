<img width="1006" height="291" alt="image" src="https://github.com/user-attachments/assets/94a5e4ef-c6af-458a-8225-552cf5318f52" /># engce-301-week12-jwt-microservices


หน้า กาฟาน่า
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/4c16bf3e-4768-463b-b287-0e1514b55c0e" />


หน้าโลกิ ทำงาน
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/aaf4281d-9b67-4e0f-a8ff-5ea06cf396bf" />


{
  "error": "Unauthorized",
  "message": "กรุณา Login ก่อน — ไม่พบ Token ใน Authorization header"
}
<img width="1002" height="201" alt="image" src="https://github.com/user-attachments/assets/5f7681f7-ca80-4b18-90ee-f309651cbb7c" />








Register และ Login เพื่อรับ Token

<img width="1008" height="265" alt="image" src="https://github.com/user-attachments/assets/b8411417-878e-41f6-b389-f9070aa3ad24" />





ลอง ล็อกอิน ผ่าน
<img width="1014" height="184" alt="image" src="https://github.com/user-attachments/assets/3a232015-1d15-496e-b474-dd6ad709ea93" />





















| Test Case           | Expected    | Actual                      | ✅/❌ | หมายเหตุ                             |
| ------------------- | ----------- | --------------------------- | --- | ------------------------------------ |
| 1. ไม่มี Token      | 401         | 401 Unauthorized            | ✅   | authMiddleware ปฏิเสธทันที           |
| 2. Login สำเร็จ     | 200 + Token | 200 + JWT Token             | ✅   | ได้ Token จาก `/api/auth/login`      |
| 3. มี Token ถูกต้อง | 200 + data  | `{"tasks":[],"total":0}`    | ✅   | JWT ผ่าน แต่ user ยังไม่มี task      |
| 4. Token Invalid    | 401         | `{"error":"Invalid Token"}` | ✅   | ระบบตรวจ signature                   |
| 5. Forbidden (403)  | 403         | 401 Unauthorized            | ❌   | Alice login ไม่สำเร็จเลยไม่ได้ token |
| 6. Admin access     | 200         | 200 + data                  | ✅   | Admin สามารถดู tasks/users           |
| 7. Rate Limit       | 429         | 429                         | ✅   | หลัง login ผิด 5 ครั้ง               |
| 8. SQL Injection    |             |                             |     |                                      |
zz

