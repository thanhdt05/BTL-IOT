# Worker — TODO

- TV2 tạo `mqtt_worker.py`: ingest/status/ACK, dispatch/outbox và reconciliation.
- TV4 tạo `alert_worker.py`: đánh giá rule/timer/offline/stale.

Mỗi loại một process riêng; không khởi chạy subscriber trong Flask app factory. Hiện chưa có worker hoặc heartbeat/readiness.
