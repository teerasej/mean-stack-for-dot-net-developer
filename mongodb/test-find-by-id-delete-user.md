
# Test: Find by username, delete user

เปิดไฟล์ `routes/users.test.js` 



## 1. เขียน test สำหรับทดสอบการหา user ด้วยชื่อ 

```js
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
```

## 2. เขียน test สำหรับลบ user ด้วย id

```js
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
```