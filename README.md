# Hệ thống bảo mật RFID kết hợp cảnh báo chuyển động

Dự án hệ thống nhúng ứng dụng đọc thẻ từ (RC522) để cấp quyền truy cập và cảm biến gia tốc/góc nghiêng (MPU6050) để phát hiện xâm phạm/di chuyển trái phép, kèm theo hệ thống cảnh báo bằng LED. Toàn bộ hệ thống được cấp nguồn ổn định qua mạch hạ áp (Step-down converter).

## 🛠 Phụ kiện & Phần cứng

*   **Vi điều khiển chính:** [Tên MCU, ví dụ: Arduino Uno / STM32F103C8T6 / ESP32]
*   **Module RFID:** MFRC522 (Giao tiếp SPI)
*   **Cảm biến chuyển động:** MPU6050 (Giao tiếp I2C)
*   **Nguồn cấp:** Mạch hạ áp [ví dụ: LM2596 / MP1584] (Giảm áp từ [12V] xuống [5V/3.3V])
*   **Hiển thị/Cảnh báo:** Đèn LED

## 🔌 Sơ đồ nối dây (Pinout)

*Mạch hạ áp cần được chỉnh biến trở để đo ngõ ra chuẩn [5V hoặc 3.3V] trước khi cấp nguồn cho vi điều khiển và các module cảm biến để tránh cháy nổ.*

| MFRC522 (SPI) | Chân trên MCU | | MPU6050 (I2C) | Chân trên MCU |
| :--- | :--- | :--- | :--- | :--- |
| SDA (SS) | [Pin_X] | | VCC | 3.3V / 5V |
| SCK | [Pin_Y] | | GND | GND |
| MOSI | [Pin_Z] | | SCL | [Pin_SCL] |
| MISO | [Pin_W] | | SDA | [Pin_SDA] |
| RST | [Pin_R] | | INT | [Không dùng / Pin_I] |
| 3.3V | 3.3V | | | |
| GND | GND | | | |

| Thành phần khác | Chân trên MCU |
| :--- | :--- |
| Đèn LED (Cảnh báo) | [Pin_LED] |

## 🚀 Nguyên lý hoạt động

1.  **Chế độ chờ & Cảnh báo:** Hệ thống liên tục đọc dữ liệu góc nghiêng/gia tốc từ MPU6050. Nếu phát hiện thiết bị bị rung lắc hoặc nghiêng quá ngưỡng cho phép, đèn LED sẽ nháy sáng cảnh báo liên tục.
2.  **Mở khóa / Tắt cảnh báo:** Khi người dùng quẹt thẻ từ hợp lệ vào module RC522, hệ thống sẽ xác thực UID của thẻ. Nếu đúng, đèn LED báo trạng thái an toàn và vô hiệu hóa báo động.
3.  **Quản lý nguồn:** Mạch hạ áp đảm bảo dòng điện và điện áp ổn định để nuôi cả MCU và các cảm biến cùng lúc, tránh hiện tượng sụt áp khi module SPI và I2C hoạt động song song.

## 📦 Thư viện sử dụng

*   [Tên thư viện RC522 - ví dụ: MFRC522 by GithubCommunity]
*   [Tên thư viện MPU6050 - ví dụ: Adafruit MPU6050]

## ⚙️ Hướng dẫn cài đặt và Build code

1.  Clone repository này về máy:
    ```bash
    git clone [https://github.com/](https://github.com/)[username_của_bạn]/[tên_repo].git
    ```
2.  Mở project bằng IDE [Arduino IDE / PlatformIO / STM32CubeIDE].
3.  Cài đặt các thư viện phụ thuộc đã nêu ở trên.
4.  Biên dịch (Build) và nạp (Upload/Flash) code xuống vi điều khiển.
