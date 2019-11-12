
# Drop collection ในการ test

เราจะใช้ประโยชน์จาก Schema `UserModel` ในการทดสอบด้วย ซึ่งจะมีการสร้างข้อมูลจำลอง และทำลายทิ้งหลังจากการ test เสร็จสิ้น

## 1. เพิ่ม function populateMockData เอาไว้ใช้

เปิดไฟล์ `models/UsersModel.js`

```js
export const mockUserArray = [{
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
}];

export const populateMockData = () => {
  let user = DB.model('User', userSchema);
  user.create(mockUserArray);
}
```

## 2. เขียน test ทดสอบ mock array ของ user 

สร้างไฟล์ `models/UsersModel.test.js`

```js
import { expect } from "chai";
import { mockUserArray } from "./UsersModel";

describe('UsersModel test', () => {
    
    it('should get array of 10 users', () => {
        expect(mockUserArray).to.be.an('array');
        expect(mockUserArray.length).to.be.equal(10);
    });

});
```

## 3. กำหนดใช้สร้างข้อมูลใน Database ก่อนเริ่มการ test และทำลายทิ้งหลัง test เสร็จ

เปิดไฟล์ `routes/users.test.js`

```js
import UserModel, { populateMockData } from "../models/UsersModel";

describe('User Route', () => {

    // แยก token ออกมาจาก test เดิม เพื่อจะได้ใช้ใน test อื่น
    const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImIiLCJwYXNzd29yZCI6ImEiLCJpYXQiOjE1NzMzNzk5MzN9.y3OBo9muCDuFb7vOaRsaHKQoolrlAbUbLesvnAujmSY';
    let instance;
    let request;

    beforeEach(() => {
        instance = server.listen();
        request = supertest.agent(instance);

        // สร้างข้อมูลจำลอง ก่อนรัน test 
        populateMockData();
    });

    // drop collection ทิ้ง หลังรัน test แต่ละตัวเสร็จ
    afterEach(()=>{
        if( UserModel.collection.length > 0 ) {
            UserModel.collection.drop();
        }
    })

    afterAll(() => {
        instance.close();
    });

    ...
```




