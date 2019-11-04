
# ติดตั้ง Babel เพื่อใช้ความสามารถเพิ่มเติม

## 1. ติดตั้ง babel module 

รันคำสั่งด้านล่างในโปรเจค 

```bash
npm install --save-dev babel-cli @babel/core @babel/preset-env babel-watch
```

หรือ 

```bash
yarn add --dev babel-cli @babel/core @babel/preset-env babel-watch
```

## 2. สร้างไฟล์ config สำหรับ babel 

สร้างไฟล์ `.babelrc` ไว้ในโปรเจค และเพิ่มค่านี้ลงไป

```json
{
  "presets": ["@babel/preset-env"]
}
```

## 3. กำหนด Script ให้รัน babel-watch ตอนเขียนโปรเจค 

เปิดไฟล์ package.json และเพิ่มคำสั่ง script เข้าไปดังนี้ 

```json
"scripts": {
    "dev": "babel-watch index.js"
}
```

## 4. ทดสอบรันโปรเจค

