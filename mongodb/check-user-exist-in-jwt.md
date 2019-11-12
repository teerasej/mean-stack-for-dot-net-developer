
# JWT: เช็คข้อมูล user ใน database ก่อนปล่อย request เข้า route

เปิดไฟล์ `jwt/JWT.js`

เราจะนำเข้า `isUserExist` function มาใช้

```js
import { isUserExist } from "../models/UsersService";
```

และยังจากถอด token จาก request object ได้แล้ว เราจะเอา user ที่ถอดได้ มาเช็คกับฐานข้อมูล

```js
    res.status(401).json({ error: "Not Authorized" });
        throw new Error("Not Authorized");
    }

    // เช็ค user object ที่ได้จากการ verify() token ว่ามีข้อมูลตรงกับในระบบหรือไม่ 
    // ถ้าไม่มี ก็ response เป็น 404 กลับไปให้ client 
    if(!isUserExist(user.username, user.password)){
        res.status(404).json({ error: "Account doesn't exist" });
    }

    // ผ่าน request เข้า route
    return next();
```

## A. ไฟล์เต็ม `jwt/JWT.js`

```js
import jwt from "jsonwebtoken";
import fs from "fs";
import { isUserExist } from "../models/UsersService";

export const passphase = 'nextflow';

export function generateToken( payload = {username: 'a', password: 'b'} ) {
    // let privateKey = fs.readFileSync('./private.pem', 'utf8');
    let token = jwt.sign(
        payload, 
        passphase,
        { algorithm: 'HS256'}
    );
    return token;
}

export function isAuthenticated(req, res, next) {

    if (typeof req.headers.authorization !== "undefined") {
        
        let token = req.headers.authorization.split(" ")[1];
        // let privateKey = fs.readFileSync('./private.pem', 'utf8');
        
        jwt.verify(token, passphase, { algorithm: "HS256" }, (err, user) => {
            
            // if there has been an error...
            if (err) {  
                // shut them out!
                res.status(401).json({ error: "Not Authorized" });
                throw new Error("Not Authorized");
            }

            if(!isUserExist(user.username, user.password)){
                res.status(404).json({ error: "Account doesn't exist" });
                // throw new Error("Not Authorized");
            }


            // if the JWT is valid, allow them to hit
            // the intended endpoint
            return next();
        });
    } else {
        // No authorization header exists on the incoming
        // request, return not authorized and throw a new error 
        res.status(401).json({ error: "Not Authorized" });
        throw new Error("Not Authorized");
    }
}
```