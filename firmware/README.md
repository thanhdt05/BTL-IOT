# Firmware — TV1 — TODO

P0 CONTROLLER: ESP32 + DHT22 + relay + quạt DC, AUTO/safety, Wi-Fi/MQTT telemetry/status/ACK v3, remote config và MANUAL TTL.

TV1 tạo firmware C/C++ Arduino ESP32, ghi phiên bản core/library sau build thử; dùng `common/` cho module dùng chung và `controller/` cho profile P0. P1 FAN_MONITOR chỉ thêm sau P0.

Chưa có firmware. Credential cấu hình cục bộ, không commit Wi-Fi/MQTT password; TLS kiểm CA/hostname theo đặc tả.
