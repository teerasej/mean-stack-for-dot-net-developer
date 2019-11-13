
# HttpClient: การส่ง token ไปกับ request

## 1. สร้างตัวส่ง request เพื่อขอข้อมูล 

เปิดไฟล์ `src/app/services/user.service.ts`

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { AuthenResult } from '../models/authen-result';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private endpoint = 'http://localhost:3000';
  private token;

  constructor(private http: HttpClient) { }

  async login(username, password) {
      //...
  }

  // ส่ง request ไปหา web api เพื่อขอข้อมูล users ทั้งหมด
  async getAllUsers() {
    let users = [];

    const url = this.endpoint + '/users';
    // สร้าง header object ที่มี token แนบไปด้วย
    const requestHeaders = this.generateHeader();
    
    // ส่ง request ไป web api และรับข้อมูลกลับมาเป็น array
    const result = await <any>this.http.get(url, { headers: requestHeaders }).toPromise();
    console.log('users', result);
    users = result;

    return users;
  }

  // สร้าง method สำหรับสร้าง header object ที่จะส่งไปกับ request 
  generateHeader() {
    let headers = new HttpHeaders();
    // แนบ token ไปกับส่วนที่ชื่อ Authorization 
    // ส่วนนี้จะถูกอ่านใน Web API เพื่อเช็คว่ามีสิทธิ์เข้าถึงข้อมูลหรือเปล่า
    headers = headers.append('Accept', 'application/json').append('Authorization', 'Bearer ' + this.token);
    console.log('header', headers);
    return headers;
  }

}
```

## 2. เรียกใช้ service เพื่อดึงข้อมูลในหน้า dashboard

เอาข้อมูลที่ได้มาเก็บใน property ของ component 

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.ts`

```ts
import { Component, OnInit } from '@angular/core';
// นำ user service มาใช้งาน
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-dashboard-page',
  templateUrl: './dashboard-page.component.html',
  styleUrls: ['./dashboard-page.component.css']
})
export class DashboardPageComponent implements OnInit {
 
  // สร้าง property สำหรับนำไปใช้งานใน html
  users = []

  // ประกาศใช้งาน user service 
  constructor(private userService: UserService) {
     // เรียกคำสั่ง load user เพื่อเริ่มต้นการขอข้อมูลจาก web api
     this.loadUser();
  }

  async loadUser() {
    // เรียกใช้งาน user service และเอาผลลัพธ์ที่ได้ มาเก็บใน property
    this.users = await this.userService.getAllUsers();
  }

  ngOnInit() {
    
  }

}
```

## 3. นำ property มาวนลูปใน html

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.html`

เราจะใช้ `ngFor` directive ของ Angular ในการวนลูป property `users` ที่เป็น array ของเรากับ HTML element 

ในที่นี้ใช้ `<li>`

```html
<p>dashboard-page works!</p>
<ul *ngFor="let item of users">
    <li>{{ item.username }}</li>
</ul>
```

## 4. ทดสอบ

1. ให้แน่ใจว่า มีข้อมูลอยู่ใน database โดยการรันคำสั่ง `npm run dev-pre-db` ในโปรเจค Web API ก่อน
2. ทดสอบโดยการ login จากหน้า login page เพื่อให้ได้ token มาใช้ในหน้า dashboard page
3. ใช้ account นี้ในการทดสอบ 

```json
{
  username: "Carry",
  password: "I3nZ77WiigR"
}
```

## A. ไฟล์เต็ม `src/app/services/user.service.ts`

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
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

    try {

      const url = this.endpoint + '/users/login';

      const result = await <any>this.http.post(url, json).toPromise();

      console.log(result.token);
      this.token = result.token;
      return new AuthenResult(true, this.token);

    } catch (e) {
      console.error(e);
      return new AuthenResult(false, e.status + ' ' + e.error.message);
    }
  }

  async getAllUsers() {
    let users = [];

    const url = this.endpoint + '/users';
    const requestHeaders = this.generateHeader();

    const result = await <any>this.http.get(url, { headers: requestHeaders }).toPromise();
    console.log('users', result);
    users = result;

    return users;
  }

  generateHeader() {
    let headers = new HttpHeaders();
    headers = headers.append('Accept', 'application/json').append('Authorization', 'Bearer ' + this.token);
    console.log('header', headers);
    return headers;
  }

}
```

