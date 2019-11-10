
# Authenticate with JWT & Test

## 1. นำ isAuthenticated function มาใช้ใน Route

เปิดไฟล์ `routes/users.js`

```js
import { isAuthenticated } from "../jwt/JWT";

...

// นำ isAuthenticated function มาใช้ใน route สำหรับแสดงข้อมูล user ในระบบ
router.get('/', isAuthenticated, (req, res) => {
    res.send('all users');
});
```

## 2. เขียน test ทดสอบ Route get /

เปิดไฟล์ `routes/users.test.js`

เราจะเขียน

```js
import UserRoute from "./users";
import { expect } from "chai";
import supertest from 'supertest';
import server from "../server";

describe('User Route', () => {

    ///// สร้าง instance ของ server เพื่อควบคุมการรันของ server และการ test
    let instance;
    let request;

    // beforeEach() จะทำงานทุกครั้งก่อน it() test จะรัน
    beforeEach(() => {
        instance = server.listen();
        request = supertest.agent(instance);
    });

    // afterEach() จะทำงานทุกครั้ง หลัง it() test รันเสร็จ
    afterEach(() => {
        instance.close();
    });

    it('should exist', () => {
        expect(UserRoute).to.not.be.null;
    });

    // ทดสอบว่าถ้าเรียก GET /users ตรงๆ จะต้องคืนค่าเป็น 401 Unauthorized
    // done เป็น function ที่จะยืนยันการทำงานของ it() แบบ asynchronous
    it('should return 401', (done) => {
        request.get('/users/')
            .expect(401)
            .end(done);
    });

    // ทดสอบว่าถ้าเรียก GET /users แบบมี token ที่ถูกต้องติดไปกับ request ด้วย จะต้องได้ 200 กลับมาเป็นการยืนยันว่าเข้าถึง route ได้
    // ค่า token สามารถหาได้จาก log ตอนรัน test ของ JWT 
    it('should return 200', (done) => {

        const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImIiLCJwYXNzd29yZCI6ImEiLCJpYXQiOjE1NzMzNzk5MzN9.y3OBo9muCDuFb7vOaRsaHKQoolrlAbUbLesvnAujmSY';

        request.get('/users/')
            .set('Authorization', 'Bearer ' + token)
            .expect(200)
            .end(done);
    });

});
```

## A. ไฟล์เต็ม `routes/users.js`

```js
import express from 'express';
import { isAuthenticated } from "../jwt/JWT";
const router = express.Router();

router.post('/register', (req, res) => {

    const newUser = req.body;

    res.send('register');
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

## B. ไฟล์เต็ม `routes/users.test.js`

```js
import UserRoute from "./users";
import { expect } from "chai";
import supertest from 'supertest';
import server from "../server";

describe('User Route', () => {

    let instance;
    let request;

    beforeEach(() => {
        instance = server.listen();
        request = supertest.agent(instance);
    });

    afterEach(() => {
        instance.close();
    });

    it('should exist', () => {
        expect(UserRoute).to.not.be.null;
    });

    it('should return 500', (done) => {
        request.get('/users/')
            .expect(500)
            .end(done);
    });

    it('should return 200', (done) => {

        const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImIiLCJwYXNzd29yZCI6ImEiLCJpYXQiOjE1NzMzNzk5MzN9.y3OBo9muCDuFb7vOaRsaHKQoolrlAbUbLesvnAujmSY';

        request.get('/users/')
            .set('Authorization', 'Bearer ' + token)
            .expect(200)
            .end(done);
    });

});
```