
# Validate Request ด้วย Joi

## 1. ติดตั้ง module 

```bash
ืnpm i @hapi/joi 
```

หรือ

```bash
yarn add @hapi/joi
``` 

## 2. เรียกใช้ @hapi/joi

เปิดไฟล์ `routes/users.js`

```js
import express from 'express';
import { isAuthenticated } from "../jwt/JWT";
import UsersModel from "../models/UsersModel";
import Joi from "@hapi/joi";

const router = express.Router();

router.post('/register', async (req, res) => {

    const newUser = req.body;

    // สร้าง Schema สำหรับ validation 
    // กำหนด schema เป็น object ที่มี field (keys) ต่อไปนี้...
    const userValidateJoi = Joi.object().keys({
        // กำหนด username อย่างน้อย 1 ตัวอักษร
        username: Joi.string().required().min(1),
        // กำหนด password อย่างน้อย 1 ตัวอักษร
        password: Joi.string().required().min(1)
    });

    // ใช้ schema validate object 
    const validateResult = userValidateJoi.validate(newUser);
    // result จะมีค่า error กับ value มาเช็คได้
    const { error, value } = validateResult;

    // ถ้า error != undefined แสดงว่า validate ไม่ผ่าน 
    // return code 402
    if(error) {
        res.status(422).json({
            message: 'Invalid request',
            data: error
        })
    }

    console.log('newUser', newUser);

    let userModel = new UsersModel();

    ...
```

## 3. ทดสอบการทำงาน

รันคำสั่งด้านล่าง เพื่อให้ jest ค้นหาและรัน test ในโปรเจคของเรา 

```bash
npm test
```

หรือ

```bash
yarn test
```