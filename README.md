# Hệ thống bảo mật RFID kết hợp cảnh báo chuyển động

Dự án hệ thống nhúng ứng dụng đọc thẻ từ (RC522) để cấp quyền truy cập và cảm biến gia tốc/góc nghiêng (MPU6050) để phát hiện xâm phạm/di chuyển trái phép, kèm theo hệ thống cảnh báo. Toàn bộ hệ thống được cấp nguồn ổn định qua mạch hạ áp (Step-down converter).

## 🛠 Phụ kiện & Phần cứng

*   **Vi điều khiển chính:** STM32F103C8T6 - Blue Pill
*   **Module RFID:** MFRC522 (Giao tiếp SPI1)
*   **Cảm biến chuyển động:** MPU6050 (Giao tiếp I2C1)
*   **Nguồn cấp:** Mạch hạ áp LM2596 (Giảm áp từ 12V xuống 5V/3.3V)
*   **Hiển thị/Cảnh báo:** Đèn LED tích hợp và Còi chip (Buzzer)

## 🔌 Sơ đồ nối dây (Pinout)

*Mạch hạ áp cần được chỉnh biến trở để đo ngõ ra chuẩn 5V hoặc 3.3V trước khi cấp nguồn cho vi điều khiển và các module cảm biến để tránh cháy nổ.*

Dựa trên cấu hình vi điều khiển, sơ đồ đấu nối chân chi tiết như sau:

**1. Kết nối Module RFID RC522 (Sử dụng bộ SPI1)**
| Chân MFRC522 | Chân trên STM32 | Ghi chú |
| :--- | :--- | :--- |
| SDA (SS) | PA4 | Được cấu hình là `RC522_RS` |
| SCK | PA5 | Cấu hình `SPI1_SCK` |
| MISO | PA6 | Cấu hình `SPI1_MISO` |
| MOSI | PA7 | Cấu hình `SPI1_MOSI` |
| RST | PA1 | Được cấu hình là `RC522_RST` |
| 3.3V | 3.3V | Cấp nguồn 3.3V |
| GND | GND | Nối chung mass |

**2. Kết nối Cảm biến MPU6050 (Sử dụng bộ I2C1)**
| Chân MPU6050 | Chân trên STM32 | Ghi chú |
| :--- | :--- | :--- |
| SCL | PB6 | Cấu hình `I2C1_SCL` |
| SDA | PB7 | Cấu hình `I2C1_SDA` |
| VCC | 3.3V / 5V | Tuỳ thuộc module có IC ổn áp hay không |
| GND | GND | Nối chung mass |

**3. Các ngoại vi cảnh báo và giao tiếp khác**
| Thành phần | Chân trên STM32 | Ghi chú |
| :--- | :--- | :--- |
| LED (Tích hợp) | PC13 | Cấu hình `LED_BUILDIN` |
| Còi chip (Buzzer)| PB1 | Cấu hình `Buzz` dùng để báo động |
| Output phụ | PB0 | Cấu hình `GPIO_Output` |
| UART1 (Giao tiếp) | PA9 (TX), PA10 (RX) | Cấu hình `USART1_TX`, `USART1_RX` |

## 🚀 Nguyên lý hoạt động

1.  **Chế độ chờ & Cảnh báo:** Hệ thống liên tục đọc dữ liệu góc nghiêng/gia tốc từ MPU6050. Nếu phát hiện thiết bị bị rung lắc hoặc nghiêng quá ngưỡng cho phép, module còi báo (Buzzer) trên chân PB1 và LED trên chân PC13 sẽ phát tín hiệu cảnh báo.
2.  **Mở khóa / Tắt cảnh báo:** Khi người dùng quẹt thẻ từ hợp lệ vào module RC522, hệ thống sẽ thông qua giao tiếp SPI để xác thực UID của thẻ. Nếu đúng, hệ thống báo trạng thái an toàn và vô hiệu hóa báo động.
3.  **Quản lý nguồn:** Mạch hạ áp đảm bảo dòng điện và điện áp ổn định để nuôi MCU, cảm biến và các thành phần báo động, tránh hiện tượng sụt áp khi hệ thống hoạt động đồng thời.

## 📦 Thư viện sử dụng

Dự án này sử dụng các thư viện chuyên biệt dành cho STM32:
*   **Thư viện RC522:** [marcusnogueiraa/rc522-stm32-library](https://github.com/marcusnogueiraa/rc522-stm32-library) - Thư viện giao tiếp module RC522 viết riêng cho vi điều khiển STM32 "Blue Pill".

