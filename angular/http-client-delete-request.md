
# HttpClient: request เพื่อลบข้อมูล

## 1. สร้างตัวลบไฟล์ใน HTML 

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.html`

ในที่นี้เราจะใช้ `<a>` เพื่อความเรียบง่ายในการทำ

โดยเราจะใช้ Event binding อีกครั้ง เพื่อเรียกใช้ method `removeUser(item)` โดยส่งผ่าน item แต่ละตัวลงไปใน method ด้วย

```html
<p>dashboard-page works!</p>
<ul *ngFor="let item of users">
    <li>{{ item.username }} <a (click)="removeUser(item)">X</a></li>
</ul>

```

## 2. เพิ่ม method ส่ง request เพื่อการลบ user ไปยัง Web API 

เปิดไฟล์ `src/app/services/user.service.ts`

เราจะเพิ่ม method นี้ลงไป

```ts
async removeUser(userId) {

    const url = this.endpoint + '/users/' + userId;
    const requestHeaders = this.generateHeader();

    const result = await <any>this.http.delete(url,{ headers: requestHeaders }).toPromise();
    console.log('delete result', result);
    return result;
  }

```

## 3. เรียกใช้ UserService.removeUser ใน Dashboard Page Component

เปิดไฟล์ `src/app/dashboard-page/dashboard-page.component.ts`

และเพิ่ม method `removeUser` ลงไป 

```ts
async removeUser(user){
    let deletedUser = await this.userService.removeUser(user._id);

    this.users = this.users.filter((item) => {
      return item._id != deletedUser._id;
    })
}
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