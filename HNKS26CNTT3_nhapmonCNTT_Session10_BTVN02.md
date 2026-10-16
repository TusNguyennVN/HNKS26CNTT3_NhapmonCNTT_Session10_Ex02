# HƯỚNG DẪN CÀI ĐẶT **CẢM BIẾN ĐỘ ẨM** CHO TRANG TRẠI LAN
---
## 1. TỔNG QUAN
Cảm biến độ ẩm giúp theo dõi độ ẩm không khí trong nhà lưới, đảm bảo môi trường tối ưu cho Lan phát triển

### THIẾT BỊ CẦN CHUẨN BỊ
- Cảm biến độ ẩm (DHT22 / AM2302)
- Raspberry Pi / Jetson / SoC
- Dây nối Dupoint
- Tua vít, băng keo, ốc vít
- Thẻ nhớ đã cài hệ điều hành
### KÝ HIỆU TRONG TÀI LIỆU
- **Văn bản giải thích** (đọc và làm theo hướng dẫn)
- **Câu lệnh cần gõ vào Terminal** (sao chép và dán hoặc gõ chính xác)
## 2.Các bước cài đặt
1. Cập nhật hệ thống

Mở Terminal và cập nhật danh sách gói
```bash
sudo apt update && sudo apt upgrade -y
```

2. Cài đặt thư viện cho cảm biến DHT22

Cài đặt thư viện đọc dữ liệu từ cảm biến
```bash
sudo apt install -y git python3-pip
sudo pip3 install Adafruit_DHT
```

3. Kích hoạt giao tiếp 1-Wire(Nếu dùng DHT22)

Bật giao tiếp 1-Wire trên hệ thống
```bash
sudo raspi-config
# Chọn: Interface Options >
# 1-Wire > Enable
```

4. Kiểm tra module đã được nhận

Sau khi bật 1-Wire, kiểm tra thiết bị
```bash
ls /sys/bus/wl/devices/
# Kết quả có dạng 28-xxxxxxx
```

5. Chạy thử chương trình đọc cảm biến

Tải và chạy file Python mẫu
```bash
git clone GitHub - adafruit/Adafruit_Python_DHT: Python library to read the DHT series of humidity and tempera
cd Adafruit_Python_DHT/examples
```
```bash
python3 Adafruit_DHT.py 22 4
# 22: DHT22, 4: GPIO pin
# Nhận kết quả độ ẩm và nhiệt độ
```

6. Tự động chạy khi khởi động (Tùy chọn)

Thêm vào crontab để đọc dữ liệu định kỳ
```bash
crontab -e
# Thêm dòng (ví dụ mỗi 5 phút)
# */5 * * * * python3 /home/pi/read_dht22.py
```
## 3. SƠ ĐỒ KẾT NỐI
| DHT22 | Raspberry Pi (BCM) |
| :---: | :---: |
| VCC | 3.3V (Pin 1)|
| GND | GND (Pin 6) |
| DATA | GPIO4 (Pin 7) |
| NC | (Không kết nối) |
## 4. VỊ TRÍ LẮP ĐẶT

Lắp cảm biến ở nơi thoáng, tránh ánh nắng trực tiếp và nước bẩn vào

## 5. KIỂM TRA KẾT QUẢ

Kết quả mẫu khi chạy lệnh ở bước 5:
```bash
Temp=28.4* Humidity=67.1%
pi@farm:~ S
```
```
Ghi chú: Giá trị độ ẩm lý tưởng cho Lan thường từ 60% - 80%. Nếu thấp hơn, cần tăng cường phun sương
```