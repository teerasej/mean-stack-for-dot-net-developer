
# ใช้ user schema ในการสร้าง user ใหม่ลง Database

เปิดไฟล์ `routes/users.js` 

```js
// ใช้ babel-polyfill เพื่อใช้ async/await ได้
import 'babel-polyfill';
import express from 'express';
import { isAuthenticated } from "../jwt/JWT";

// นำ UsersModel มาใช้งาน
import UsersModel from "../models/UsersModel";

const router = express.Router();

router.post('/register', async (req, res) => {

    const newUser = req.body;
    console.log('newUser', newUser);

    // สร้าง UserModel object
    let userModel = new UsersModel();
    // กำหนดค่าจาก request ลง userModel
    userModel.username = req.body.username;
    userModel.password = req.body.password;

    // สั่ง save เพื่อให้ mongoose บันทึกข้อมูลลง db
    let result = await userModel.save();
    // ส่งค่า id กลับไปให้ client
    res.json({ uid: result.id });
});

...
```

## A. ไฟล์เต็ม `routes/users.js` 


```js
import 'babel-polyfill';
import express from 'express';
import { isAuthenticated } from "../jwt/JWT";
import UsersModel from "../models/UsersModel";

const router = express.Router();

router.post('/register', async (req, res) => {

    const newUser = req.body;
    console.log('newUser', newUser);

    let userModel = new UsersModel();
    userModel.username = req.body.username;
    userModel.password = req.body.password;
    let result = await userModel.save();

    res.json({ uid: result.id });
});

router.get('/', isAuthenticated, (req, res) => {
    res.send('all users');
});

router.get('/:id', (req, res) => {
    res.send(`user with id ${req.params.id}`);
});
 
router.put('/', (req, res) => {

    const targetUser = req.body;
    res.send(`update user ${targetUser.id}`);

})

router.delete('/', (req, res) => {
    
    const targetUser = req.body;
    res.send(`delete user ${targetUser.id}`);

})


export default router;
```