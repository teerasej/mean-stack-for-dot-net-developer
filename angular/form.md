
# การทำงานกับ Form

## 1. ติดตั้ง module 

การใช้งาน Form จะเป็นระบบที่ต้องประกาศใช้ และไฟล์ `app.module.ts` จะเป็นไฟล์ที่เราใช้งาน 

เปิดไฟล์ `src/app/app.module.ts`

เริ่มจากการ import module ที่ต้องใช้งาน

```ts
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
```

และนำ module มาใส่ในส่วน `imports` ของ `@NgModule`

```ts
@NgModule({
  declarations: [
   ...
  ],
  imports: [
    ...,
    FormsModule,
    ReactiveFormsModule
  ],
  ...
```

## 2. ประกาศ property ของ Form ใน Component ที่ต้องการ

เปิดไฟล์ `src/app/login-page/login-page.component.ts`

เริ่มจาก import `FormBuilder` มาจาก module 

```ts
import { FormBuilder } from '@angular/forms';
```

และเขียนการสร้าง form object ใช้งานใน class `LoginPageComponent` 

```ts
export class LoginPageComponent implements OnInit {

  // เก็บตัวอ้างอิง form ไว้เชื่อมกับ form ใน HTML 
  loginForm;

  constructor(private router: Router, private formBuilder: FormBuilder) { 

    // สร้าง group ของ form เก็บไว้ โดยกำหนดค่า username และ password สำหรับดึงค่าจาก input มาใช้งาน
    this.loginForm = this.formBuilder.group({
      username: '',
      password: ''
    });
  }

  // สร้าง method สำหรับตอนที่กด submit form
  onSubmit(loginData) {
    console.log(loginData);

    // เคลียร์ข้อมูลใน form
    this.loginForm.reset();
  }
```

## 3. binding HTML form กับ loginForm property และ method onSubmit

เปิดไฟล์ `src/app/login-page/login-page.component.html`

เราจะสร้างส่วนของ web form ด้วย HTML 

```html
<!-- กำหนดค่า formGroup เป็น loginForm property -->
<!-- และ event ngSubmit กับ onSubmit method โดยส่งค่าของ form เข้าไปด้วย -->
<form [formGroup]="loginForm" (ngSubmit)="onSubmit(loginForm.value)">

    <div>
      <label for="username">
        Username
      </label>
      <input id="username" type="text" formControlName="username">
    </div>

    <div>
      <label for="password">
        Password
      </label>
      <input id="password" type="password" formControlName="password">
    </div>

    <button class="button" type="submit">Login</button>

  </form>

```

## 4. ทดสอบการทำงาน