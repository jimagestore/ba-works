# USBARUP.2.4_Confirmation_modal

---

**User story:** USBARUP.2.4

**Conversation:** UI logics - Show Confirmation modal when user changes current plan and lose beneficial price.

**Design:** https://www.figma.com/design/nOJoqMlFuZtUxfAQVSzpbG/SBM---UI-Mockups?node-id=4725-181047&m=dev 

**Card:**
- 1. Switch Plan Confirmation modal

---

**Confirmation:**

**Pre-condition / Path:**
Pre-condition:
User has installed MS Barcode successfully
User are subscribing a plan having state = old

Path:
1. Pricings page -> Select other plan card -> Subscribe/Downgrade/Upgrade 
2. Pricings page -> Current "Old" plan card -> Un-subscribe

---

## Requirements

### Switch Plan Confirmation modal

- **Header**
  - **Type:** Text
  - **Required:** N/A
  - **Description:** - "Switch Plan Confirmation"
  - *DEV: True | Test: True*

- **Content**
  - **Type:** Text
  - **Required:** N/A
  - **Description:** - "You are currently on the <current "Old" plan>, giving you all benefits of the <active plan same tier> for better price. 
  Once you switch, you will NOT be able to return to the <current "Old" plan>. Are you sure?" 
  
  e.g: "You are currently on the Pro Plan (Old), giving you all benefits of the Pro Plan for better price. 
  Once you switch, you will NOT be able to return to the Pro Plan (Old). Are you sure?"
  - *DEV: True | Test: True*

- **Cancel**
  - **Type:** Button
  - **Required:** N/A
  - **Description:** - Click button này để đóng modal, giữ nguyên current plan với state Old
  - *DEV: True | Test: True*

- **Sure, I want to continue**
  - **Type:** Button
  - **Required:** N/A
  - **Description:** - Click button này để confirm switch plan
  => Redirect sang confirmationURL của Shopify/ confirm việc unsubscribe current plan với state = old
  - *DEV: True | Test: True*

- **(x) icon**
  - **Type:** Icon
  - **Required:** N/A
  - **Description:** - Click button này để đóng modal, giữ nguyên current plan với state Old
  - *DEV: True | Test: True*

- **BR1**
  - - Với user có current plan/tier thuộc state = active 
  + Giữ nguyên luồng subscription management như hiện tại
  + Không hiện modal confirm ở trên, khi change plan hoặc un-subscribe current plan
  - *DEV: True | Test: True*

- **BR2**
  - - Với user có current plan/tier thuộc state = old VÀ "giá của current plan" LỚN HƠN "giá của active tier ngang bằng"
  + Follow luồng ẩn current plan card VÀ luồng gạch chân giá cũ như các US trước
  + Khi click Approve new price (tại plan card của active tier ngang bằng), giữ nguyên luồng Approve new price như hiện tại
  - *DEV: True | Test: True*

- **BR3**
  - - Với user có current plan/tier thuộc state = old VÀ "giá của current plan" NHỎ HƠN hoặc BẰNG "giá của active tier ngang bằng"
  => Hiển thị modal Switch Plan Confirmation nêu trên khi user
  + Un-subscribe current plan 
  + hoặc Switch sang bất kỳ plan khác (cùng/khác tier, cùng/khác interval)
  - *DEV: True | Test: True*

- **BR4**
  - - Tại Switch Plan Confirmation modal, nếu user click Cancel
  => Đóng modal, vẫn giữ tại current plan có state = old
  - *DEV: True | Test: True*

- **BR5**
  - - Nếu user click Sure, I want to continue
  + Nếu là Un-subscribe, confirm việc back về Free
    * Đóng modal, dừng việc hiển thị, logic theo plan card nêu trên 
    * Follow logic tier đó có state = old và KHÔNG phải là current plan => ẩn khỏi UI của store đó
  
  + Nếu là switch sang paid plan khác 
     * Redirect sang confirmationURL của Shopify
     * Khi user approve subscription mới tại URL này và back về Pricings page, follow theo current plan vừa switch
     * Plan card ban đầu sẽ bị ẩn khỏi UI (do khi này state = old và KHÔNG phải current plan)
  - *DEV: True | Test: True*
