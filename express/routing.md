
# Routing 

เปิดไฟล์ `index.js` 

เราจะเขียน Route เพิ่มสำหรับการสร้าง และจัดการ User Account (CRUD)

```js
app.post('/register', (req, res) => {
    res.send(`register`);
});

app.get('/users', (req, res) => {
    res.send(`get all users`);
});

app.get('/users/:id', (req, res) => {
    res.send(`get user with id`);
});
 
app.put('/users', (req, res) => {
    res.send('update user');
})

app.delete('/users', (req, res) => {
    res.send('delete user');
})
```

บันทึกไฟล์ และรันทดสอบการทำงานด้วย POSTMan 

