
# การสร้าง Service ไว้ใช้งาน

Service เป็น component ที่สามารถแชร์ไปใช้งานใน Component อื่นๆ ได้ จุดเด่นคือ instance ของ Service จะถูกสร้างขึ้นมาครั้งเดียว และแชร์ไปใช้ใน component ที่ import ตัว service ไปใช้งาน

เราสามารถเทียบ Service ของ Angular ได้เหมือน Model ใน MVC หรือพวก Utility class ก็ได้

## 1. รันคำสั่งสร้าง service component 

```bash
ng g service services/user
```

## 2. สร้าง Service สำหรับจัดการข้อมูล User

เสร็จแล้วเปิดไฟล์ `src/app/services/user.service.ts`

- เราจะกำหนด property ชื่อ `endpoint` เอาไว้สำหรับต่อ web api
- และ `login(username, password)` method สำหรับให้ component เรียกใช้เพื่อทำการ login 

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  endpoint = 'http://localhost:3000';

  constructor() { }

  async login(username, password) {

  }

}
```

## 3. เรียกใช้งานใน Login Page Component

เปิดไฟล์ `src/app/login-page/login-page.component.ts`

เราจะทำการ import, และเรียกใช้ `UserService` ใน method `onSubmit()` เพื่อเริ่มการ login กับ Web API

เริ่มจาก import `UserService`

```ts
import { UserService } from '../services/user.service';
```

ประกาศใน constructor

```ts
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
```

และเรียกใช้ใน `onSubmit()` method

```ts
async onSubmit(loginData) {

    console.log(loginData);
    this.loginForm.reset();

    await this.userService.login(loginData.username, loginData.password);
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

  async onSubmit(loginData) {

    console.log(loginData);
    this.loginForm.reset();

    await this.userService.login(loginData.username, loginData.password);
  }

  ngOnInit() {
  }

  login() {
    this.router.navigateByUrl('/dashboard');
  }

}
```