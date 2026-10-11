# Kiểm thử — TODO

Hiện chưa có test và chưa có ca PASS. Mọi ca nghiệp vụ là `NOT_RUN`; tham chiếu mục 12 và 15.3 của đặc tả.

- `auth/`: TV3, login/session/refresh/CSRF/password/revoke/rate limit/last admin.
- `security/`: TV3 phối hợp TV2, RBAC/device scope/ACL/revoke/redaction.
- `integration/`: nhóm, PostgreSQL/migrations/API/MQTT/alerts; không thay DB tích hợp bằng SQLite.

Mỗi ca ghi precondition, bước, expected/actual, người/ngày/commit và bằng chứng. TV4 tổng hợp; simulator ghi SIMULATED, ca P1 hoãn ghi P1_DEFERRED.
