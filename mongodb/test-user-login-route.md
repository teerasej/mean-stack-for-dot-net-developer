
# Test: ทดสอบการ login แบบต่างๆ

เปิดไฟล์ `routes/users.test.js`

เราจะนำเข้า `mockUserArray` เพื่อนำข้อมูล user จำลองมาใช้ในการ test

```js
import UserModel, { populateMockData, mockUserArray } from "../models/UsersModel";
```

จากนั้นเพิ่ม test case ต่างๆ สำหรับ POST `/users/login`

```js

    it('login should return 422 if request has invalid json', () => {
        const json = { user:'1', pass:'1'};

        request.post('/users/login')
            .send(json)
            .expect(422)
            .end();
    });

    it('login should return 422 if request has no json', () => {
        request.post('/users/login')
        .expect(422)
        .end();
    });

    it('login should return 404 if user not found in system', () => {
        const json = { username:'1', password:'1'};

        request.post('/users/login')
            .send(json)
            .expect(404)
            .end();
    });

    it('login should return 200 with token if user found in system', () => {
        const json = mockUserArray[0];

        request.post('/users/login')
            .send(json)
            .expect(200)
            .then(res => {
                expect(res.body.token).to.be.a('string');
            })
    });

```

## A. ไฟล์เต็ม `routes/users.test.js`

```js
import UserRoute from "./users";
import { expect } from "chai";
import supertest from 'supertest';
import server from "../server";

import UserModel, { populateMockData, mockUserArray } from "../models/UsersModel";

describe('User Route', () => {

    const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImIiLCJwYXNzd29yZCI6ImEiLCJpYXQiOjE1NzMzNzk5MzN9.y3OBo9muCDuFb7vOaRsaHKQoolrlAbUbLesvnAujmSY';
    let instance;
    let request;

    beforeEach(() => {
        instance = server.listen();
        request = supertest.agent(instance);
        populateMockData();
    });

    afterEach(()=>{
        UsersModel.db.dropCollection('users');
    })

    afterAll(() => {
        instance.close();
    });

    it('should exist', () => {
        expect(UserRoute).to.not.be.null;
    });

    it('should return 401', (done) => {
        request.get('/users/')
            .expect(401)
            .end(done);
    });

    it('should return 200', (done) => {
        
        request.get('/users/')
            .set('Authorization', 'Bearer ' + token)
            .expect(200)
            .end(done);
    });

    it('should got user id after register', (done) => {

        request.post('/users/register')
            .send({ username: 'abc', password: 'def' })
            .expect(200)
            .then(res => {
                expect(res.body.uid).to.not.be.null; 
                done();
            });
        
    });

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

    it('should return a user with name', (done) => {

        const username = 'Camile'

        request.get('/users/username/' + username)
        .set('Authorization', 'Bearer ' + token)
        .expect(200)
        .then(res => {
            expect(res.body._id).to.be.not.null;
            expect(res.body.username).to.equal(username);
            expect(res.body._id).to.be.not.null;
            done();
        });
    });

    it('should remove target user', (done) => {

        const username = 'Camile'

        request.get('/users/username/' + username)
        .set('Authorization', 'Bearer ' + token)
        .expect(200)
        .then(res => {
            expect(res.body._id).to.be.not.null;
            expect(res.body.username).to.equal(username);
            expect(res.body._id).to.be.not.null;
            
            let targetId = res.body._id

            request.delete('/users/')
            .set('Authorization', 'Bearer ' + token)
            .send({ id: targetId })
            .expect(200)
            .then(res => {
                expect(res.body._id).to.equal(targetId);
                done();
            });

        });
    });

    it('login should return 422 if request has invalid json', () => {
        const json = { user:'1', pass:'1'};

        request.post('/users/login')
            .send(json)
            .expect(422)
            .end();
    });

    it('login should return 422 if request has no json', () => {
        request.post('/users/login')
        .expect(422)
        .end();
    });

    it('login should return 404 if user not found in system', () => {
        const json = { username:'1', password:'1'};

        request.post('/users/login')
            .send(json)
            .expect(404)
            .end();
    });

    it('login should return 200 with token if user found in system', () => {
        const json = mockUserArray[0];

        request.post('/users/login')
            .send(json)
            .expect(200)
            .then(res => {
                expect(res.body.token).to.be.a('string');
            })
    });

});
```