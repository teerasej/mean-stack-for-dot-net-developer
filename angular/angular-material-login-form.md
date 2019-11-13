
# ใส่ UI ด้วย Angular Material: Login Form

## 1. เรียกใช้งาน Module

เปิดไฟล์ `src/app/app.module.ts`

เราจะเรียกใช้งาน module เพิ่มเติมดังนี้

```ts
import { 
  MatToolbarModule, 
  MatFormFieldModule, 
  MatInputModule,
  MatButtonModule,
  MatGridListModule,
  MatCardModule
} from "@angular/material";

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    MatToolbarModule, 
    MatFormFieldModule, 
    MatInputModule,
    MatButtonModule,
    MatGridListModule,
    MatCardModule
  ],
```

## 2. ใช้งานใน HTML 

เปิดไฟล์ `src/app/login-page/login-page.component.html`


```html
<mat-grid-list cols="3" rowHeight="1:2">
  <mat-grid-tile></mat-grid-tile>
  <mat-grid-tile>
    <form [formGroup]="loginForm" (ngSubmit)="onSubmit(loginForm.value)" class="example-container">
      <mat-form-field appearance="standard" class="example-full-width">
        <mat-label for="username">
          Username
        </mat-label>
        <input matInput id="username" type="text" formControlName="username">
      </mat-form-field>

      <mat-form-field appearance="standard" class="example-full-width">
        <mat-label for="password">
          Password
        </mat-label>
        <input matInput id="password" type="password" formControlName="password">
      </mat-form-field>

      <button color="primary" mat-raised-button class="button" type="submit">Login</button>

    </form>
  </mat-grid-tile>
  <mat-grid-tile></mat-grid-tile>
</mat-grid-list>


<!-- <a routerLink="/dashboard">Log in</a>
<p></p>
<a (click)="login()">Login in TS</a> -->
```

## 3. เพิ่ม CSS Style

เปิดไฟล์ `src/app/login-page/login-page.component.css`

```css
.example-full-width {
    display: block;

  }

  .button {
    width: 100%;
  }
```

## 4 . ทดสอบ