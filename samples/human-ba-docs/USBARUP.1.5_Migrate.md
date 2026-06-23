# USBARUP.1.5_Migrate

---

**User story:** USBARUP.1.5

**Conversation:** DB mechanism - Migrate the pricing blocking from old type (by Plan code) into new one (by Tier + State).

**Card:**
- 1. Current plan = Std with price = 8.49$, state = old

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:


---

## Requirements

- **BR1**
  - - Tại USBARUP1.1, mới define tier cho các plan ứng với current price ở trên Live
  + Standard: 7.99$
  + Plus: 18$
  + Pro: 27.99$
  
  => Expected: 
  Dev migrate cả các price từ trước tới nay của từng plan
  + Add thêm row data cho những price từ ngày xưa này 
  + Hứng những store vẫn đang dùng price cũ này, vẫn giữ current tier tương ứng và nhận state = old
  (chi tiết như bảng phân loại bên cạnh)                                                                                                                                                                                                            
  - *DEV: True | Test: True*

- **BR2**
  - - Với các store vẫn ở price ngày trước
  + Std: 10$
  + Std: 8.99$
  + Pro: 30$ 
  => Ghi nhận current tier của họ đang có state = old
  => Logic ẩn hiển current plan card tương tự các US nêu trên
  => Giữa các plan có cùng giá trị tier (VD: cùng tier = 2 hoặc tier = 3):
  + Giá current NHỎ HƠN hoặc BẰNG active tier ngang bằng (Std - 7.49 hoặc Pro - 27.99)
     Vẫn hiện current plan card + logic Un-subscribe sẽ xóa toàn bộ tier này ở app, hiển thị chữ "Old" ở plan card 
  + Giá current LỚN HƠN active tier ngang bằng 
     Ẩn current plan card 
     Giữ nguyên logic Approve new price ở plan card của active tier ngang bằng 
     Giữ nguyên logic các banner thông báo Approve new price,... ở Homepage
  - *DEV: True | Test: True*

- **BR3**
  - - DB organization:
  + Lý tưởng là lock hết các column data của bảng pricing_plans này (plan_code, price, tier) CHỈ được edit cột state 
  + Tester khi muốn test flow thay đổi giá 1 plan, thêm plan mới, vui lòng add new row trong bảng DB này, chứ ko phải edit row data đang có
  VD: plan Std đang có tier = 1, state = active, price = 7.99$
  tester muốn test TH khi Std active update giá về 5$, cần thêm 1 row data mới
  + row cũ: Std update các data (chỉ edit cột state)
                  tier = 1
                  price = 7.99$
                  state = OLD
  + add thêm row mới: điền mọi ttin cho Std
                  tier = 1
                  price = 5$
                  state = ACTIVE
  - *Test: True*
