
# Routing Module

เราจะสร้างไฟล์ Javascript แยกออกมาสำหรับจัดการ Route URL สำหรับ User Account โดยเฉพาะ 

เริ่มจากการสร้างไฟล์ `routes/users.js`

## 1. สร้าง Router จาก Express

เราจะ import express มาไว้ที่ไฟล์นี้ และเรียกใช้ express.Router() เพื่อสร้าง router object

```js
import express from 'express';

const router = express.Router();
```

โดยเราสามารถกำหนด route ใส่ router object ได้ตามปกติ

```js
router.post('/register', (req, res) => {
    res.send('register');
});

router.get('/', (req, res) => {
    res.send('all users');
});

router.get('/:id', (req, res) => {
    res.send(`user with id ${req.params.id}`);
});

router.put('/', (req, res) => {
    res.send('update user');
})

router.delete('/', (req, res) => {
    res.send('delete user');
})
```

จากนั้น export ออกไปใช้งานด้วยคำสั่ง 

```js
export default router; 
```

## 2. เรียกใช้ `routes/users.js` เป็น Middleware

เปิดไฟล์ `index.js` 

เราจะเขียนคำสั่ง import `./routes/users` ในชื่อของ `UsersRoute`

```js
...
import UsersRoute from "./routes/users";

const app = express();
const port = 3000;

app.use(express.json()) // for parsing application/json
app.use(express.urlencoded({ extended: true })) // for parsing application/x-www-form-urlencoded

app.get('/', (req, res) => {
    res.status(200).json({ status: 'ok' } );
});

// ไว้ตรงนี้
app.use('/users', UsersRoute);


app.listen(port, () => console.log(`Express server running on port ${port}`));
```

## A. ไฟล์เต็ฒ `index.js`

```js
import express from 'express';
import UsersRoute from "./routes/users";

const app = express();

const port = 3000;

app.use(express.json()) // for parsing application/json
app.use(express.urlencoded({ extended: true })) // for parsing application/x-www-form-urlencoded

app.get('/', (req, res) => {
    res.status(200).json({ status: 'ok' } );
});

app.use('/users', UsersRoute);

app.listen(port, () => console.log(`Express server running on port ${port}`));
```


## B. ไฟล์เต็ม `routes/users.js`

```js
import express from 'express';

const router = express.Router();

router.post('/register', (req, res) => {
    res.send('register');
});

router.get('/', (req, res) => {
    res.send('all users');
});

router.get('/:id', (req, res) => {
    res.send(`user with id ${req.params.id}`);
});

router.put('/', (req, res) => {
    res.send('update user');
})

router.delete('/', (req, res) => {
    res.send('delete user');
})

export default router; 
```