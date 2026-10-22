# cn012-web-cua-hang-may-tre-la
# TUẦN 1: KHỞI TẠO DỰ ÁN VÀ CẬP NHẬT ĐỀ CƯƠNG CN-012
### Đề cương đề tài: Website bán hàng có gợi ý sản phẩm (CN-012)
- **Đơn vị áp dụng:** Cửa hàng Mây Tre Lá & Dụng cụ câu cá Phát Tài.
- **Mục tiêu:** Xây dựng website bán hàng trực tuyến hỗ trợ đặt hàng, quản lý sản phẩm, đơn hàng, khuyến mãi và tích hợp module gợi ý sản phẩm ("Sản phẩm mua kèm", "Có thể bạn sẽ thích") nhằm cải thiện doanh số ngày thường.
- **Công nghệ dự kiến:** React, TypeScript, Python (FastAPI), PostgreSQL, Content-Based Filtering.
  
# TUẦN 2: PHÂN TÍCH ĐỀ TÀI, QUY TRÌNH NGHIỆP VỤ & SƠ ĐỒ TỔNG QUÁT 

## 1. Phân tích đề tài & Xác định yêu cầu
* **Đặc thù bài toán tại Cửa hàng Mây Tre Lá & Dụng cụ/Mồi câu cá Phát Tài:**
  - Cửa hàng kinh doanh 2 nhóm hàng chính: Đồ thủ công mây tre lá (mẹt tre, rổ, quang gánh, giường tre 2m...) và Dụng cụ/Mồi câu cá (cần câu, phao, lưỡi câu, cám, trùng, dế...).
  - Các mặt hàng hết xu hướng (hết trend) sẽ được ngừng nhập và xóa khỏi danh mục bán để tinh gọn kho. Các mặt hàng kích thước lớn (như giường tre 2m) hoặc đơn đi xa cần có bước xác nhận 2 bên và chuyển khoản/đặt cọc trước khi giao để tránh rủi ro hoàn đơn (bom hàng).
* **Yêu cầu chức năng cốt lõi:**
  1. **Khách hàng:** Đăng ký/Đăng nhập (lưu Họ tên, SĐT, Địa chỉ nhận hàng); Duyệt danh mục, Tìm kiếm & Lọc sản phẩm; Xem chi tiết sản phẩm kèm danh sách gợi ý *"Sản phẩm mua kèm"* và *"Có thể bạn sẽ thích"*; Quản lý giỏ hàng; Đặt hàng & Thanh toán (Tiền mặt COD hoặc Chuyển khoản ngân hàng); Đánh giá sản phẩm.
  2. **Quản trị viên (Admin):** Quản lý danh mục (CRUD); Quản lý sản phẩm (CRUD, tải ảnh, xóa mặt hàng ngừng bán); Quản lý khuyến mãi cơ bản (giảm giá trực tiếp mặt hàng bán chậm hoặc giảm tiền cố định cho đơn mua nhiều nguyên giá); Quản lý đơn hàng (liên hệ xác nhận với khách và cập nhật trạng thái đơn); Xem thống kê doanh thu.
  3. **Hệ thống Gợi ý (Recommendation Module):** Tự động lưu vết lịch sử tương tác (xem, tìm kiếm, giỏ hàng, mua hàng) và sử dụng thuật toán *Content-Based Filtering* để đề xuất sản phẩm tương đồng.
* **Định hướng phát triển nâng cao (Future Scope):** Xây dựng hệ thống chương trình khuyến mãi chuyên sâu quy mô lớn (voucher đa tầng, săn sale theo khung giờ, phân hạng thành viên như các sàn TMĐT Shopee) sau khi hoàn thiện hệ thống cốt lõi.

---

## 2. Quy trình nghiệp vụ cốt lõi

### a. Quy trình Đặt hàng & Xác nhận 2 bên (Chống bom hàng)
1. **Khách hàng** đăng nhập, chọn sản phẩm vào giỏ hàng, kiểm tra ưu đãi tự động (nếu đủ điều kiện đơn hàng) và chọn phương thức thanh toán (*Chuyển khoản ngân hàng* hoặc *COD*) rồi gửi yêu cầu Đặt hàng.
2. **Hệ thống** ghi nhận đơn hàng ở trạng thái **"Chờ xác nhận"**.
3. **Chủ cửa hàng (Admin)** kiểm tra đơn mới, chủ động liên hệ qua Số điện thoại của khách để **xác nhận đồng thuận 2 bên** (chốt tình trạng hàng hóa, phương thức vận chuyển đối với hàng cồng kềnh/mồi tươi sống và kiểm tra giao dịch chuyển khoản/cọc).
4. Sau khi hai bên thống nhất, **Admin** chuyển trạng thái đơn sang **"Đang giao hàng"** --> **"Hoàn thành"** (hoặc **"Hủy đơn"** nếu khách đổi ý).

### b. Quy trình Thu thập hành vi & Gợi ý sản phẩm (Content-Based Filtering)
1. Khi **Khách hàng** xem chi tiết sản phẩm, tìm kiếm hoặc thêm vào giỏ hàng, hệ thống tự động ghi nhận sự kiện vào bảng lịch sử hành vi (`user_behavior_logs`).
2. **Module Gợi ý** phân tích từ khóa nội dung (Tên, Danh mục, Mô tả, Tags) của các sản phẩm đang bán để trích xuất danh sách **"Sản phẩm mua kèm"** (cùng nhóm công dụng) và **"Có thể bạn sẽ thích"** (dựa trên lịch sử xem gần đây) hiển thị lên giao diện.

---
