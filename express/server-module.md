
# Separate Server Setup

เพื่อให้การทดสอบ Web API เป็นการทดสอบตัว server ได้โดยไม่ต้องรัน server ขึ้นมาจริงๆ (`listen()` เป็นการสั่งรัน server จริง)

เราจำเป็นต้องแยกการสร้าง และตั้งค่า route ต่างๆ ใน server ออกมาเป็น module ต่างหาก ไม่รวมอยู่ใน `index.js`

## 1. สร้างไฟล์​ `server.js`

และย้ายส่วนของการ config middleware และ route มาไว้ใน `server.js`

```js
import express from 'express';
import UsersRoute from "./routes/users";
import DefaultRoute from "./routes/default";

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.use('/', DefaultRoute);
app.use('/users', UsersRoute);

// จากนั้นก็ export ตัว app (server instance) ออกไปใช้งาน
export default app;
```

## 2. ปรับไฟล์ index.js

```js
// สังเกตว่าเรานำเอา server ที่ config เสร็จแล้วมารันในไฟล์ index.js เหมือนเดิม (เพราะ package.json สั่งรันไฟล์นี้เป็นตัวเริ่มต้น)
import app from "./server";

const port = 3000;
app.listen(port, () => console.log(`Express server running on port ${port}`));
```