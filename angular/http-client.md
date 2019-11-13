
# การใช้งาน HttpClient

## 1. setup HttpClientModule 

เปิดไฟล์ `src/app/app.module.ts`

เราจะทำการ import `HttpClientModule` และเพิ่มเข้าไปในส่วน `imports` ของ `@NgModule`

```ts
import { HttpClientModule } from "@angular/common/http";

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...,
    HttpClientModule
  ],
  ...
```

## 2. เรียกใช้งาน HttpClient ใน Service

เปิดไฟล์ `src/app/services/user.service.ts`

```ts
import { Injectable } from '@angular/core';
// import เพื่อใช้ส่ง request
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private endpoint = 'http://localhost:3000';
  // สร้างตัวแปร ไว้เก็บ token ที่ได้จากการ login
  private token;
    
  constructor(private http: HttpClient) { }

  async login(username, password) {

    // สร้าง object javascript เพื่อส่งไปกับ POST request
    const json = {
      username: username,
      password: password
    }

    // สร้าง URL route 
    const url = this.endpoint + '/users/login';

    // ใช้ httpclient.post เพื่อส่งข้อมูลไป WebAPI
    const result = await<any> this.http.post(url, json).toPromise();
    // เช็คดูข้อมูล token ผ่าน console
    console.log(result.token);
    // เก็บข้อมูลไว้ใน token property เพื่อใช้ในการส่ง request อื่นๆ 
    this.token = result.token;
  }

}
```

## 3. ทดสอบ

1. ให้แน่ใจว่า มีข้อมูลอยู่ใน database โดยการรันคำสั่ง `npm run dev-pre-db` ในโปรเจค Web API ก่อน
2. ทดสอบโดยการ login จากหน้า login page เพื่อให้ได้ token มาใช้ในหน้า dashboard page
3. ใช้ account นี้ในการทดสอบ 

```json
{
  username: "Carry",
  password: "I3nZ77WiigR"
}
```

## 4. สร้าง class ไว้สำหรับเก็บข้อมูลการ Authentication ไปใช้ในระบบ 

รันคำสั่ง 

```bash
ng g class models/authen-result
```
เสร็จแล้วเปิดไฟล์ `src/app/models/authen-result.ts`

และกำหนด constructor ที่สร้าง property `pass` และ `message`

```ts
export class AuthenResult {
    constructor(public pass:boolean, public message:string){}
}
```

## 5. ใช้ try catch ในการจับ http error และส่งผลลัพธ์ออกไปจาก login method

เปิดไฟล์ `src/app/services/user.service.ts`

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
// นำ AuthenResult มาใช้งาน
import { AuthenResult } from '../models/authen-result';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private endpoint = 'http://localhost:3000';
  private token;

  constructor(private http: HttpClient) { }

  async login(username, password) {

    const json = {
      username: username,
      password: password
    }

    // สร้าง try catch block ในกรณีที่ response จาก web api กลับมาเป็น error
    try {
      
      const url = this.endpoint + '/users/login';

      const result = await <any>this.http.post(url, json).toPromise();

      console.log(result.token);
      this.token = result.token;

      // ถ้าไม่มีปัญหาอะไร กำหนดค่า pass เป็น true และแนบ token ออกไปด้วย
      return new AuthenResult(true, this.token);

    } catch (e) {
      // ถ้าเกิด error ให้ log 
      console.error(e);

      // และส่งค่า pass ออกไปเป็น false พร้อมกับ status code และข้อความ error
      return new AuthenResult(false, e.status + ' ' + e.error.message);
    }
  }

}
```

## 6. แก้การใช้งาน Service ใน Login Page component ให้รับกับค่า AuthenResult

เปิดไฟล์ `src/app/login-page/login-page.component.ts`

```ts
const authenResult = await this.userService.login(loginData.username, loginData.password);

    if(authenResult.pass) {
      this.router.navigateByUrl('/dashboard');
    } else {
      alert('Cannot login! ' + authenResult.message);
    }
```

## A. ไฟล์เต็ม `src/app/login-page/login-page.component.ts`

```ts
import { Component, OnInit } from '@angular/core';
import { Router } from "@angular/router";
import { FormBuilder } from '@angular/forms';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-login-page',
  templateUrl: './login-page.component.html',
  styleUrls: ['./login-page.component.css']
})
export class LoginPageComponent implements OnInit {

  loginForm;

  constructor(
    private router: Router, 
    private formBuilder: FormBuilder,
    private userService: UserService
  ) { 
    this.loginForm = this.formBuilder.group({
      username: '',
      password: ''
    });
  }
  
  // กำหนดเป็น async method
  async onSubmit(loginData) {

    console.log(loginData);
    this.loginForm.reset();

    // รับค่าจากการเรียกใช้ user service
    const authenResult = await this.userService.login(loginData.username, loginData.password);

    // ถ้าค่า pass เป็น true ให้เปิดไปยังหน้า dashboard ได้
    if(authenResult.pass) {
      this.router.navigateByUrl('/dashboard');
    } else {
      // ถ้าเป็น false ให้แสดง popup แทน
      alert('Cannot login! ' + authenResult.message);
    }
  }

  ngOnInit() {
  }

  login() {
    this.router.navigateByUrl('/dashboard');
  }

}
```


