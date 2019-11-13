
# ใส่ UI ด้วย Angular Material: Dashboard

## 1. กำหนด CSS Style

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.css`

```css
.container {
    padding: 30px;
} 

.example-card {
    width: 80%;
}

.example-tile {
   padding: 20px;
} 
```

## 2. สร้าง layout ใน HTML 

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.html`


```html
<div class="container">
        <h1>Users</h1>
        <mat-grid-list cols="3" rowHeight="2:1">
                <mat-grid-tile *ngFor="let item of users">
                    {{ item.username }} <a (click)="removeUser(item)">X</a>
                </mat-grid-tile>
        </mat-grid-list>
</div>
```

## 3. ทดสอบ


## 4. นำ Card และ Button มาใช้แทน 

```html
<div class="container">
        <h1>Users</h1>
        <mat-grid-list cols="3" rowHeight="2:1">
                <mat-grid-tile *ngFor="let item of users" class="example-tile">
                        <mat-card class="example-card">
                                <mat-card-content>
                                        <h2>{{ item.username }} </h2>
                                </mat-card-content>
                                <mat-card-actions>
                                        <button mat-raised-button (click)="removeUser(item)">Remove</button>
                                </mat-card-actions>
                        </mat-card>
                </mat-grid-tile>
        </mat-grid-list>
</div>
```

## 5. ทดสอบ