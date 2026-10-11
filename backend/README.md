# Backend chung — TODO

TV2 tạo Flask app factory, `extensions.py` dùng chung SQLAlchemy/Flask-Migrate/JWT và đăng ký Blueprints. TV3 thêm auth/users/permissions/audit/activity/broker helpers. TV4 thêm alerts.

Thư mục theo mục 10.1 của đặc tả; hiện chỉ có chỗ trống, chưa có app/endpoint/model/migration. TV2 thêm `requirements.txt` với phiên bản đã kiểm thử và hướng dẫn chạy khi dựng nền.

Migration thực đặt ở `migrations/`; P0 đích 16 bảng pha A → 24 pha B → 27 pha C. TV2/TV3 cùng review, không tạo DB auth riêng. `extensions_optional/` dành cho P1/P2/OTA sau P0.
