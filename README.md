# รายงานแบบฝึกหัด Socket API & Docker (Assignment 1 & 2)

**ข้อมูลผู้จัดทำ**
- ชื่อ-นามสกุล: เลิศชัย เรืองทรัพย์
- รหัสนักศึกษา: 6814951025
- กลุ่ม/เซคชัน: 1

---

## แบบฝึกหัดที่ 1: ระบบ Chat ด้วย Docker (2 Clients & 1 Server)

### คำอธิบายการทำงาน
ระบบแชทใช้ TCP Socket ส่งข้อความระหว่าง Client 2 ตัวผ่าน Server กลาง Server รับข้อความจาก Client หนึ่งแล้วกระจายไปยัง Client อื่นที่เชื่อมต่ออยู่ การทำงานแบ่งเป็น 3 containers และจัดการด้วย Docker Compose โดย Client เชื่อมต่อ Server ผ่านชื่อ service `server` บนพอร์ต `65432` ภายในเครือข่าย Docker

### วิธีเริ่มใช้งาน
เปิด Terminal ที่โฟลเดอร์โปรเจกต์ แล้วเริ่ม Server:

```bash
docker compose up --build -d server
```

เปิด Terminal อีก 2 หน้าต่าง แล้วเริ่ม Client แยกกัน:

```bash
docker compose run --rm client1
```

```bash
docker compose run --rm client2
```

พิมพ์ข้อความในหน้าต่าง Client หนึ่ง ข้อความจะแสดงในอีกหน้าต่างหนึ่ง เมื่อต้องการหยุด กด `Ctrl+C` ในหน้าต่าง Client แล้วปิด Server:

```bash
docker compose down
```

### ผลการทำงาน
เมื่อ Client ทั้งสองเชื่อมต่อสำเร็จ Server จะแสดงสถานะการเชื่อมต่อ เมื่อ Client ส่งข้อความ Server รับและส่งต่อให้ Client อีกตัว จากนั้น Server แสดงข้อความเมื่อ Client ตัดการเชื่อมต่อ

---

## แบบฝึกหัดที่ 2: เว็บแอป Flask เชื่อมต่อ Socket Server ด้วย Docker

### คำอธิบายการทำงาน
เว็บแอป Flask แสดงแบบฟอร์มให้ผู้ใช้กรอกข้อความ เมื่อส่งฟอร์ม แอปจะเชื่อมต่อไปยัง Socket Server ผ่าน TCP ที่พอร์ต `12345` และส่งข้อความไปให้ Server จากนั้น Server ตอบกลับข้อความในรูปแบบ `Echo: <ข้อความ>` เว็บแอปแสดงทั้งข้อความที่ส่งและคำตอบที่ได้รับ ทั้งสองส่วนทำงานแยกกันใน containers และสื่อสารผ่านชื่อ service `socket_server`

### วิธีเริ่มใช้งาน
เข้าโฟลเดอร์ `socket` แล้วสร้างและเริ่ม containers:

```bash
cd socket
docker compose up --build -d
```

เปิดเว็บเบราว์เซอร์ไปที่ `http://localhost:5000` กรอกข้อความในแบบฟอร์มแล้วกด **Send** ระบบจะแสดงข้อความที่ส่งและคำตอบจาก Socket Server

ดูบันทึกการทำงานของ Socket Server ได้ด้วยคำสั่ง:

```bash
docker compose logs -f socket_server
```

เมื่อใช้งานเสร็จ ปิด containers:

```bash
docker compose down
```

### ผลการทำงาน
เมื่อส่งข้อความผ่านหน้าเว็บ แอป Flask ส่งข้อมูลไปยัง Socket Server ซึ่งพิมพ์ข้อความที่ได้รับลงใน log และตอบกลับด้วยข้อความ Echo ตัวอย่างเช่น ส่ง `Hello` แล้วได้รับ `Echo: Hello`
