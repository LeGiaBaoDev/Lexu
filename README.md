# Lexu

Website bán túi xách (e-commerce) xây bằng HTML tĩnh, Bootstrap 5, Font Awesome và jQuery.
Dự án mang tính học tập: giao diện nhiều trang (trang chủ, giỏ hàng...) dùng chung header và footer.

## Công nghệ dùng trong dự án

- **HTML5** cho cấu trúc trang.
- **Bootstrap 5** (trong `assets/vendor/bootstrap`) cho layout, grid, component (carousel, card, button...).
- **Font Awesome 6** cho icon.
- **jQuery** (trong `assets/vendor/jquery`) cho tương tác.
- **CSS thường** trong `assets/css/style.css` cho style riêng.

## Cấu trúc thư mục

```
Lexu/
├── html/               # Các trang và mảnh HTML dùng lại
│   ├── index.html      # Trang chủ
│   ├── cart.html       # Trang giỏ hàng
│   ├── header.html     # Mảnh header dùng chung
│   └── footer.html     # Mảnh footer dùng chung
├── assets/
│   ├── css/style.css   # Toàn bộ style tự viết
│   ├── script/script.js# JS tự viết
│   ├── images/         # Ảnh sản phẩm, banner, logo...
│   └── vendor/         # Thư viện ngoài (bootstrap, jquery)
├── README.md           # Tài liệu này
└── AGENTS.md           # Quy ước chi tiết cho người (và AI) viết code
```

## Chạy dự án

Đây là web tĩnh nên chỉ cần mở file HTML trong trình duyệt.
Nên dùng một local server nhẹ (ví dụ extension **Live Server** của VS Code) để đường dẫn tương đối và giỏ hàng hoạt động đúng.

## Quy ước viết code (tóm tắt)

Phần dưới là những nguyên tắc quan trọng nhất. Quy ước đầy đủ và ví dụ nằm trong [AGENTS.md](./AGENTS.md).

### 1. Đặt tên class theo BEM

BEM = **Block – Element – Modifier**. Một khối giao diện là một *block*; các phần bên trong nó là *element*; biến thể trạng thái là *modifier*.

- Block: `.product-card`
- Element (phần con, nối bằng **hai gạch dưới** `__`): `.product-card__image`, `.product-card__title`
- Modifier (biến thể, nối bằng **hai gạch ngang** `--`): `.product-card--sale`, `.btn--large`

Viết tên bằng tiếng Anh, chữ thường, các từ trong một tên nối bằng **một gạch ngang** (`product-card`, không viết `productCard`).
Mỗi class nói *nó là gì*, không nói *nó trông thế nào* (dùng `.product-card__price`, tránh `.red-text`).

### 2. Viết HTML để dùng lại được nhiều lần, nhiều trang

- Phần lặp lại trên nhiều trang (header, footer) tách ra thành mảnh riêng trong `html/` và nhúng vào, thay vì chép tay mỗi trang một bản.
- Một "component" (ví dụ thẻ sản phẩm) phải có **cùng một cấu trúc HTML và cùng bộ class** ở mọi nơi nó xuất hiện. Muốn đổi giao diện thì chỉ sửa CSS của class đó, không phải sửa từng trang.
- Không nhét style cứng vào thẻ (hạn chế `style="..."` inline). Đưa style vào `style.css` qua class để trang khác tái sử dụng được.
- Dùng đường dẫn tương đối nhất quán tới `assets/` để mảnh HTML nhúng vào trang nào cũng chạy.

### 3. Viết CSS từ rộng đến chi tiết

Sắp xếp CSS theo thứ tự độ cụ thể tăng dần: **reset/chung → layout → component → trạng thái/biến thể → responsive**.

```css
/* 1. Rộng nhất: reset, quy tắc chung cho cả site */
* { margin: 0; padding: 0; box-sizing: border-box; }

/* 2. Component: style cho từng block */
.product-card { ... }
.product-card__title { ... }

/* 3. Chi tiết nhất: biến thể và trạng thái */
.product-card--sale { ... }
.product-card__title:hover { ... }
```

Ưu tiên selector dựa trên class, giữ độ "sâu" của selector thấp (tránh lồng nhiều cấp như `.a .b .c .d`), và **không dùng `!important`** trừ khi bắt buộc phải đè Bootstrap.
