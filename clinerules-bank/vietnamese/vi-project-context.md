---
description: Vietnamese-specific rules. Dành cho dự án Việt Nam — ngôn ngữ, tiền tệ, múi giờ, tích hợp dịch vụ nội địa.
author: TaiVo-AI
version: 1.0
tags: ["vietnamese", "localization", "i18n", "vietnam"]
---

# Vietnamese Project Context

> Dành cho contribute vào `cline-promes`. Copy vào `.clinerules/` khi làm dự án Việt Nam.

## Ngôn ngữ & Encoding

- LUÔN trả lời bằng Tiếng Việt
- Database: collation `utf8mb4_unicode_ci` (MySQL) hoặc `UTF8` (PostgreSQL)
- CSV xuất cho Excel: thêm BOM `\xEF\xBB\xBF`
- PDF: embed font Roboto / Noto Sans (hỗ trợ dấu tiếng Việt)

## Định dạng dữ liệu Việt Nam

```typescript
// Tiền VND — không có phần thập phân, dấu chấm ngăn hàng nghìn
const formatVND = (n: number) =>
  new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(n);

// Múi giờ: Asia/Ho_Chi_Minh (UTC+7)
const vnDate = new Intl.DateTimeFormat('vi-VN', {
  timeZone: 'Asia/Ho_Chi_Minh', dateStyle: 'short'
}).format(new Date());

// Số điện thoại: 10 số, đầu 03x/05x/07x/08x/09x
const vnPhone = /^(0|\+84)[3-9]\d{8}$/;

// Mã số thuế: 10 hoặc 13 chữ số
const mst = /^\d{10}(\d{3})?$/;
```

## Dịch vụ tích hợp phổ biến

| Dịch vụ | Loại | Ghi chú |
|---------|------|---------|
| VNPay, MoMo, ZaloPay | Thanh toán | Cần merchant registration |
| VNPT/Viettel SMS | OTP / thông báo | API SMS gateway |
| Ahamove, Grab | Giao hàng | Webhook tracking |
| HKDT | Hóa đơn điện tử | TT 78/2021 |

## Compliance

- **Nghị định 13/2023**: Bảo vệ dữ liệu cá nhân — cần explicit consent
- **TT 78/2021**: Hóa đơn điện tử bắt buộc từ 1/7/2022
