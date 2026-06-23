# USBARUP.1.2_State_rules

---

**User story:** USBARUP.1.2

**Conversation:** DB mechanism - Add State rules to organize pricing plans based on development progress.

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
  - - Actual:
  Chưa có trường thông tin nào define logics ẩn, hiện, UI của các pricings plans trên app SBM
  Khi có nhu cầu thêm plan mới, thay đổi giá,... cần phải nhờ dev sửa DB hoặc set biến môi trường 
  
  => Expected:
  Dev thêm trường và logics state vào code/DB để phân biệt tình trạng hoạt động của từng pricing plans trên app
  - *DEV: True | Test: True*

- **BR2**
  - - Sẽ bao gồm các state như sau:
  + old: plan cũ, không nhận subscribe mới, nhưng những user đang dùng vẫn được tiếp tục dùng
  + current/active: plan đang hoạt động hiện tại, có nhận subscribe mới
  + draft: gói chưa hoạt động, đang dev,…
  + future: gói sẽ active ở tương lai, đã done code, done logic nhưng chưa hoạt động
  - *DEV: True | Test: True*

- **BR3**
  - - Với app SBM, đang có Free plan và 2 Paid plans ở trên Live
  => Gán cho cả 3 plans với state = active
  - *DEV: True | Test: True*

- **BR4**
  - - Dev xây cơ chế khi có logics xóa/ẩn plans hoặc thay đổi giá gói
  => Các plans bị triggered này sẽ bị chuyển state về old
  - *DEV: True | Test: True*

- **BR5**
  - - Dev xây cơ chế khi có logics add thêm plans mới
  => Các plans này sẽ được gán state future (khi đang developing hoặc chưa publish cho all domains)
  => Sau khi plans mới được release cho all domains, sẽ chuyển thành active
  - *DEV: True | Test: True*

- **BR6**
  - - Khi bind với logic UI UX của app, sẽ chỉ phân luồng pricing plans theo 3 state chính
  + old
  + active
  + others (bao gồm toàn bộ các state != old AND active)
  - *DEV: True | Test: True*

- **BR7**
  - - Các plans có state future sẽ bind với logic subsciption management hiện tại, cụ thể việc test khi add plans mới vào app
  => Cần work match với luồng setup en_var để chỉ hiện tại các store để test, để publish lên all domains,...
  - *DEV: True | Test: True*

- **BR8**
  - - Logic phân theo state sẽ work trong từng Interval (Yearly pricing hoặc Monthly pricing)
  => Với app SBM, các Paid plan ở Intervel Yearly không có state, mà được tính toán theo plan tương ứng bên Interval Monthly
  - *Test: True*
