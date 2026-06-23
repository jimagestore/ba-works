# USBARUP.2.2_Current_plan_card

---

**User story:** USBARUP.2.2

**Conversation:** UI logics - Current plan card when applying new DB mechanism.

**Design:** https://www.figma.com/design/nOJoqMlFuZtUxfAQVSzpbG/SBM---UI-Mockups?node-id=4725-181047&m=dev 

**Card:**
- 1. Current plan = Pro + state = Old; access interval of current plan
- 3. UI of subscribed states of all plan cards - Show Current Plan + Un-subscribe CTA

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully

Path:
1. SBM app -> Pricing page -> Interval of current plan
2. SBM app -> Pricing page -> Interval not having current plan

---

## Requirements

### Plan card - Current plan

- **Plan Name**
  - **Type:** Text
  - **Required:** N/A
  - **Description:** - Với Current Plan có state = old
  => Thêm suffix "(Old)" vào cuối Plan name
  
  - Các Plan card khác, hoặc Current plan ở state khác, Plan name vẫn work như hiện tại
  - *DEV: True | Test: True*

- **Help text (below list of functions)**
  - **Type:** Text
  - **Required:** N/A
  - **Description:** - Với Current Plan có state = old
  + Hiển thị help text bên dưới list các tính năng
  + Content = "Disappear after unsubscribing/changing plan!"
  - *DEV: True | Test: True*

- **Current Plan**
  - **Type:** Text
  - **Required:** N/A
  - **Description:** - "Current Plan" text sẽ có color code trùng với color code của 
  1. Banner - Plan Name
  2. Plan card - Plan name 
  3. Plan card - divider của card
  
  - Cụ thể color code:
  + Free: #303030 var(--p-color-text)
  + Standard: #005BD3 var(--p-color-text-emphasis) 
  + Plus: #5700D1 var(--p-color-text-magic)
  + Pro: #5E4200 var(--p-color-text-warning)
  
  => Với Free plan, Current Plan có color code != code của Plan Name, Divider (var(--p-color-text-secondary))
  - *DEV: True | Test: True*

### Pricing banner - based on Current plan

- **Current = Free**
  - **Type:** Banner
  - **Required:** N/A
  - **Description:** - Icon "Info"
  - Background color: #EAF4FF var(--p-color-surface-info) 
  - Content: "Upgrade plan to enjoy exciting advanced features."
  - *DEV: True | Test: True*

- **Current = Std**
  - **Type:** Banner
  - **Required:** N/A
  - **Description:** - Icon "Subscription"
  - Background color: #F0F2FF var(--p-color-surface-emphasis)
  - Content: 
  + "You are using Standard Plan. Your next billing period is <thời gian renew>." 
  for current = monthly std
  + "You are using Standard Yearly Plan. Your next billing period is <thời gian renew>." 
  for current = yearly std
  
  - *DEV: True | Test: True*

- **Current = Plus**
  - **Type:** Banner
  - **Required:** N/A
  - **Description:** - Icon "Subscription"
  - Background color: #F8F7FF var(--p-color-surface-magic)
  - Content: 
  + "You are using Plus Plan. Your next billing period is <thời gian renew>."
  for current = monthly plus
  + "You are using Plus Yearly Plan. Your next billing period is <thời gian renew>."
  for current = yearly plus
  - *DEV: True | Test: True*

- **Current = Pro**
  - **Type:** Banner
  - **Required:** N/A
  - **Description:** - Icon "Subscription"
  - Background color: #FFF1E3 var(--p-color-surface-warning)
  - Content:
  + "You are using Pro Plan. Your next billing period is <thời gian renew>."
  for current = monthly pro
  + "You are using Pro Yearly Plan. Your next billing period is <thời gian renew>."
  for current = yearly pro
  - *DEV: True | Test: True*

- **BR1**
  - - Dev check điều kiện để hiển thị, update thông tin trên card của Current plan dựa theo state và so sánh giá với tier ngang bằng (trong CÙNG một Yearly hoặc Montthly pricing)
  - *DEV: True | Test: True*

- **BR2**
  - 1. State là active/current: Hiển thị trên giao diện
  
  2. State là old:
  a. Nếu “giá của currrent plan” THẤP HƠN hoặc bằng “giá của active plan với tier ngang bằng”: 
  + Hiển thị current plan card
  + Thêm “(Old)” vào plan name 
     * trên current plan card
     * trên Pricing banner 
  + Thêm chữ “Disappear after unsubscribing/changing plan!” vào cuối list quyền lợi
  => Sắp xếp plan card này ở cuối list (trong hàng state = Old)
  (work trong trường hợp MS muốn tăng giá 1 plan)
  
  b. Nếu “giá” CAO HƠN “giá của active plan với tier ngang bằng”: Ẩn current plan card
  (work trong trường hợp MS muốn giảm giá 1 plan)
  
  3. State là future:
  - Logic hiển thị theo logic của state others
  + Chỉ hiển thị Plan card này tại những store test của MS (store được điền trong biến môi trường)
  - *DEV: True | Test: True*

- **BR3**
  - - Về CTA:
  + nút CTA tại Current plan card chuyển thành chữ “Current Plan”, màu chữ giống màu chữ của tên plan (check design để rõ hơn)
  + Vẫn có nút Un-subscribe ở dưới chữ "Current Plan" để user thao tác việc hủy gói (cho những plan card != Free)
  - *DEV: True | Test: True*

- **BR4**
  - - Chỉ xét so sánh giá của current plan với các tier ngang bằng TRONG CÙNG một interval: Yearly hoặc Montlhy
  => Với loại interval còn lại, các tier ngang bằng với current plan vẫn đươc hiển thị bình thường theo rules tier + state - Nói rõ ở US sau
  - *DEV: True | Test: True*
