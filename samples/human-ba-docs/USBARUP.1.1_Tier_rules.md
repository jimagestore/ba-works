# USBARUP.1.1_Tier_rules

---

**User story:** USBARUP.1.1

**Conversation:** DB mechanism - Add Tier rules to clarify details of each pricing plans (benefits, price,...).

**Card:**
- 

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:
1. Pricing page -> Subscribe/Unsubscribe/Change plans

---

## Requirements

- **BR1**
  - - Actual:
  Định nghĩa các pricing plan/gói theo plan code (VD: Standard, Pro, Plus)
  Phân biệt quyền lợi gói theo plan code này
  ...
  
  => Expected:
  Dev add thêm logic tier vào code/DB để phân biệt các gói theo tier
  - *DEV: True | Test: True*

- **BR2**
  - - Mỗi Tier tương ứng với
  + Giá
  + Quyền lợi
  của một gói
  
  - Đánh số cho các tier từ 0 -> infinity
  + Tier 0 luôn dành cho Free plan
  + Các tier khác lần lượt được đánh cho từng gói trả tiền
  + Sản phầm không có Free plan thì gói thấp nhất là tier 1
  + Không nhất thiết tier nhỏ hơn có giá tiền nhỏ hơn
  - *DEV: True | Test: True*

- **BR3**
  - - Với app Barcode, đang có 3 paid plans trên Live
  => Đánh số tier luôn cho Free plan và 3 paid plan này
  Free: tier 0
  Standard: tier 1
  Plus: tier 2
  Pro: tier 3
  - *DEV: True | Test: True*

- **BR4**
  - - Tương ứng việc đánh số, mỗi tier sẽ được assign luôn price và quyền lợi plan, việc chặn theo plan 
  VD: 
  tier 2 sẽ bao gồm các quyền lợi và giá cả của Plus plan
  $27.99/month
  30000 monthly labels (expire at the end of period)
  All features in Free Plan
  Priority Support Channel
  - *DEV: True | Test: True*

- **BR5**
  - - Chuyển việc chặn quyền lợi app, switch quyền lợi app, charge tiền,.... dựa vào current tier của store đó
  + Khi ở Free plan nhận tier 0, sau đó subscribe paid plan, thì store này nhận tier tương ứng (VD: subscribe lên Pro thì nhận tier 3)
  + Khi ở Paid plan, sau đó unsubcribe về Free plan, sẽ nhận tương ứng tier 0
  + Khi ở Paid plan, sau đó change plan sẽ nhận tier tương ứng của plan mới (VD: đang ở Standard tier 1, change thành Plus sẽ nhận tier 2)
  - *DEV: True | Test: True*

- **BR6**
  - - Tier của Monthly và Yearly plans
