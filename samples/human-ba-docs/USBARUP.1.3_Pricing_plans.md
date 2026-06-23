# USBARUP.1.3_Pricing_plans

---

**User story:** USBARUP.1.3

**Conversation:** DB mechanism - Define Pricing plans by combining Tier and State rules.

**Card:**
- 

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:


---

## Requirements

- **BR1**
  - - Dev xây cơ chế định nghĩa pricing plans dựa theo tier 
  => Xây dựng tier (đánh số, giá cả, quyền lợi) trước tiên, rồi mới gán cho các plans
  - *DEV: True | Test: True*

- **BR2**
  - - Bind logic các pricing plans dựa theo cả state
  + không được phép có 2 plan cùng tier cùng state = active (có thể cùng tier nhưng 1 plan old 1 plan active)
  + logic subscription management bình thường vẫn work cho cả old và active
  => nếu có 2 plan cùng tier, 1 plan state active và 1 plan state future, khi chuyển plan future thành active (publish plan đó cho all domains), chắc chắn plan đang active phải chuyển thành old hoặc state khác
  - *DEV: True | Test: True*
