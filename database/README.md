# Database — TODO

TV2 phối hợp TV3 làm ERD, data dictionary, fixture/seed và hướng dẫn backup/restore. PostgreSQL 16, database `iot_cabinet`.

Schema thực do SQLAlchemy + migrations trong `backend/migrations/` quản lý; không giữ một schema SQL độc lập mâu thuẫn. P0 đích 27 bảng theo đặc tả; bộ khung hiện chưa tạo bảng.

Fixtures đầu tiên: `cabinet-001` (REAL) và `sim-unassigned` (SIMULATED), dùng để test scope. TV3 phối hợp seed roles/permissions/bootstrap admin; không có tài khoản/mật khẩu mặc định.
