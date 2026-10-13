# Bài 5: Chẩn đoán và khắc phục sự cố CI/CD

## 1. Mục tiêu

Phân tích và sửa ba lỗi trong GitHub Actions workflow để khôi phục pipeline CI/CD.

## 2. Lỗi 1: Permission Denied

**Nguyên nhân:** Gradle Wrapper không có quyền thực thi trên GitHub Actions Runner Linux.

**Dấu hiệu:** Lệnh `./gradlew` thất bại với lỗi `Permission denied`.

**Khắc phục:**

```bash
chmod +x gradlew
./gradlew clean test bootJar
```

**Kết quả mong đợi:** Gradle Wrapper chạy được và các bước build, test hoàn thành.

## 3. Lỗi 2: Authentication Failed

**Nguyên nhân:** Tên biến Secret được tham chiếu trong workflow không khớp với tên Secret đã cấu hình trên GitHub, hoặc thông tin xác thực không hợp lệ.

**Khắc phục:**

- Kiểm tra tên Secret trong Settings → Secrets and variables → Actions.
- Sử dụng đúng `${{ secrets.REGISTRY_TOKEN }}` nếu Secret có tên `REGISTRY_TOKEN`.
- Kiểm tra username, token và quyền truy cập Docker Registry.

**Kết quả mong đợi:** Bước đăng nhập Docker Registry hoàn thành thành công.

## 4. Lỗi 3: Build Context Error

**Nguyên nhân:** Docker build action sử dụng sai thư mục context hoặc sai đường dẫn Dockerfile.

**Khắc phục:**

```yaml
with:
  context: .
  file: ./Dockerfile
  push: false
```

Hai đường dẫn trên áp dụng khi `Dockerfile` nằm ở thư mục gốc của repository.

**Kết quả mong đợi:** Docker tìm thấy Dockerfile và build image thành công.

## 5. Kiểm tra kết quả

- [ ] Gradle Wrapper chạy thành công.
- [ ] Docker Registry authentication thành công.
- [ ] Docker image build thành công.
- [ ] Workflow trên GitHub Actions chuyển trạng thái xanh.
- [ ] Đã chụp ảnh màn hình làm minh chứng.

## 6. Kết luận

Đã xác định nguyên nhân và áp dụng biện pháp khắc phục cho ba nhóm lỗi: quyền thực thi, xác thực Docker Registry và đường dẫn Docker build context. Kết quả cuối cùng được xác nhận bằng log chạy thực tế trên GitHub Actions.
