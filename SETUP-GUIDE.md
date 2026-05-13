# 🔥 วิธีตั้งค่า Firebase (ทำครั้งเดียว ~10 นาที)

User แค่กด "เข้าสู่ระบบด้วย Google" แล้วใช้งานได้เลย ไม่ต้องตั้งค่าอะไรเพิ่ม

---

## ขั้นตอนสำหรับคุณ (Developer)

### 1. สร้างโปรเจกต์ Firebase

1. ไปที่ https://console.firebase.google.com
2. กด **Add project** → ตั้งชื่อ เช่น `budget-app`
3. ปิด Google Analytics (ไม่จำเป็น) → กด **Create project**

---

### 2. เปิดใช้ Authentication

1. เมนูซ้าย → **Authentication** → **Get started**
2. แท็บ **Sign-in method** → เลือก **Google**
3. กด Toggle เปิด → ใส่ Project support email → กด **Save**

---

### 3. สร้าง Firestore Database

1. เมนูซ้าย → **Firestore Database** → **Create database**
2. เลือก **Production mode** → กด Next
3. เลือก Region ใกล้บ้าน เช่น `asia-southeast1` (Singapore) → **Enable**

---

### 4. ตั้ง Security Rules

1. Firestore → แท็บ **Rules**
2. ลบทุกอย่างแล้ววางเนื้อหาจากไฟล์ `firestore.rules` แทน
3. กด **Publish**

Security Rules นี้ทำให้:
- แต่ละ user อ่าน/เขียน/ลบได้เฉพาะข้อมูลของตัวเองเท่านั้น
- ไม่มีใครเข้าถึงข้อมูลคนอื่นได้เลย

---

### 5. เพิ่ม Web App & คัดลอก Config

1. Project Overview → กด **</>** (Web)
2. ตั้งชื่อ App เช่น `budget-web` → กด **Register app**
3. คุณจะได้ code ประมาณนี้:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "budget-app-xxxx.firebaseapp.com",
  projectId: "budget-app-xxxx",
  storageBucket: "budget-app-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

4. **Copy ทั้งหมด** → กด **Continue to console**

---

### 6. วาง Config ใน index.html

เปิดไฟล์ `index.html` ค้นหาบรรทัด:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY",
  ...
```

แทนที่ค่าทั้งหมดด้วย config ที่ copy มา แล้ว commit ขึ้น GitHub

---

### 7. เพิ่ม Authorized Domain

1. Firebase Console → **Authentication** → **Settings** → แท็บ **Authorized domains**
2. กด **Add domain** → ใส่ domain ของคุณ เช่น `username.github.io`
3. กด **Add**

> `localhost` และ `127.0.0.1` ถูกเพิ่มอัตโนมัติแล้ว

---

## ✅ เสร็จแล้ว!

Push ขึ้น GitHub Pages — user เปิดแอป → กด **เข้าสู่ระบบด้วย Google** → ใช้งานได้เลย

---

## โครงสร้างข้อมูลใน Firestore

```
users/
  {userId}/               ← Google UID ของ user แต่ละคน
    transactions/
      {txId}              ← รายการแต่ละรายการ
        id: string
        type: "income" | "expense"
        amount: number
        note: string
        cat: string
        date: "YYYY-MM-DD"
```

---

## Free Tier (Spark Plan) — เกินได้ยากมาก

| | ฟรี ต่อวัน |
|---|---|
| Reads | 50,000 ครั้ง |
| Writes | 20,000 ครั้ง |
| Deletes | 20,000 ครั้ง |
| Storage | 1 GB |
| Auth users | ไม่จำกัด |

สำหรับแอปบันทึกเงินส่วนตัว/กลุ่มเพื่อน ใช้ฟรีได้ตลอดไป

---

## Troubleshooting

**Popup ไม่ขึ้น** → ตรวจสอบว่าเบราว์เซอร์ไม่บล็อก popup

**auth/unauthorized-domain** → เพิ่ม domain ใน Authorized domains (ขั้นตอน 7)

**permission-denied** → ตรวจสอบว่า Paste Firestore Rules ถูกต้องและกด Publish แล้ว

**โปรเจกต์ยังใช้ค่า YOUR_API_KEY** → แก้ `FIREBASE_CONFIG` ในไฟล์ index.html แล้ว redeploy
