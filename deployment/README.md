# Deployment — TV4 phối hợp TV2/TV3 — TODO

Mục tiêu Compose: PostgreSQL 16, Mosquitto Dynamic Security, backend API, MQTT worker, alert worker và reverse proxy HTTPS.

Hiện chưa có Compose/Dockerfile/cấu hình broker/proxy/TLS. TV4 thêm cấu hình sau khi app nền chạy; TV3 phối hợp broker ACL, credential, CA/cookie/Origin.

Khóa image tag sau kiểm thử; không dùng `latest` ở bản bàn giao. Không publish DB ra mạng chung. Tách profile dev khỏi HTTPS/MQTTS bàn giao. `.env`, CA private key và broker runtime không commit.
