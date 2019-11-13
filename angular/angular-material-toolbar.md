
# ใส่ UI ด้วย Angular Material: Toolbar

## 1. เรียกใช้งาน Toolbar

เปิดไฟล์ `src/app/app.module.ts`

แต่ละ component ใน Angular Material จะมี Module ของตัวเอง 

ถ้าต้องการใช้งาน จะต้องประกาศใช้ในไฟล์ `app.module.ts` ให้เรียบร้อย 

เช่นในที่นี้เราจะใช้ toolbar ก็ต้องเอา `MatToolbarModule` มา

```ts
import { MatToolbarModule } from "@angular/material";

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    MatToolbarModule
  ],
```

## 2. ใช้งานใน HTML 

เปิดไฟล์ `src/app/app.component.html`

และเขียนใช้งาน `<mat-tool-bar>`

```html
<mat-toolbar color="primary">
    <span>My Application</span>
</mat-toolbar>
<router-outlet></router-outlet> 
```

## 3. ทดสอบ