
# Create Component


## 1. ใช้ generator สร้าง component ชื่อ log-in page

```bash
ng g component login-page
```

จะมีไฟล์สร้างขึ้นมาตามด้านล่าง 

- `src/app/login-page/login-page.component.css`
- `src/app/login-page/login-page.component.html`
- `src/app/login-page/login-page.component.spec.ts`
- `src/app/login-page/login-page.component.ts`

นี่คือ login page component 


## 2. นำ login-page component มาใช้งาน

ลบโค้ดในไฟล์ `client-app/src/app/app.component.html` ให้เหลือแค่ด้านล่าง 

```html

<router-outlet></router-outlet>
```

และใช้ login-page component tag ตามที่เขียนไว้ใน selector ของ `src/app/login-page/login-page.component.ts `

```ts
// src/app/login-page/login-page.component.ts 
@Component({
  selector: 'app-login-page',
  ...
})
```

จะได้เป็นแบบนี้ในไฟล์  `client-app/src/app/app.component.html`

```html
<app-login-page></app-login-page>
<router-outlet></router-outlet>
```

## 3. ทดสอบรัน 

รันทดสอบด้วยคำสั่ง 

```bash
ng serve
```