# LEIJI-407 — [All] [Pricing] New universal pricing definition data (DB) & display (UI) mechanism

**Status:** Done
**Reporter:** Ji Phan
**Assignee:** Ji Phan
**Priority:** Low
**Project:** Leiji Project All

---

## Description

Logic định nghĩa và hiển thị pricing mới, áp dụng cho tất cả các sản phẩm.

---

### [ĐỊNH NGHĨA PRICING]

1. Mỗi **pricing plan** sẽ có một trường data là **tier**, nhận giá trị từ **0 → infinity**, thể hiện mức quyền lợi của plan
   1. **Tier 0** là tier đặc biệt, chuyên dành cho **free plan**, chứ không mang ý nghĩa "dành cho gói thấp nhất". Sản phẩm không có free plan thì gói thấp nhất sẽ là tier 1.
      - VD với Barcode:
        - Free: tier 0
        - Standard: tier 1
        - Plus: tier 2
        - Pro: tier 3
   2. Cơ chế chặn **quyền lợi** plan sẽ **không check theo tên gói, id gói,…** nữa mà **check theo tier**.
      - VD với Barcode:
        - tier 0: 200 monthly labels
        - tier 1: 2k monthly labels
        - tier 2: 25k monthly labels
        - tier 3: unlimited monthly labels

2. Mỗi **pricing plan** có một trường đánh dấu **state** để phân biệt tình trạng hoạt động của plan:
   1. **old**: plan cũ, không nhận subscribe mới, nhưng những user đang dùng vẫn được tiếp tục dùng
   2. **current/active**: plan đang hoạt động hiện tại, có nhận subscribe mới
   3. **draft**: gói chưa hoạt động, đang dev,…
   4. **future**: gói sẽ active ở tương lai, đã done code, done logic nhưng chưa hoạt động
   5. Có thể thêm các state khác nếu tương lai cần thiết

3. Khi định nghĩa một pricing plan:
   1. **luôn phải gán tier trước, logic luôn phát biểu theo tier**
   2. luôn quy hoạch cách gán state:
      - **không được phép có 2 plan cùng tier cùng active**
      - gán state dựa theo development progress (khi nào để là draft, để là future,… và khi nào chính thức chuyển thành active, khi chuyển thành active thì có gói nào khác bị đẩy thành old không,…)
      - trong những trường hợp có logic đặc biệt ăn theo development progress, thì ngoài điều kiện check chặn logic theo tier, còn có thể check chặn logic theo state luôn

4. **One-time** có thể định nghĩa riêng bảng hoặc chung bảng **tuỳ dev**, nhưng **không liên quan đến tier**, còn **state vẫn có thể có**.

---

### [LOGIC HIỂN THỊ UI TRANG PRICING]

1. Khi vào trang, check state các gói:
   1. State là **active**: Hiển thị ra giao diện
   2. State là **old**:
      - Nếu là current plan:
        - Nếu "giá" **THẤP HƠN hoặc bằng** "giá của active plan với tier ngang bằng":
          - **Hiển thị current plan card**
          - Thêm "(Old)" vào plan name trên current plan card
          - Thêm chữ "Disappear after unsubscribing/changing plan!" vào cuối list quyền lợi
        - Nếu "giá" **CAO HƠN** "giá của active plan với tier ngang bằng": **Ẩn current plan card**
      - Nếu không phải current plan: Dùng chung logic của **state khác (1.c)**
   3. State khác:
      - Chỉ hiển thị trên các store test được ghi nhận trên biến môi trường

2. Sắp xếp các gói trên giao diện:
   1. Các gói active → các gói old (nếu có) → các gói state khác (nếu có)
   2. Trong cùng một nhóm (active/old/other…), sắp xếp các gói theo thứ tự tier (tier thấp nhất lên đầu)

3. Ở current plan card, nút CTA chuyển thành chữ "Current Plan", màu chữ giống màu chữ của tên plan (check design để rõ hơn)

4. Check thông tin của **current plan**, từ đó sửa đổi cách hiển thị cho những active plan card **khác**:
   1. current tier = 0: đang ở free **hoặc** current plan nằm ở interval (tab) khác (VD đang mở tab yearly, nhưng current plan là monthly)
      - CTA của tất cả các active plan card khác đều hiện label "Subscribe now"
   2. current tier != 0 **và** current plan nằm ở interval (tab) hiện tại (VD đang mở tab monthly, current plan cũng là monthly):
      - hiển thị nút unsubscribe ở plan card hiện tại
      - với các active plan card tier cao hơn: CTA hiện label "Upgrade"
      - với các active plan card tier thấp hơn:
        - tier != 0: CTA hiện label "Downgrade", chuyển thành dạng default/secondary
        - tier = 0 (free): Không hiển thị action gì
      - với các **active** plan card **tier ngang bằng**:
        - nếu giá của plan đó thấp hơn giá của current plan:
          - CTA hiện label "Approve new price"
          - hiện thêm giá của current plan với gạch ngang bên cạnh (mang ý nghĩa old price)
        - nếu giá của plan đó cao hơn hoặc bằng giá của current plan: CTA hiện label "Subscribe"

---

### [CÁC LOGIC SUBSCRIPTION MANAGEMENT]

- Những phần xử lý subscription management hiện tại (subscribe, unsubscribe, change plan,… đại khái là những gì xảy ra sau khi click các button trên giao diện) giữ nguyên, không thay đổi gì
- Thêm phần xử lý unsubscribe/change plan khi tăng giá ("giá của current plan" **THẤP HƠN** "giá của active plan với tier ngang bằng"): Vì chỉ cần mất current plan, là merchant không thể re-subscribe lại giá tốt được nữa, nên phải búng popup confirm báo merchant kiểu "Mày đang được dùng giá tốt, nếu mày thay đổi/huỷ gói thì sau này sẽ không thể subscribe lại gói thơm tho này nữa đâu"

---

### Notes for BA

- Đây là phần logic universal quan trọng, nên:
  - Giữ nguyên một bản logic gốc của Ji để refer nếu phát sinh điều gì không hiểu
  - Cần một document tổng để follow chung lâu dài, giống phần document công thức invoice của Kaitlyn, focus vào document này trước thay vì các BA Docs theo US. Hiện tại có thể copy nguyên logic của Ji cũng được, chưa cần sửa gì thêm
  - Trong quá trình bẻ task, làm docs theo US, chỗ nào thấy thiếu thông tin thì cần hỏi lại Ji luôn, nếu muốn chủ động tạo solution trước thì sau đó vẫn cần hỏi Ji confirm, không nên tự đắp thêm thông tin xong bắn thẳng dev mà chưa qua confirm. Quá trình này cũng có thể cần hỏi ý kiến a Đen, vì đây là logic universal cho tất cả app
  - Bóc task cho cả Barcode và Order Printer.

---

**Design:** [Figma Mockups](https://www.figma.com/design/nOJoqMlFuZtUxfAQVSzpbG/SBM---UI-Mockups?node-id=4697-84107&p=f&t=dMzoXlim8TD8O0uD-0)
