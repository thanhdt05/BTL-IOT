# Contract tham chiếu

Nguồn chuẩn: [đặc tả V1.3](../IOT_TU_THIET_BI_DAC_TA_CHUNG_V1_3.md). File này chỉ là chỉ mục để nhóm tra cứu; chưa tạo contract triển khai riêng.

| Nội dung | Mục đặc tả |
| --- | --- |
| Roles, permission, scope và session | 5.1–5.3 |
| PostgreSQL/schema/migration/seed | 6 và 12.5 |
| REST `/api/v1`, response, pagination, idempotency | 7 |
| MQTT v3 topic/payload/QoS/LWT/commands/ACK | 8 |
| Config canonical JSON và SHA-256 fixtures | 8.5 |
| Lifecycle, alerts và UI | 9 |
| Phân công và điểm bàn giao | 10 |
| Demo và nghiệm thu | 12 |
| Threat model và ca kiểm thử | 15 |

Quy ước tối thiểu: JSON snake_case, UTC ISO8601; success `{ "data": ... }`, list thêm `meta`, error `{ "error": { "code": "...", "message": "...", "request_id": "..." } }`.

MQTT prefix: `cabinet/v3/devices/{device_id}`, suffix `telemetry`, `status`, `commands`, `acks`; `schema_version=3`. Ví dụ P0: `cabinet-001` CONTROLLER, `fan_binding_id=null`.

HTTP 202 chỉ xác nhận ACCEPTED. UI hiển thị APPLIED sau ACK match từ thiết bị. P0 chỉ biết đầu ra relay, chưa chứng minh quạt đang quay.
