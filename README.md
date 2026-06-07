# 📋 INV Audit – Dashboard & Checklist

ระบบติดตามการตรวจนับสินค้า (Inventory) ของ Makro ทั้ง 4 Regional พร้อม **Firebase Realtime Sync v2** ซึ่งทำให้:

- ✅ **ทีมหลายคนใช้งานพร้อมกันได้** — แก้ที่สาขาไหน Dashboard ของทุกคนเห็น**ภายใน 1 วินาที** (Realtime จริง)
- ✅ **Per-store Instant Listener** — เครื่องที่ดูสาขาเดียวกันจะอัปเดตทันทีโดยไม่ต้อง refresh
- ✅ **Write Confirmation** — จุด 💚 เขียวกระพริบ = บันทึกสำเร็จ / 🔵 น้ำเงินกระพริบ = ได้รับข้อมูลใหม่
- ✅ **ใช้ได้ทุกอุปกรณ์** — iOS / Android / PC / Tablet เปิดเว็บเดียวกัน เห็นข้อมูลเดียวกัน
- ✅ **Offline ก็ทำงานได้** — ใช้ localStorage เป็น cache, พอ online ส่งขึ้น Firebase ให้อัตโนมัติ
- ✅ **ไม่ต้องล็อกอิน** — เปิดเว็บ → เลือกสาขา → ใช้ได้เลย

---

## 🚀 ติดตั้ง 4 ขั้นตอน (10 นาที)

### 📌 ขั้นที่ 1: ตั้งค่า Firebase Realtime Database

> ✅ ถ้าคุณมี Firebase project `inv-checklist` แล้ว ข้ามไปขั้นนี้ได้เลย ที่ขาดคือ Security Rules

