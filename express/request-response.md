
# Request & Response Object 

Express เป็น Web server ที่จะรับ **Request** ที่ส่งมาจาก Client ในรูปแบบของ parameter ชื่อ `request`

ในส่วนของการตอบข้อมูลกลับไปให้ Client ก็จะอยู่ในรูปแบบของ **Response** ซึ่งแทนที่ด้วย parameter ที่ชื่อว่า `response`

## ลองอ่านค่าจาก request และใช้งาน response 

เปิดไฟล์ `routes/users.js`

```js
import express from 'express';
const router = express.Router();

router.post('/register', (req, res) => {

    const newUser = req.body;

    res.send('register');
});

router.get('/', (req, res) => {
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
