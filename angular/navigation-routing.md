
# Navigation & Routing


## 1. เพิ่ม Route path 

เปิดไฟล์​ `client-app/src/app/app-routing.module.ts`

และกำหนด route object ลงไปในตัวแปร `routes` ดังด้านล่าง

```js
const routes: Routes = [
    // redirect ไปที่ login ถ้า เข้ามาที่ '/' 
    { path: '', redirectTo: '/login', pathMatch: 'full' },
    // route /login จะแสดง LoginPageComponent ขึ้นมา
    { path: 'login', component: LoginPageComponent }
];
```

บันทึกไฟล์ และทดสอบด้วย `ng serve`

จะเห็นว่าตอนเปิดมา url address บน web browser จะไปอยู่ที่ `localhost:4200/login` และเห็นส่วน HTML ของ Login Page Component 

## 2. สร้าง Dashboard Page และเพิ่มเข้า Route 

รันคำสั่งสร้าง component ใหม่ 

```bash
ng g component dashboard-page
```

จากนั้นเปิดไฟล์ `client-app/src/app/app-routing.module.ts`

และเพิ่ม path ของ dashboard page เข้า Route 

```js
const routes: Routes = [
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  { path: 'login', component: LoginPageComponent },
  { path: 'dashboard', component: DashboardPageComponent}
];
```

## 3. ทำ link เปิดจากหน้า Login ไปหน้า Dashboard

เปิดไฟล์ `client-app/src/app/login-page/login-page.component.html`

เพื่อเพิ่มส่วนของการใช้ link เปิดไปหน้า dashboard page

```html
<p>login-page works!</p>
<a routerLink="/dashboard">Log in</a>
```

บันทึกไฟล์ และทดสอบกด link 

## 