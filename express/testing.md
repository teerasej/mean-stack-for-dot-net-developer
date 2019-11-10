
# Test with JEST

## 1. ติดตั้ง module สำหรับ test

```bash
ืnpm i --save-dev babel-watch jest chai supertest 
npm i @types/supertest 
```

หรือ

```bash
yarn add --dev babel-watch jest chai supertest 
yarn add @types/supertest 
```

## 2. ตั้งค่าใน package.json 

เปิดไฟล์ `package.json`

```json
{
  ...
  "scripts": {
    "test": "jest --watchAll",
    "dev": "babel-watch index.js"
  },
  ...
  "jest": {
    "testEnvironment": "node"
  }
```

## 3. สร้าง Test แรก 

สร้างไฟล์ `routes/users.test.js`

```js
import UserRoute from "./users";
import { expect } from "chai";

// กำหนด test suite
describe('User Route', () => {

    // สร้าง test
    it('should exist', () => {
        // ใช้ expect function ของ chai ในการ validate ค่าที่ต้องการ
        expect(UserRoute).to.not.be.null;
    });

}); 
```

## 4. รัน jest

รันคำสั่งด้านล่าง เพื่อให้ jest ค้นหาและรัน test ในโปรเจคของเรา 

```bash
npm test
```

หรือ

```bash
yarn test
```