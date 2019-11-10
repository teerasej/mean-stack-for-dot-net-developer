
# Middleware

Middleware คือรูปแบบ function ที่ Express กำหนดขึ้นมา เพื่อให้เราจัดการ request หรือ respose ที่วิ่งเข้ามาในระบบได้อย่างอิสระ 

เราอาจจะเรียก middleware ว่า plug-in ก็ได้ครับ 

## 1. กำหนด Middleware ของ Express ในการถอดข้อมูลจาก Client Request 

เปิดไฟล์ `index.js`

เราจะใช้คำสั่ง `use()` ของ express ในการกำหนด middleware ลงไป 2 ตัว

1. `express.json()` สำหรับการถอดข้อมูล json มาใช้งาน
2. `express.urlencoded({ extended: true })` สำหรับการถอดข้อมูลจาก web form ทั่วไป มาใช้งาน

```js
import express from 'express';

const app = express();

const port = 3000;

app.use(express.json()) // for parsing application/json
app.use(express.urlencoded({ extended: true })) // for parsing application/x-www-form-urlencoded
...
```