1. เข้า [Firebase Console](https://console.firebase.google.com/) → เลือก project **inv-checklist**
2. เมนูซ้าย → **Build** → **Realtime Database**
3. กดแท็บ **Rules** ด้านบน
4. **คัดลอกเนื้อหาในไฟล์ `database.rules.json`** ในโปรเจ็คนี้ ไปวางทับ → กด **Publish**

   ```json
   {
     "rules": {
       "checklists": {
         ".read": true,
         ".write": true
       }
     }
   }
   ```

   > ⚠️ Rules นี้เปิดให้ทุกคนเข้าถึงได้ (ใช้ภายในทีม) ถ้าต้องการ Auth ภายหลังให้ทักผมเพิ่มได้ครับ

5. ตรวจสอบว่า Database URL ตรงกับในไฟล์ `index.html`:
   ```
   https://inv-checklist-default-rtdb.asia-southeast1.firebasedatabase.app
   ```

---

### 📌 ขั้นที่ 2: สร้าง GitHub Repository

1. เข้า [github.com](https://github.com/) → กด **+** มุมขวาบน → **New repository**
2. ตั้งค่า:
   - **Repository name**: `inv-checklist` (หรือชื่ออื่นได้)
   - เลือก **Public** (จำเป็นสำหรับ GitHub Pages ฟรี)
   - **ติ๊ก** ✅ Add a README file
3. กด **Create repository**

---

### 📌 ขั้นที่ 3: อัปโหลดไฟล์ขึ้น GitHub

**วิธีง่ายที่สุด (ลากไฟล์ใส่เบราว์เซอร์):**

1. ใน repo ที่สร้าง → กดปุ่ม **Add file** → **Upload files**
2. **ลากไฟล์เหล่านี้ใส่:**
   - `index.html`  ← **ไฟล์หลัก**
   - `database.rules.json`  ← (ไม่จำเป็นต้องอัปโหลด แต่เก็บไว้เป็น reference)
   - `README.md`  ← (อันนี้)
   - `.gitignore`  ← (อันนี้)
3. ใส่ commit message: `Initial setup`
4. กด **Commit changes**

---

### 📌 ขั้นที่ 4: เปิด GitHub Pages

1. ใน repo → กดแท็บ **Settings** บนสุด
2. เมนูซ้าย → **Pages**
3. ตั้งค่า:
   - **Source**: เลือก `Deploy from a branch`
   - **Branch**: เลือก `main` → folder `/ (root)` → กด **Save**
4. รอประมาณ **1-2 นาที**, รีเฟรชหน้านี้ จะเห็น URL คล้าย:

   ```
   https://YOUR-USERNAME.github.io/inv-checklist/
   ```

5. **คลิกที่ URL → เปิดได้เลย! 🎉**

---

## ✨ ตรวจสอบว่าใช้งานได้

เปิดเว็บแล้ว ดูที่ **มุมขวาบน** ของหน้าจอ:

| สถานะ | สี | ความหมาย |
|------|----|---------|
| 🟡 กำลังเชื่อมต่อ... | เหลือง | กำลังเชื่อมต่อ Firebase (ไม่เกิน 3 วินาที) |
| 🟢 ออนไลน์ | เขียว | Sync พร้อมใช้งาน — ทุกการแก้ไขจะถูกบันทึก realtime |
| 🔴 ออฟไลน์ | แดง | ไม่มีอินเทอร์เน็ต — ยังใช้งานได้ พอ online จะ sync ให้อัตโนมัติ |

**ทดสอบ Sync:**
1. เปิดเว็บใน 2 browser (เช่น Chrome + iPhone)
2. เลือกสาขาเดียวกัน
3. กดสถานะ ✓ เรียบร้อย ใน browser หนึ่ง
4. ดู browser อีกตัว → **ควรเห็นการอัปเดตภายใน 1-2 วินาที** ✅

---

## 🔧 การใช้งาน

### หน้า Dashboard 📊
- ดูภาพรวม 4 Regional, สาขาที่เสร็จแล้ว/กำลังดำเนินการ/ยังไม่เริ่ม
- คลิก Region card เพื่อดูรายละเอียดสาขาในนั้น

### หน้า Checklist ✅
- เลือกสาขาจาก Store Picker (จะแสดงเป็นกลุ่ม Regional พร้อมสี/สถานะ)
- กรอก Checklist 4 Sheet:
  - **Sheet 0**: ก่อนนับ (Preparation) — 21 รายการ
  - **Sheet 1**: Pre-Count (4 คืน) — แต่ละคืน 11 รายการ
  - **Sheet 2**: Regular Count
  - **Sheet 3**: Recheck & Final Report
- กดเปลี่ยนสถานะ → บันทึกอัตโนมัติทันที (ทั้ง localStorage และ Firebase)

### Export 📤
- ปุ่ม **Export Excel** ในหน้า Checklist → ดาวน์โหลด CSV ของสาขาที่เลือก
- ปุ่ม **Export Excel** ในหน้า Regional Detail → ดาวน์โหลดรายงาน Region
- ปุ่ม **🖨️ Print** → พิมพ์เอกสาร Checklist พร้อม header

---

## 🛠️ Trouble Shooting

### 🔥 ข้อมูลไม่ sync ข้ามเครื่อง / กดบันทึกแล้วเครื่องอื่นไม่เห็น

นี่คือปัญหาที่เจอบ่อยที่สุด — **99% เกิดจาก Firebase Rules ยังไม่ Publish หรือยังเป็นโหมด Test/Locked**

**วิธีตรวจสอบทีละขั้น:**

#### ✅ Step 1: เปิด Console (F12) ดู Error
- กด **F12** ในเบราว์เซอร์ → แท็บ **Console**
- ถ้าเห็นข้อความสีแดง `[FB write FAIL]` หรือ `permission_denied` → **เป็นปัญหา Rules แน่นอน**
- ในหน้าเดียวกัน พิมพ์ `fbDebug()` แล้วกด Enter จะเห็น:
  ```
  { fbReady: true, writeOk: 0, writeFail: 12, ... }
  ```
  ถ้า **writeFail > 0** = เขียนไม่ได้ → ปัญหา Rules
  ถ้า **writeOk เพิ่มขึ้นทุกครั้งที่กด** = เขียนสำเร็จ ✓

#### ✅ Step 2: ตรวจ Rules ใน Firebase Console
1. เข้า https://console.firebase.google.com/ → project **inv-checklist**
2. **Build → Realtime Database → Rules**
3. **ต้องเห็น Rules แบบนี้** (ถ้ายังเป็น `".read": false` หรือ `"auth != null"` คือยัง block อยู่):
   ```json
   {
     "rules": {
       "checklists": {
         ".read": true,
         ".write": true
       }
     }
   }
   ```
4. กด **Publish** สีฟ้ามุมขวาบน
5. รอ 5 วินาที → กด F5 ที่หน้าเว็บใหม่ → ทดสอบใหม่

#### ✅ Step 3: ทดสอบ Real-time
- เปิดเว็บใน **2 เบราว์เซอร์/อุปกรณ์**
- เลือก **สาขาเดียวกัน** ทั้ง 2 เครื่อง
- เครื่องที่ 1 กดสถานะ → จุดเขียวกระพริบ 💚 (เขียนสำเร็จ)
- เครื่องที่ 2 ควรเห็นจุด **น้ำเงินกระพริบ** 🔵 และข้อมูลอัปเดต **ภายใน 1 วินาที**
- ถ้าจุดน้ำเงินไม่กระพริบ = Listener ไม่ได้รับ event → ปัญหา Rules อ่านไม่ได้

#### ✅ Step 4: ดู Firebase Console ว่ามีข้อมูลจริง
1. Firebase Console → Realtime Database → แท็บ **Data**
2. ควรเห็น node `checklists/{ชื่อสาขา}/...` มีข้อมูลเข้ามาเรื่อยๆ ตอนกดบันทึก
3. ถ้า **ไม่มี node `checklists` เลย** = ไม่มี Write เลย → ปัญหา Rules

### 🎨 ความหมายของจุดสี (สำหรับ Debug)

| สี | ความหมาย |
|----|---------|
| 🟢 เขียวคงที่ | เชื่อมต่อ Firebase สำเร็จ |
| 💚 เขียวกระพริบสั้น | **Write สำเร็จ** — ข้อมูลถูกส่งขึ้น Firebase แล้ว |
| 🔵 น้ำเงินกระพริบ | **Incoming update** — รับข้อมูลใหม่จากเครื่องอื่น |
| 🟡 เหลืองกระพริบ | กำลังเชื่อมต่อ |
| 🔴 แดงคงที่ + ⚠️ | **Write/Read ล้มเหลว** → ปัญหา Rules แน่ |

### ❌ Sync Badge แสดง "ออฟไลน์" ตลอด

**สาเหตุ:** ไม่มี Internet, Firebase URL ผิด, หรือ JavaScript ถูก block

**แก้:**
1. ตรวจ Internet ก่อน
2. ตรวจสอบ Console (F12) ว่ามี error อะไร
3. ตรวจว่า URL ใน `index.html` ตรงกับใน Firebase:
   ```
   https://inv-checklist-default-rtdb.asia-southeast1.firebasedatabase.app
   ```

### ❌ GitHub Pages ขึ้น 404

**แก้:**
- รอเพิ่ม 5 นาที (บางครั้ง GitHub ใช้เวลา deploy)
- ตรวจ Settings → Pages → ต้องเห็นข้อความเขียว "Your site is live at ..."
- แน่ใจว่าชื่อไฟล์เป็น `index.html` ตัวพิมพ์เล็กทั้งหมด (case-sensitive)

### ❌ Console เห็น "Permission denied" ตอน Write

**สาเหตุ:** Rules block การเขียน

**แก้:** ทำตาม **Step 2** ด้านบน — Publish Rules ใหม่

---

## 📂 โครงสร้างไฟล์ในโปรเจ็ค

```
inv-checklist/
├── index.html              ← ไฟล์หลัก (ทุกอย่างอยู่ในนี้ — HTML/CSS/JS/Firebase)
├── database.rules.json     ← Security Rules สำหรับ Firebase
├── README.md               ← คู่มือนี้
└── .gitignore              ← ไฟล์ที่ไม่ขึ้น Git
```

---

## 🔒 ความปลอดภัย (สำหรับใช้ Production)

ปัจจุบันใช้ Rules แบบเปิด `read/write = true` เหมาะกับการใช้ภายในทีม

**ถ้าต้องการเข้มงวดมากขึ้น** (Optional):

### Option A: ใส่รหัสผ่านชุดเดียวสำหรับทีม
- ใช้ Firebase Authentication → Email/Password
- เปลี่ยน Rules เป็น `".read": "auth != null"`

### Option B: ล็อก Domain ที่อนุญาต
- Firebase Console → Project Settings → Authentication → Authorized domains

ถ้าต้องการ implement ทักผมเพิ่มได้ครับ 🙋

---

## 🗄️ โครงสร้างข้อมูลใน Firebase

```
inv-checklist/
└── checklists/
    └── ST11 - พิษณุโลก/
        ├── 0_S1/                 ← Sheet 0, item S1
        │   ├── status: "done"
        │   └── note: "..."
        ├── 1_P-S1_n1/            ← Sheet 1 (Pre-count), item P-S1, คืนที่ 1
        │   ├── status: "pending"
        │   └── note: "..."
        ├── 1_P-S1_n2/            ← คืนที่ 2
        ├── 2_RC-C1/              ← Sheet 2 Regular Count
        └── 3_RC-F1/              ← Sheet 3 Final Report
```

- ทุกครั้งที่กดเปลี่ยนสถานะ → Hook localStorage → ส่งขึ้น Firebase ที่ path นี้
- Firebase Listener ฟัง `checklists/` ทั้งก้อน → ถ้ามีการเปลี่ยนแปลง → sync ลง localStorage → re-render UI

---

## 📞 ติดต่อ / ปัญหาเพิ่มเติม

ถ้ามีปัญหาหรืออยากเพิ่ม feature ทักผมได้เลยครับ — ระบุ:
1. **อาการ**: เช่น "Sync ไม่ทำงาน"
2. **Browser/Device**: เช่น "Chrome บน Windows"
3. **Console Error** (กด F12 → Console tab → คัดลอก error สีแดง)

---

**Project**: CP AXTRA — INV Audit  
**Stack**: Vanilla JS + Firebase Realtime DB + GitHub Pages  
**Region**: asia-southeast1 (Singapore)
