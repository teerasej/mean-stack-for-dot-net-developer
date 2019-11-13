
# เก็บข้อมูลลง Client ด้วย Local Storage

JavaScript รุ่นใหม่ มี localStorage ทำหน้าที่คล้ายๆ cookie หรือ session ใน server โดยเก็บข้อมูลง่ายๆ ไว้ในฝั่ง client 

ในที่นี้เราจะเก็บ token ไว้ข้างในกัน

## 1. สร้าง service จัดการ token 

รันคำสั่ง 

```bash
ng g service services/token
```

## 2. เขียนให้ service เก็บ token ไว้ใน local storage 

เปิดไฟล์ `src/app/services/token.service.ts`

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TokenService {

  constructor() { }

  set token(newToken){
    localStorage.setItem('token', newToken);
  }

  get token() {
    return localStorage.getItem('token');
  }

  removeToken() {
    localStorage.removeItem('token');
  }
}
```

## 3. ให้ UserService เก็บ token ไว้ใน local storage แทน

เปิดไฟล์ `src/app/services/user.service.ts`

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { AuthenResult } from '../models/authen-result';
// นำ token service มาใช้งาน
import { TokenService } from './token.service';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  
  private endpoint = 'http://localhost:3000';
  

  constructor(private http: HttpClient, private tokenService: TokenService) { }

  async login(username, password) {

    const json = {
      username: username,
      password: password
    }

    try {

      const url = this.endpoint + '/users/login';

      const result = await <any>this.http.post(url, json).toPromise();

      console.log(result.token);
      // เก็บ token ลง local storage ผ่าน token service
      this.tokenService.token = result.token;
      return new AuthenResult(true, this.tokenService.token);

    } catch (e) {
      console.error(e);
      return new AuthenResult(false, e.status + ' ' + e.error.message);
    }
  }

  // ...

  generateHeader() {
    let headers = new HttpHeaders();
    // ดึง token มาจาก token service แทน
    headers = headers.append('Accept', 'application/json').append('Authorization', 'Bearer ' + this.tokenService.token);
    console.log('header', headers);
    return headers;
  }

  //...

}
```

