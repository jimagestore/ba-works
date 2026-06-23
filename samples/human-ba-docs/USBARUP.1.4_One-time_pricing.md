# USBARUP.1.4_One-time_pricing

---

**User story:** USBARUP.1.4

**Conversation:** DB mechanism - Define One-time pricing by State rules.

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
  - - Bind logic của State vào các packages app cung cấp
  + Package đang trên LIVE: State = active
  + Package bị ẩn đi: State = old
  + Package mới add nhưng điều chỉnh việc lên LIVE theo biến môi trường
     State = future khi chưa publish
     State = active khi đã publish
  - *DEV: True | Test: True*
