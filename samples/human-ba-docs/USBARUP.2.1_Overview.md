# USBARUP.2.1_Overview

---

**User story:** USBARUP.2.1

**Conversation:** UI logics - Overview of Pricing plans page when applying new DB mechanism.

**Design:** https://www.figma.com/design/nOJoqMlFuZtUxfAQVSzpbG/SBM---UI-Mockups?node-id=4725-181047&m=dev 

**Card:**
- 1. Pricing page - Having Free, Std, Plus, Pro plan - Current plan = Pro with state = Old

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:
1. SBM app -> Pricings page

---

## Requirements

### Plan Card - Plan details

- **Free plan**
  - **Type:** Text 
  - **Required:** N/A
  - **Description:** - Dev update the details of plan card as below:
  200 monthly labels 
  (expire at the end of period)
  Generate unlimited barcodes
  GTINs (EANs, UPCs,...) support
  Advanced Label Customization
  Email & Live-chat support
  - *DEV: True | Test: True*

- **Standard plan**
  - **Type:** Text 
  - **Required:** N/A
  - **Description:** - Dev update the details of plan card as below:
  2000 monthly labels 
  (expire at the end of period)
  All features in Free Plan
  Priority Support Channel
  - *DEV: True | Test: True*

- **Plus plan**
  - **Type:** Text 
  - **Required:** N/A
  - **Description:** - Dev update the details of plan card as below:
  30000 monthly labels 
  (expire at the end of period)
  All features in Standard Plan
  Priority Support Channel
  - *DEV: True | Test: True*

- **Pro plan**
  - **Type:** Text 
  - **Required:** N/A
  - **Description:** - Dev update the details of plan card as below:
  Unlimited monthly labels 
  (expire at the end of period)
  All features in Standard Plan
  Priority Support Channel
  - *DEV: True | Test: True*

- **BR1**
  - - Dev set điều kiện hiển thị UI của các tier - plans trên Pricing page dựa theo state của các plan:
  + State là active/current: Hiển thị ra giao diện
  
  + State là old: 
     Là current plan: Vẫn hiển thị trên giao diện + details plan card tùy theo điều kiện (nói rõ ở US sau)
     Không phải current plan: Chỉ hiển thị trên các store test được ghi nhận trên biến môi trường  => hiện tại app đã work rồi Ẩn khỏi UI
  
  + State khác (others): Chỉ hiển thị trên các store test được ghi nhận trên biến môi trường
  => Cần task set biến môi trường cho state này
  => Các state future, draft,... đều include trong state khác (others) này 
  
  (refers to USBARUP.1.2)
  - *DEV: True | Test: True*

- **BR2**
  - - Dev sắp xếp giao diện hiển thị của các Plan cards trên giao diện như sau
  + Các gói active/current → các gói old (nếu có) → các gói state others (nếu có)
  (Mockup đang thể hiện Plus plan có state = active)
  + Trong cùng một nhóm (active/old/other…), sắp xếp các gói theo thứ tự tier dạng asc (tier thấp nhất lên đầu)
  - *DEV: True | Test: True*

- **BR3**
  - - Hiện tại, với app Barcode, đang có các Plan (tier) sau:
  + Free plan
  + Standard plan
  + Pro plan
  + Plus plan (state = others)
  
  => All domains: Hiển thị 3 Plan card và theo thứ tự Free -> Standard -> Pro 
  (Đối ứng với tier 0 - 1 - 3)
  => Specific domains (những store test): Hiển thị thêm Plus plan, xếp đằng sau Pro plan (do đang là state others) 
  - *DEV: True | Test: True*

- **BR4**
  - - Khi set up cho Plus plan show ở All domains được thực hiện (chỉnh DB cho state của Plus = active)
  => Plus plan chuyển state thành Active, và hiện ở mọi store
  => Sắp xếp lại Plan card theo logic ở BR2
  - *DEV: True | Test: True*

- **BR5**
  - - Các Plan card của interval Yearly sẽ follow theo Plan card tương ứng tại Monthly
  + StandardYear - Standard 
  + ProYear - Pro
  + PlusYear - Plus 
  
  => Khi update tier + state của tier/plan bên interval Monthly, update tương ứng cho bên interval Yearly (trừ TH current plan = old và Yearly)
  - *DEV: True | Test: True*
