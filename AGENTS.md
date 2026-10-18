# Quy ước viết code cho dự án Lexu

Tài liệu này dành cho bất kỳ ai viết code trong dự án — cả người lẫn AI.
Mục tiêu: code đồng nhất, dễ đọc, và **tái sử dụng được** giữa các trang.
Hãy đọc hết trước khi thêm hoặc sửa HTML/CSS.

---

## 1. Nguyên tắc chung

- Viết code bằng tiếng Anh cho tên class, id, biến; comment có thể bằng tiếng Việt.
- Giữ code gọn: xoá code thừa, không để lại class hoặc style không dùng.
- Ưu tiên tận dụng tiện ích có sẵn của **Bootstrap** (grid, spacing `p-3`, flex `d-flex`...) trước khi tự viết CSS mới.
- Chỉ tự viết CSS khi Bootstrap không đáp ứng được, và viết vào `assets/css/style.css`.
- Thụt lề thống nhất (4 dấu cách), đóng đầy đủ thẻ, mỗi khối giao diện xuống dòng rõ ràng.

---

## 2. Đặt tên class theo BEM

Dùng quy ước **BEM (Block – Element – Modifier)** cho mọi class tự tạo.

### Block — một khối giao diện độc lập
Là một thành phần đứng một mình có nghĩa: `header`, `product-card`, `cart-item`, `footer`.
Tên viết thường, nhiều từ nối bằng **một gạch ngang**: `product-card`.

### Element — phần con bên trong block
Nối tên block với tên element bằng **hai dấu gạch dưới** `__`:

```html
<div class="product-card">
    <img class="product-card__image" ...>
    <h3 class="product-card__title">...</h3>
    <span class="product-card__price">...</span>
</div>
```

- Element luôn gắn với block cha của nó, không đứng một mình.
- Không lồng element trong tên: viết `product-card__title`, **không** viết `product-card__body__title`.

### Modifier — biến thể hoặc trạng thái
Nối bằng **hai dấu gạch ngang** `--`, dùng kèm class gốc chứ không thay thế nó:

```html
<article class="product-card product-card--sale">...</article>
<button class="btn btn--large">...</button>
```

### Quy tắc đặt tên
- Tên mô tả **chức năng / ý nghĩa**, không mô tả hình thức: dùng `product-card__price`, tránh `.text-red`, `.big-box`.
- Luôn `kebab-case` (chữ thường, gạch ngang), không `camelCase`, không `snake_case`.
- Một class = một việc. Không nhồi nhiều ý nghĩa vào một tên.

> Lưu ý: class của **Bootstrap** (`d-flex`, `container`, `btn`, `carousel`...) giữ nguyên, không đổi theo BEM. BEM chỉ áp dụng cho class **mình tự tạo**.

---

## 3. Viết HTML để dùng lại nhiều lần, nhiều trang

### 3.1. Tách phần dùng chung
- Header và footer dùng chung cho mọi trang → giữ trong `html/header.html` và `html/footer.html`, nhúng vào từng trang thay vì chép tay.
- Khi sửa header/footer, chỉ sửa một chỗ; mọi trang phải cập nhật theo.

### 3.2. Component phải nhất quán
- Một component (ví dụ thẻ sản phẩm) phải có **cùng cấu trúc HTML và cùng bộ class** ở mọi nơi.
- Nhờ vậy, muốn đổi giao diện tất cả thẻ sản phẩm thì chỉ sửa CSS `.product-card`, không phải đi sửa từng trang.
- Trước khi tạo khối HTML mới, kiểm tra xem đã có component tương tự chưa; nếu có thì tái sử dụng cấu trúc/class đó.

### 3.3. Tách nội dung khỏi trình bày
- **Không** nhét style cứng vào thẻ bằng `style="..."` inline. Đưa style vào `style.css` qua class.
  - Ví dụ `style="height: 70px;"` trên logo nên chuyển thành class `.logo__image { height: 70px; }`.
- Dùng thẻ HTML đúng ngữ nghĩa: `<header>`, `<nav>`, `<section>`, `<footer>`, `<button>` cho nút, `<a>` cho liên kết.
- Thuộc tính `alt` cho ảnh phải mô tả thật, không để `alt="..."`.

### 3.4. Đường dẫn
- Dùng đường dẫn tương đối nhất quán tới `assets/` (`../assets/...` từ thư mục `html/`) để mảnh HTML nhúng vào trang nào cũng chạy.
- Ảnh đặt trong `assets/images/`, theo cách đặt tên đang dùng (`product1.jpg`, `C-Handbags.jpg`, `bg-banner1.png`...).

---

## 4. Quy tắc CSS: viết từ rộng đến chi tiết

Sắp xếp `style.css` theo **độ cụ thể tăng dần**, từ trên xuống dưới:

1. **Reset / chung toàn site** — rộng nhất.
   ```css
   * { margin: 0; padding: 0; box-sizing: border-box; }
   ```
2. **Layout / khung trang** — section, container, lưới chính.
3. **Component (block)** — style cho từng block và element của nó.
   ```css
   .product-card { ... }
   .product-card__image { ... }
   .product-card__title { ... }
   ```
4. **Modifier / trạng thái** — biến thể, `:hover`, `:focus`, `active`. Chi tiết nhất, đặt sau cùng của mỗi component.
   ```css
   .product-card--sale { ... }
   .product-card__title:hover { ... }
   ```
5. **Responsive** — các `@media` gom ở cuối (hoặc cuối mỗi component), đi từ màn hình lớn tới nhỏ nhất quán.

### Nguyên tắc selector
- Ưu tiên selector dựa trên **class**, hạn chế selector theo tag hoặc id.
- Giữ selector **nông**: tránh lồng sâu như `.a .b .c .d`. Với BEM, thường chỉ cần một class là đủ.
- **Không dùng `!important`**, trừ khi bắt buộc phải đè class của Bootstrap — và khi dùng phải ghi chú lý do.
- Nhóm code theo component, mỗi nhóm có comment tiêu đề:
  ```css
  /* ===== product-card ===== */
  ```
- Màu, khoảng cách, font nên dùng lại giá trị thống nhất giữa các component (tránh mỗi chỗ một mã màu khác nhau).

---

## 5. JavaScript

- JS tự viết đặt trong `assets/script/script.js`.
- Chọn phần tử qua class/id đã có; nếu cần class chỉ dùng cho JS thì đặt tiền tố `js-` (ví dụ `js-add-to-cart`) và **không** gắn style vào class đó.
- Tận dụng jQuery đã có sẵn trong dự án thay vì thêm thư viện mới.

---

## 6. Checklist trước khi commit

- [ ] Class tự tạo đặt theo đúng BEM (`block__element--modifier`).
- [ ] Không thêm `style="..."` inline mới; style nằm trong `style.css`.
- [ ] Component mới tái dùng cấu trúc/class đã có, không chép trùng.
- [ ] CSS xếp từ rộng đến chi tiết, có comment nhóm, không `!important` vô cớ.
- [ ] Đường dẫn `assets/` tương đối và chạy đúng trên mọi trang.
- [ ] Mở thử trên trình duyệt, kiểm tra cả màn hình nhỏ (responsive).
