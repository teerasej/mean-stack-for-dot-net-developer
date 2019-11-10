
# 

## 1. สร้างไฟล์สำหรับ default route

สร้างไฟล์ `routes/default.js`

และย้าย default route จาก `index.js` มาไว้ที่นี่แทน

```js
import express from "express";

const router = express.Router();

router.get('/', (req, res) => {
    res.status(200).json({ status: 'ok' } );
});


export default router 
```

## 2. เรียกใช้ DefaultRoute เป็น middleware ใน index.js แทน 

แก้ไขไฟล์ `index.js` ให้เป็นแบบด้านล่าง

```js
import express from 'express';
import UsersRoute from "./routes/users";

// import default route มาใช้งาน
import DefaultRoute from "./routes/default";

const app = express();

const port = 3000;

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// กำหนด path ของ default route
app.use('/', DefaultRoute);
app.use('/users', UsersRoute);


app.listen(port, () => console.log(`Express server running on port ${port}`));
```