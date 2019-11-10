
# JWT: JSON Web Token 

## 1. ติดตั้ง module 

```bash
ืnpm i jsonwebtoken 
```

หรือ

```bash
yarn add jsonwebtoken
```

## 2. สร้าง module สำหรับสร้าง token และทดสอบ

สร้างไฟล์ `jwt/JWT.js`

```js
import jwt from "jsonwebtoken";
import fs from "fs";

export const passphase = 'nextflow'; 

// function ที่ใช้สร้าง token ส่งกลับไปให้ client ของเรา
export function generateToken( payload = {username: 'a', password: 'b'} ) {
    // let privateKey = fs.readFileSync('./private.pem', 'utf8');
    let passphase = 'nextflow';
    let token = jwt.sign(
        payload, 
        passphase,
        { algorithm: 'HS256'}
    );
    return token;
} 

// function ใช้ตรวจ request ที่ส่งเข้ามาในระบบว่ามี token ตรงกับที่สร้างจาก server ของเราไหม
export function isAuthenticated(req, res, next) {

    if (typeof req.headers.authorization !== "undefined") {

        let token = req.headers.authorization.split(" ")[1];
        // let privateKey = fs.readFileSync('./private.pem', 'utf8');

        jwt.verify(token, passphase, { algorithm: "HS256" }, (err, user) => {

            // if there has been an error...
            if (err) {  
                // shut them out!
                res.status(500).json({ error: "Not Authorized" });
                throw new Error("Not Authorized");
            }
            // if the JWT is valid, allow them to hit
            // the intended endpoint
            return next();
        });
    } else {
        // No authorization header exists on the incoming
        // request, return not authorized and throw a new error 
        res.status(500).json({ error: "Not Authorized" });
        throw new Error("Not Authorized");
    }
} 
```

จากนั้นสร้างไฟล์ทดสอบ 

```js
import { generateToken, passphase } from "./JWT";
import { expect } from "chai";

describe('JWT engine', () => {

    it('should generate token, even not input any payload', () => {
        let result = generateToken();
        expect(result).to.not.equal(null);
        expect(result).to.be.a('string');
    });

    it('should generate token, with a payload', () => {
        let result = generateToken({ username: 'b', password: 'a'});
        expect(result).to.not.equal(null);
        expect(result).to.be.a('string');
        console.log('token', result);
    });

    it('passphase should be defined and is a string', () => {
        expect(passphase).to.not.equal(null);
        expect(passphase).to.be.a('string');
    });

}); 
```

## 3. ทดสอบการทำงาน

รันคำสั่งด้านล่าง เพื่อให้ jest ค้นหาและรัน test ในโปรเจคของเรา 

```bash
npm test
```

หรือ

```bash
yarn test
```