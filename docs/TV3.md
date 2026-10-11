# TV3 — Tài khoản và bảo mật

**Trạng thái: kế hoạch và vị trí file; chưa triển khai.** Người dùng repo hiện phụ trách TV3.

Phạm vi lấy từ đặc tả V1.3: mục 5, phần IAM/audit/activity ở mục 6, API mục 7.1, phân công mục 10 và kiểm thử mục 15.3.

## Vị trí làm việc

| Phần | Vị trí | Ghi chú |
| --- | --- | --- |
| Auth, phiên, refresh, re-auth, password | `backend/app/auth/` | Blueprint trên app chung TV2 |
| Users, role catalog, quản trị phiên | `backend/app/users/` | Không public registration |
| Permission và scope | `backend/app/permissions/` | TV2/TV4 dùng cùng helper |
| Audit bảo mật | `backend/app/audit/` | Admin-only, không chứa secret |
| Nhật ký chức năng | `backend/app/user_activity/` | Admin toàn hệ thống; user chỉ bản thân |
| Broker controller/ACL | `backend/app/broker_operations/` | TV3 làm interface; TV2 gọi trong durable operations |
| Model/migration IAM | `backend/app/models/`, `backend/migrations/` | Phối hợp TV2, không tạo nguồn schema thứ hai |
| UI Users/Profile | `frontend/src/pages/users/`, `frontend/src/pages/profile/` | App/router chung TV4; Login phối hợp TV3/TV4 |
| Kiểm thử | `tests/auth/`, `tests/security/` | Dùng Flask test client và PostgreSQL chung |

## Thứ tự triển khai trên nhánh TV3

- [ ] Nhận app factory/DB/migration nền từ TV2 và router/API client từ TV4.
- [ ] Chốt model IAM với TV2: users, roles, permissions, role_permissions, user_roles, auth_sessions, refresh_tokens, login_attempts; phối hợp device_access/audit_logs/user_activity_logs.
- [ ] Seed ADMIN/OPERATOR/VIEWER, permission catalog; bootstrap admin một lần từ cấu hình cục bộ, không mật khẩu mặc định.
- [ ] Auth: login/me/logout/change-password; Argon2id, access JWT, opaque refresh/session.
- [ ] Rotation/reuse revoke, signed CSRF + Origin, re-auth, idle/absolute timeout, revoke/lock/token_version, giới hạn session.
- [ ] Permission/scope helpers, truy vấn SQL theo scope, rate limit bền vững, chống mất admin ACTIVE cuối cùng.
- [ ] Users/role/password reset/session endpoints, audit và nhật ký có redaction.
- [ ] Dynamic Security: credential riêng, client_id binding, exact-topic ACL, disable/disconnect/reconnect denial; phối hợp TV2 lifecycle.
- [ ] Users/Profile UI và Login phối hợp TV4; access token trong memory, refresh single-flight.
- [ ] Chạy kiểm thử auth/scope/session/ACL/log và ghi bằng chứng theo đặc tả.

## Interface cần bàn giao cho nhóm

Đây là tên interface đã nêu trong đặc tả, chưa có implementation:

- `PermissionService.require(permission, device_id=None)`.
- `visible_devices_query(user)` cho list/count/statistics/export có SQL scope.
- Helper audit/activity dùng cùng transaction của nghiệp vụ; API cụ thể thống nhất TV2/TV4 trước khi viết.
- Broker controller: `provision_client`, `enable_client`, `disable_client`; chữ ký/kiểu kết quả phối hợp TV2.

Các helper kiểm quyền phải default deny cho quyền không tồn tại, user/session không hợp lệ hoặc scope thiếu. Role permission và device scope là hai điều kiện riêng. Ownership/location/group không tự cấp quyền.

## Quy tắc bảo mật cần giữ đúng

- Một role hệ thống/user qua `user_roles`, không thêm `users.role` làm nguồn quyền thứ hai.
- Access JWT 15 phút; refresh opaque ≥32 byte, DB chỉ lưu SHA-256 digest; session tuyệt đối 8 giờ, idle 30 phút theo cấu hình đặc tả.
- Access trong memory; refresh cookie host-only HttpOnly/Secure/SameSite=Strict; signed CSRF ràng buộc session và Origin allowlist.
- Mỗi API bảo vệ kiểm DB user/session/token_version/quyền hiện tại. Không tin role claim cũ.
- Ngoài device scope trả 404; có VIEW nhưng thiếu quyền hành động trả 403; thiếu/sai/revoked credential trả 401.
- Không trả password_hash/refresh/broker secret ở API đọc; không log password/token/cookie/Authorization/raw login body.
- Audit và activity append-only; `ACCEPTED` khác `SUCCESS`, HTTP 202 khác ACK `APPLIED`.
- Giữ ít nhất một admin ACTIVE trong transaction khi khóa/hạ quyền; password reset/role/credential/revoke nhạy cảm có re-auth.

## Kiểm thử bàn giao TV3

Mọi ca hiện `NOT_RUN`. Dùng ID gốc để nối bằng chứng với đặc tả:

| Nhóm ca | Nội dung |
| --- | --- |
| IAM-01, IAM-02 | Viewer không control; không grant `sim-unassigned` thì không đọc/list/count/export ngoài scope |
| IAM-03–IAM-05 | Revoke/lock/password change, rotating refresh/reuse/concurrency, signed CSRF/Origin |
| IAM-06, IAM-07 | Không mất admin cuối khi concurrent; rate limit tồn tại sau restart |
| MQTT-01, MQTT-02 | Exact ACL/client_id, revoke đang kết nối và broker reconciliation |
| LOG-01–LOG-03 | Nhật ký đúng actor/outcome/redaction/self scope, không log mỗi tick polling |

Tạo nhánh `feature/tv3-auth` từ `main`; mở PR từng phần có cách kiểm tra. Bộ khung `main` hiện chỉ là nơi thống nhất cấu trúc, chưa hoàn thành ca nào.
