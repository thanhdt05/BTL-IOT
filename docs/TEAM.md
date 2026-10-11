# Phân công và điểm bàn giao

Tài liệu này tóm tắt mục 10–12 của đặc tả V1.3. Mọi hạng mục dưới đây hiện **TODO**, bộ khung chưa triển khai chức năng.

| Thành viên | Bàn giao đầu tiên | Phối hợp |
| --- | --- | --- |
| TV1 | Mạch nhiệt/ẩm + AUTO/relay, sau đó telemetry/status/ACK v3 | TV2 chốt payload; TV3 kiểm credential/ACL |
| TV2 | Flask app factory, `extensions.py`, PostgreSQL/migrations, device fixtures và API mẫu | TV3 schema IAM/scope; TV4 API/dashboard |
| TV3 | Auth/session/RBAC, helper permission/audit/activity, broker controller và UI Users/Profile | Dùng nền của TV2/TV4; xem [TV3.md](TV3.md) |
| TV4 | React app/router/API client, dashboard mock; alerts/worker; môi trường triển khai | TV3 Login/Users/Profile; TV2 query/models |

## Việc cần chốt trước khi code song song

- [ ] TV2 + TV3: model IAM và thiết bị dùng chung; thứ tự migration pha A/B/C.
- [ ] TV2: app factory và DB object chung để TV3 đăng ký Blueprint.
- [ ] TV2: device fixtures P0, phân biệt REAL/SIMULATED, fixture ngoài scope.
- [ ] TV3: chữ ký helper quyền, audit/activity, broker controller.
- [ ] TV4 + TV3: router, auth state, API client và refresh single-flight.
- [ ] TV1 + TV2: mẫu MQTT đúng v3 và config canonical/hash.
- [ ] TV4 + TV2 + TV3: môi trường dev và TLS bàn giao; khóa phiên bản sau build thử.

## Thứ tự ưu tiên

P0 một controller và toàn bộ web/backend/bảo mật/tài liệu trước. P1 RPM và P2 AI không chặn P0. Chưa làm P1 ghi `P1_DEFERRED`; ca P0 chưa chạy ghi `NOT_RUN`.
