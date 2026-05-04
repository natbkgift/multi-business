# Coupon Generator Logic | ตรรกะการสร้าง Coupon

## วิธีการสร้าง Coupon Code

### Format
```
SAUNA-[3 ตัวอักษรสุ่ม][3 ตัวเลขสุ่ม]
ตัวอย่าง: SAUNA-ABC123, SAUNA-XYZ789
```

### n8n Code Node — Generate Coupon
```javascript
function generateCouponCode() {
  const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ'; // ตัดตัวอักษรที่สับสน เช่น I, O
  const nums = '0123456789';
  
  let letters = '';
  for (let i = 0; i < 3; i++) {
    letters += chars.charAt(Math.floor(Math.random() * chars.length));
  }
  
  let numbers = '';
  for (let i = 0; i < 3; i++) {
    numbers += nums.charAt(Math.floor(Math.random() * nums.length));
  }
  
  return `SAUNA-${letters}${numbers}`;
}

// ตรวจสอบว่า code ไม่ซ้ำ (ดึงจาก Google Sheets)
// ถ้าซ้ำ generate ใหม่
const couponCode = generateCouponCode();
return [{ json: { couponCode, createdAt: new Date().toISOString() } }];
```

---

## ประเภท Coupon

| ประเภท | Discount | เงื่อนไข | อายุ |
|--------|---------|---------|------|
| Welcome (ครั้งแรก) | 10% | First visit only | 30 วัน |
| Referral | 150 ฿ | เมื่อแนะนำเพื่อน | 60 วัน |
| Birthday | Free upgrade | เดือนเกิด | 1 เดือน |
| Repeat Visit | 5% | ครั้งที่ 3 ขึ้นไป | 90 วัน |
| Campaign | กำหนดเอง | ตาม campaign | กำหนดเอง |

---

## Google Sheets - Coupons Sheet Structure

| Column | Field | Type |
|--------|-------|------|
| A | Coupon Code | Text (Unique) |
| B | Type | Dropdown |
| C | Discount Value | Number |
| D | Discount Type | % or ฿ |
| E | Issued To (LINE ID) | Text |
| F | Issued Date | Date |
| G | Expiry Date | Date |
| H | Status | Used / Unused / Expired |
| I | Redemption Date | Date |
| J | Booking ID Used | Text |
| K | Notes | Text |

---

## Coupon Validation Logic

ก่อนใช้ coupon ระบบตรวจ:
1. ✅ Coupon code ถูกต้อง (มีอยู่ใน Coupons Sheet)
2. ✅ Status = Unused
3. ✅ ยังไม่หมดอายุ (Expiry Date > วันนี้)
4. ✅ LINE ID ของผู้ใช้ตรงกับ Issued To (ถ้าเป็น personal coupon)
5. ✅ ไม่ใช้ซ้ำ (Status ≠ Used)

หากผ่านทุกข้อ → อัปเดต Status = Used + Redemption Date = วันนี้
