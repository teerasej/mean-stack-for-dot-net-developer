
# สร้าง Schema Model

สร้างไฟล์ `models/UsersModel.js`

```js
import DB from "./db";
const Schema = DB.Schema;

// กำหนด field ใน schema 
const userSchema = new Schema({
  username: String,
  password: String,
});

// ใช้ mongoose สร้าง Model ของ collections `users` และ export ออกไปพร้อมใช้ 
export default DB.model('User', userSchema);
```