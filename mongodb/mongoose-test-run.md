
#  เชื่อมต่อกับ MongoDB และทดสอบใช้งาน

## 1. สร้างไฟล์สำหรับ config ตัว mongoose connection

สร้างไฟล์ `models/db.js`

```js
const mongoose = require('mongoose');

// ต่อ mongodb
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});

// สร้าง model
const Cat = mongoose.model('Cat', { name: String });

// กำหนดและสร้าง instance ของ model
const kitty = new Cat({ name: 'Zildjian' });

// บันทึกข้อมูล และเรียกใช้ callback function
kitty.save().then(() => console.log('meow'));

// export ตัว mongoose connection เผื่อเอาไปใช้ที่อื่น
export default mongoose;
```

## 2. ทดสอบรัน db.js

เปิดไฟล์ `index.js`

```js
import app from "./server";

// ทดสอบการทำงานของ mongoose
import db from "./models/db";

const port = 3000;
app.listen(port, () => console.log(`Express server running on port ${port}`));
```

เสร็จแล้วบันทึกไฟล์ และรันคำสั่ง `npm dev`
น่าจะเห็นคำว่า `meow` แสดงขึ้นมาใน log

## 3. ใช้ mongo shell ตรวจสอบข้อมูล 

เปิด terminal อีกหน้าต่างหนึ่ง และใช้คำสั่งต่อไปนี้เช็คข้อมูล 

1. เรียกใช้ mongo shell (ค่าเริ่มต้นของ connection จะอยู่ที่ localhost:27017)

```bash
mongo 
```
2. ต่อเข้า database ชื่อ `test` 

```bash
use test 
```

3. แสดง collection ใน database

```bash
show collections
```

4. แสดงข้อมูลใน **cats** collection

```bash
db.cats.find()
```

## 4. เสร็จแล้วให้ comment คำสั่งทดสอบเอาไว้ 

เปิดไฟล์ `models/db.js`

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true});

// const Cat = mongoose.model('Cat', { name: String });
// const kitty = new Cat({ name: 'Zildjian' });
// kitty.save().then(() => console.log('meow'));

export default mongoose;
```
