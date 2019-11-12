
# Implement Route: แสดงรายชื่อของ User ทั้งหมด

## 1. สร้าง function เพื่อใช้สร้าง mock up data 

เปิดไฟล์ `models/UsersModel.js` 

โดยเราจะ insert 10 users ใน User Collection

```js
export const populateMockData = () => {
  let user = DB.model('User', userSchema);
  user.create([{
    username: "Camile",
    password: "fQdUrvYrPOC"
  }, {
    username: "Maribel",
    password: "hAUuqY3fu"
  }, {
    username: "Vicky",
    password: "4lzMQsYaQ"
  }, {
    username: "Jacquetta",
    password: "LZIUYmSZWwI2"
  }, {
    username: "Anne-corinne",
    password: "nWkOrbFb5"
  }, {
    username: "Kori",
    password: "NaHiJTW0F"
  }, {
    username: "Carry",
    password: "I3nZ77WiigR"
  }, {
    username: "Kira",
    password: "vZ5zJTXk"
  }, {
    username: "Sabina",
    password: "DVj2L3Uk"
  }, {
    username: "Rheta",
    password: "jhV6T37"
  }])
}
```

## 2. เขียน test สำหรับการเรียก GET /users

เริ่มจาก import `populateMockData`

```js
import UserModel, { populateMockData } from "../models/UsersModel";
```

และเรียกใช้ function ใน `beforeEach()` ซึ่งจะทำงานทุกครั้งก่อนรัน test

```js
beforeEach(() => {
    instance = server.listen();
    request = supertest.agent(instance);
    populateMockData();
});
```

จากนั้นเขียน test เพื่อทดสอบว่ามีค่า array คืนออกมาจาก route จริง

```js
it('should return array of users', (done) => {
        request.get('/users/')
        .set('Authorization', 'Bearer ' + token)
        .expect(200)
        .then(res => {
            expect(res.body).to.be.an('array');
            expect(res.body.length).to.greaterThan(0);
            done();
        });
    });
```

## 3. ทำ route `GET /users` ให้ return users ทั้งหมด

เปิดไฟล์ `routes/users.js` และลงมาที่ route `get /`

ทำการเรียกใช้ `UsersModel.find()` เพื่อค้นหาทุก user ใน collection 

```js
router.get('/', isAuthenticated, async (req, res) => {

    let result = await UsersModel.find();
    res.json(result);

});
```