
# สร้าง Server process

เปิดไฟล์ `index.js` 

```js
// import express module มาใช้งาน
import express from 'express';

const app = express();

const port = 3000;

// กำหนด route แรก 
app.get('/', (req, res) => {
    // response กลับไปที่ client เป็น status code 200 และ json แสดงคำว่า ok
    res.status(200).json({ status: 'ok' } );
});

// สั่งให้รับ request ที่ port ที่กำหนดไว้
app.listen(port, () => console.log(`Express server running on port ${port}`));
```

## 2. ทดสอบการทำงาน

รันโค้ด `npm dev` หรือ `yarn dev` เพื่อทดสอบการทำงาน

และเปิด Web browser ไปที่ http://localhost:3000/