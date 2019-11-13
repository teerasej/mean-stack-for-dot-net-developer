
# Using Directive with Routing

Angular สร้างสัญลักษณ์ต่างๆ เพื่อเชื่อมการทำงานระหว่าง TypeScript (พวก Component ต่างๆ) กับ HTML 

สัญลักษณ์พวกนี้ เรียกว่า **Directive** ครับ

## 1. สร้าง link ในหน้า Login page สำหรับเรียกใช้ method ใน Component

เปิดไฟล์ `src/app/login-page/login-page.component.html`

และเพิ่ม link ลงไปใน HTML 

```html
<p>login-page works!</p>
<a routerLink="/dashboard">Log in</a>
<p></p>
<a (click)="login()">Login in TS</a>
```

สังเกตว่า จะมี `(click)="login()"` อยู่ใน `<a>`  ส่วนนี้เรียกว่า event binding ใช้จับคู่ event กับ method ในฝั่ง typescript

## 2. สร้าง method สำหรับ event binding 

เปิดไฟล์ `src/app/login-page/login-page.component.ts`

เริ่มจาก import class ชื่อ Router เข้ามาใช้งาน 

```ts
import { Router } from "@angular/router";
```

จากนั้นใน Class เราจะประกาศใช้ผ่าน constructor

```ts
export class LoginPageComponent implements OnInit {

  constructor(private router: Router) { }
```

และสร้าง method ใน class เพื่อทำงานกับการกด link 

```ts
 login() {
     // ใช้ router เปิดไป url ที่ต้องการ ในที่นี้คือ url เดียวกับที่กำหนดใน routing
    this.router.navigateByUrl('/dashboard');
  }
```

## A. ไฟล์เต็ม `src/app/login-page/login-page.component.ts`

```ts
import { Component, OnInit } from '@angular/core';
import { Router } from "@angular/router";
@Component({
  selector: 'app-login-page',
  templateUrl: './login-page.component.html',
  styleUrls: ['./login-page.component.css']
})
export class LoginPageComponent implements OnInit {

  constructor(private router: Router) { }

  ngOnInit() {
  }

  login() {
    this.router.navigateByUrl('/dashboard');
  }

}
```
