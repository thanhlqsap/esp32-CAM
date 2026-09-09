# 📁 Hướng Dẫn Quản Lý & Triển Khai GitHub (ESP32-CAM)

Thư mục này tập hợp toàn bộ các tệp tin liên quan đến **GitHub Releases**, **Cloud Auto-OTA** và **GitHub Pages Web Installer**.

---

## 📂 Danh Sách Các File Trong Thư Mục `Github/`

| Tên File / Thư mục | Mục Đích Sử Dụng |
| :--- | :--- |
| **[`HD_CAP_NHAT_SITE.html`](file:///d:/Arduino/ESP32_cam/Github/HD_CAP_NHAT_SITE.html)** | **⭐ Hướng dẫn chi tiết (HTML) cách cập nhật phiên bản mới & Changelog.** |
| **[`docs/changelog.json`](file:///d:/Arduino/ESP32_cam/Github/docs/changelog.json)** | **File JSON chứa danh sách các phiên bản & nội dung Changelog hiển thị lên Web Flasher.** |
| **[`version.json`](file:///d:/Arduino/ESP32_cam/Github/version.json)** | File cấu hình phiên bản dùng cho tính năng **Cloud Auto-Update** (ESP32 tự tải cập nhật từ xa). |
| **[`web_flasher.html`](file:///d:/Arduino/ESP32_cam/Github/web_flasher.html)** | Giao diện nạp Firmware 1-Click qua Web Serial (mở trực tiếp trên máy tính để nạp qua cáp USB). |
| **[`docs/index.html`](file:///d:/Arduino/ESP32_cam/Github/docs/index.html)** | Trang web nạp chính chạy trên dịch vụ **GitHub Pages** trực tuyến. |
| **[`docs/manifest.json`](file:///d:/Arduino/ESP32_cam/Github/docs/manifest.json)** | File cấu hình nạp chuẩn **ESP Web Tools** cho Home Assistant / Web Installer. |

---

## 🚀 1. Cách Bật Trang Nạp Online (GitHub Pages)

1. Đẩy toàn bộ thư mục dự án lên repository của bạn trên GitHub (`git push origin main`).
2. Vào **Settings** của repository → chọn mục **Pages**.
3. Tại phần **Build and deployment > Branch**: Chọn nhánh **`main`** và thư mục **`/docs`** → bấm **Save**.
4. Trang web nạp sẽ hoạt động tại:
   `https://<your-username>.github.io/ESP32_cam/`

---

## 📦 2. Quy Chuẩn Tên File Firmware (.bin)

Khi chạy công cụ build (`build_firmware.bat`), hệ thống tự động xuất 2 cặp file:

1. **Bản Luôn Mới Nhất (Latest - Tự động ghi đè mỗi lần build):**
   - **`Full_Flash.bin`** (Offset `0x0000`): File gộp đầy đủ (Bootloader + Partitions + Boot_app0 + App). Dùng để nạp lần đầu qua dây cáp USB bằng [web_flasher.html](file:///d:/Arduino/ESP32_cam/web_flasher.html) hoặc trang GitHub Pages.
   - **`OTA_Update.bin`** (Offset `0x10000`): File ứng dụng tinh gọn (chỉ chứa App code). Dùng để nạp nâng cấp không dây qua Wi-Fi trên Tab **Hybrid OTA** của Web Dashboard hoặc tính năng Cloud Auto-OTA.

2. **Bản Lưu Trữ Theo Version Thời Gian (Timestamped):**
   - **`Full_Flash_ddMMyyyy_HHmmss.bin`** (Ví dụ: `Full_Flash_09092026_090000.bin`)
   - **`OTA_Update_ddMMyyyy_HHmmss.bin`** (Ví dụ: `OTA_Update_09092026_090000.bin`)

---

## ☁️ 3. Cách Cập Nhật Firmware Qua Cloud Auto-OTA

Khi có bản cập nhật mới:
1. Chạy `build_firmware.bat` để biên dịch và xuất các file `Full_Flash.bin` và `OTA_Update.bin`.
2. Sửa số phiên bản trong file `version.json` (ví dụ: `"version": "1.0.1"`).
3. Đẩy file `version.json` và file `build/OTA_Update.bin` lên GitHub.
4. Trên Web Dashboard của ESP32-CAM (Tab 3: Hybrid OTA), nhấn **"🔍 Kiểm tra & Tự cập nhật ngay"**, ESP32 sẽ tự động tải `OTA_Update.bin` qua Wi-Fi mà không cần cắm dây.
