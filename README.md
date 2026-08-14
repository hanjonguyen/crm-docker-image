# crm-docker-image

Đúc image Docker cho **Frappe CRM** (chạy trên Frappe Framework **version-15**).

## Vì sao phải tự đúc?

Image chính chủ `ghcr.io/frappe/crm` **rỗng ruột** — chỉ có framework, thiếu app `crm`.
Nguyên nhân: workflow của repo `frappe/crm` truyền danh sách app qua
`--build-arg APPS_JSON_BASE64`, trong khi `images/layered/Containerfile` của
`frappe_docker` **chỉ đọc qua BuildKit secret `apps_json`**:

```
RUN --mount=type=secret,id=apps_json,target=/opt/frappe/apps.json,uid=1000,gid=1000
```

Kho này sửa đúng chỗ đó: truyền `apps.json` qua `secret-files`.
(Cùng một lỗi và cùng cách sửa như kho `lms-docker-image`.)

## Dùng

Image: `ghcr.io/hanjonguyen/crm-docker-image:v1`

Bước cuối của workflow **tự kiểm chứng** image có thật sự chứa app `crm`
trước khi coi là build thành công.
