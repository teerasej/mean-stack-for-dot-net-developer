
# test การสร้าง user ใหม่


## 1. สร้าง test ทดสอบการสร้าง user ใหม่ 

เปิดไฟล์ `routes/users.test.js`

```js
it('should got user id after register', (done) => {

        request.post('/users/register')
            .send({ username: 'abc', password: 'def' })
            .expect(200)
            .then(res => {
                expect(res.body.uid).to.not.be.null; 
                done();
            });

    });
```

## 2. ทดสอบการทำงาน

รันคำสั่งด้านล่าง เพื่อให้ jest ค้นหาและรัน test ในโปรเจคของเรา 

```bash
npm test
```

หรือ

```bash
yarn test
```