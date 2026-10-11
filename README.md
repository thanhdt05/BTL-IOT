# BTL-IOT — Giám sát tủ thiết bị

Đồ án IoT giám sát nhiệt độ, độ ẩm và điều khiển thông gió tủ thiết bị mạng.

**Trạng thái: chỉ có bộ khung thư mục và tài liệu để nhóm bắt đầu làm việc. Chưa triển khai chức năng, chưa có ứng dụng chạy được.**

Nguồn thống nhất: [Đặc tả chung V1.3](IOT_TU_THIET_BI_DAC_TA_CHUNG_V1_3.md) và [đề bài học phần](ĐỀ%20BÀI%20ĐỒ%20ÁN%20KẾT%20THÚC%20HỌC%20PHẦN.pdf). Khi có khác biệt, nhóm đối chiếu mục 0 của đặc tả và thống nhất trước khi sửa contract.

## Phạm vi và công nghệ đã chốt

- P0: một ESP32 CONTROLLER + DHT22 + relay + quạt DC; AUTO tại thiết bị; web/backend/DB/bảo mật/cảnh báo/nhật ký.
- P1: node giám sát RPM, chỉ làm sau khi P0 đạt nghiệm thu và đủ điều kiện.
- P2: AI, là hướng phát triển sau P1.
- Firmware: C/C++, Arduino ESP32. Backend: Python/Flask, SQLAlchemy, Flask-Migrate. DB: PostgreSQL 16 `iot_cabinet`.
- Frontend: React + Vite, JavaScript. MQTT: Mosquitto + Dynamic Security, contract v3.
- Triển khai mục tiêu: Docker Compose, HTTPS và MQTTS; cấu hình triển khai sẽ do TV4 phối hợp TV2/TV3 bổ sung.

## Phân công

| Thành viên | Phần phụ trách | Thư mục chính |
| --- | --- | --- |
| TV1 | Firmware, phần cứng, MQTT phía thiết bị | `firmware/`, `hardware/` |
| TV2 | Backend thiết bị/dữ liệu, DB, MQTT worker, outbox, lifecycle | `backend/`, `database/` |
| **TV3** | **Tài khoản, phiên, RBAC/scope, audit/nhật ký, broker ACL; UI Users/Profile** | **Các module TV3 trong backend và frontend; xem [kế hoạch TV3](docs/TV3.md)** |
| TV4 | Dashboard, cảnh báo, alert worker, triển khai và tích hợp | `frontend/`, `backend/app/alerts/`, `deployment/` |

TV3 dùng chung Flask app, DB, migration và React app của nhóm. TV2/TV4 tạo nền chạy trước; TV3 bổ sung module trên nhánh riêng.

## Cấu trúc

```text
.github/        Mẫu issue và pull request
backend/        App chung, module, migrations và worker
frontend/       Web chung; chỗ cho Users/Profile của TV3
firmware/       Firmware ESP32
database/       ERD, dictionary, hướng dẫn seed
deployment/     Compose, broker, proxy, TLS khi triển khai
hardware/       BOM, pinout, sơ đồ và ảnh mạch
tests/          Các nhóm kiểm thử dùng chung
docs/           Phân công, contract tham chiếu và kế hoạch TV3
```

Các file `.gitkeep` chỉ giúp Git lưu thư mục trống; các file README mô tả điểm bàn giao, chưa chứa logic nghiệp vụ.

## Bắt đầu làm việc

```bash
git clone https://github.com/thanhdt05/BTL-IOT.git
cd BTL-IOT
git switch main
git pull --ff-only origin main
git switch -c feature/tv3-auth
```

Thay tên nhánh theo việc của mỗi người. Đọc [quy tắc đóng góp](CONTRIBUTING.md), [điểm bàn giao](docs/TEAM.md) và README của phần mình trước khi viết code.

Chưa có lệnh cài đặt/chạy ứng dụng ở commit này. Thành viên phụ trách sẽ thêm hướng dẫn và khóa phiên bản thư viện sau khi kiểm tra được phần nền chạy.

`.env.example` là mẫu tên biến tham khảo. Tạo `.env` cục bộ khi bắt đầu triển khai; không commit mật khẩu, token, MQTT credential hoặc khóa TLS.
