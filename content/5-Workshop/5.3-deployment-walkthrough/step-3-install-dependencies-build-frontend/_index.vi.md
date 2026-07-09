---
title : "Cài Dependencies & Build Frontend"
date : "2026-07-09"
weight : 3
chapter : false
pre : " <b> Step 3 </b> "
---


---

## Các Lệnh

```powershell
cd C:\Users\ADMIN\Desktop\BC\DEMO
npm install
```

---

## Kết Quả Mong Đợi

```
up to date, audited 300+ packages in 5s
```

---

## Build Frontend

```powershell
npm run build
```

---

## Build Output

```
dist/ generated successfully:
  index.html
  assets/index-xxxx.js
  assets/index-xxxx.css
```

---

## Xác Minh Build

```powershell
Test-Path dist/index.html
# → True
```

---

![Screenshot](/images/5-Workshop/step-3.png)

