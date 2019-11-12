
# Navigation & Routing

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

จะเห็นว่าตอนเปิดมา url address บน web browser จะไปอยู่ที่ `localhost:4200/login`