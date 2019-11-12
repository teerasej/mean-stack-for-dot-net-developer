
# สร้าง function ไว้เช็คว่ามี User ใน Database หรือไม่

## 1. สร้างไฟล์ `models/UsersService.js`

สร้างไฟล์ `models/UsersService.js`

```js
import 'babel-polyfill';
import UsersModel from "./UsersModel";

export const isUserExist = async (username, password) => {

    const result = await UsersModel.findOne({ username: username, password: password });

    if(result) {
        return true;
    } else {
        return false;
    }

} 
```

## 2. สร้าง test สำหรับ **isUserExist** function 

สร้างไฟล์ `models/UsersService.js`

```js

import { expect } from "chai";
import { isUserExist } from "./UsersService";
import UsersModel, { mockUserArray, populateMockData } from './UsersModel';

describe('User Service', () => {

    beforeEach(() => {
        populateMockData();
    });

    afterEach(()=>{
        if( UsersModel.collection.length > 0 ) {
            UsersModel.collection.drop();
        }
    })

    it('should found mock user', async (done) => {

        let firstMockUser = mockUserArray[0];

        const result = await isUserExist(firstMockUser.username, firstMockUser.password);
        expect(result).to.be.true;
        done();
    });

    it('should not found user', async (done) => {

        const result = await isUserExist('2134', '12312');
        expect(result).to.be.false;
        done();
    });

}); 
```