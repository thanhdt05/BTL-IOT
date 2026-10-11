# Quy tắc làm việc nhóm

## Nhánh và pull request

- `main`: nền chung và code đã được nhóm review.
- Nhánh công việc: `feature/tv1-...`, `feature/tv2-...`, `feature/tv3-...`, `feature/tv4-...`; sửa lỗi dùng `fix/...`.
- Mỗi người commit bằng tài khoản Git của mình, mở PR vào `main`, nhờ một thành viên khác review.
- Commit đặt tên ngắn, ví dụ `feat(auth): add login endpoint` hoặc `docs(tv3): update session design`.
- Cập nhật nhánh theo `main` trước khi mở PR. Không force-push `main`.

## Ranh giới tích hợp

- Một Flask app factory, một `db/session`, một hệ thống migration, một React app. TV3 không tạo backend/DB/frontend riêng.
- TV2 phối hợp TV3 review schema IAM và thứ tự migration. P0 có 27 bảng đích theo mục 6 và 12.5; commit nền chưa tạo bảng nào.
- TV2 cung cấp device fixtures `cabinet-001` và `sim-unassigned` để TV3 test scope; monitor thuộc P1.
- API `/api/v1`, JSON snake_case, timestamp UTC. MQTT topic/payload dùng v3 theo đặc tả.
- Thay API/schema/MQTT/quyền phải cập nhật tài liệu và thống nhất với thành viên sử dụng trước khi merge.
- TV3 phối hợp TV4 về Login/Users/Profile; dùng chung router, API client và cơ chế refresh.
- MQTT worker và alert worker chạy riêng, mỗi loại một instance; không khởi chạy subscriber trong Flask app factory.

## Trước khi mở PR

1. Ghi rõ chức năng thay đổi và cách người khác chạy/kiểm tra.
2. Kiểm thử đúng phần thay đổi. Nếu chưa chạy, ghi `NOT_RUN` và lý do; không ghi PASS khi chỉ có tài liệu.
3. Đánh dấu mock/simulated/real đúng nguồn; HTTP 202 là ACCEPTED, chưa phải APPLIED.
4. Kiểm tra không có `.env`, secret, token, mật khẩu, broker credential, khóa CA hoặc dữ liệu cá nhân thật.
5. Với thay đổi contract/migration, nhờ TV2/TV3/TV4 liên quan review.

Branch protection chưa được thiết lập tự động; đây là quy ước làm việc của nhóm.
