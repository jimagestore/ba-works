# USBARUP.2.3_Other_plan_cards

---

**User story:** USBARUP.2.3

**Conversation:** UI logics - Other plan cards when applying new DB mechansism.

**Design:** https://www.figma.com/design/nOJoqMlFuZtUxfAQVSzpbG/SBM---UI-Mockups?node-id=4375-155722&m=dev 

**Card:**
- 1. Current tier = 0 (store đang ở Free hoặc current plan ở Interval còn lại - yearly)
- 2. Current tier != 0 với state = old, active tier ngang bằng có giá THẤP hơn

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:
1. SBM app -> Pricings page -> Interval of/ not having current plan

---

## Requirements

- **BR1**
  - - Sau khi check tier + state của current plan để detect việc hiển thị của current plan card, dev detect điều kiện hiện plan cards khác tùy vào thông tin của current plan
  => Phân chia luồng hiển thị theo interval: Monthly và Yearly
  => Detect current tier của từng Interval để define logic hiển thị cho các plan card còn lại trong Interval đó
  - *DEV: True | Test: True*

- **BR2**
  - - Nếu current tier = 0:
  + Store đang ở Free plan (1)
  + HOẶC current plan của store ở Interval khác (VD đang mở tab yearly, nhưng current plan là monthly) (2)
  
  => Tất cả các active plan card có CTA hiện label "Subscribe now"
  => Giữ luồng Subscribe khi user click vào bất kỳ nút Subscribe nào của Interval này
  - *DEV: True | Test: True*

- **BR3.1**
  - - Nếu current tier != 0 và current plan nằm ở interval (tab) hiện tại (VD đang mở tab monthly, current plan cũng là monthly):
  + Plan card hiện tại, hiển thị nút "Unsubscribe"
  - *DEV: True | Test: True*

- **BR3.2**
  - - Các active plan card tier cao hơn: CTA hiện label “Upgrade”
  - Với các active plan card tier thấp hơn:
  + tier != 0: CTA hiện label “Downgrade”, chuyển thành dạng default/secondary
  + tier = 0 (free): Không hiển thị action gì
  - *DEV: True | Test: True*

- **BR3.3**
  - - Với các active plan card tier ngang bằng:
  + Nếu giá của plan đó THẤP HƠN giá của current plan: 
    *  CTA hiện label “Approve new price”
    * Hiện thêm giá của current plan với gạch ngang bên cạnh (mang ý nghĩa old price)
  => current plan card ở state = old và ẩn trên UI
  (work trong trường hợp MS muốn giảm giá 1 plan)
  
  + Nếu giá của plan đó CAO HƠN hoặc BẰNG giá của current plan: CTA hiện label “Subscribe” tại active plan card này
  => current plan card ở state = old và vẫn hiện trên UI
  (work trong trường hợp MS muốn tăng giá 1 plan)
  - *DEV: True | Test: True*

- **BR4**
  - - Nếu current plan được gán state = old (yearly hoặc monthly)
  + Plan card cùng tier ở Interval còn lại cũng được gán state = old (refers to USBARUP.2.1)
  => Hiển thị các plan có state = old tùy theo logic DB làm ở US trước đó (USBARUP.2.1)
       Là current plan: Hiển thị flow approve new price...
       Không là current plan: Ẩn khỏi UI
  - *DEV: True | Test: True*

- **BR5**
  - - Với plan card gán state = others - future/draft/... (yearly hoặc monthly)
  + Tương ứng plan card cùng tier ở Interval còn lại cũng được gán state = future
  + Khi chuyển state = active, đồng thời update rules state = active cho cả 2 plan cards ở 2 Interval => Hiển thị 2 plan card ở 2 Interval
  - *DEV: True | Test: True*
