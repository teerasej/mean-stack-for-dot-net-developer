
# การใช้งาน MongoDB ใน Docker

## Slide Docker for Beginner

- [Download](https://www.dropbox.com/s/cqu75rgc50pcle2/Docker-beginner.pdf?dl=0)

## ดาวน์โหลด Docker Image และรัน Docker Container

รันคำสั่ง download Docker Image ของ Mongo DB

```bash
docker pull mongo:4.0.4
```

เสร็จแล้วให้รันคำสั่งด้านล่าง เพื่อสร้าง docker container แบบ background และ map port เพื่อใช้ในการทำงานของ Web API ของเรา

```bash
docker run -d -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4
```
