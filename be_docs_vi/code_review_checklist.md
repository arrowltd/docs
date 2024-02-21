# Checklist chính

- **Đọc lại code mình viết**
  * Sourcetree hay những app tương tự sẽ xem được commit này đụng tới những files nào, đã thêm xoá sửa code gì rất rõ ràng
- Code đã theo coding convention
- Code đã theo defensive coding
- function bị xoá, logic bị xoá, có thật sự được xoá hay vô tình xoá, đã tìm hiểu kĩ chưa
- comment out là để note cho người sau đọc hiểu về code hiện tại, hay là comment out code cần phải xoá, để cho chạy
- code in log ra có cần giữ ko, hay chỉ cần lúc làm
- nếu có copy code, đã rename lại cho đúng variable, function chưa
- Code không xài nữa đã xoá chưa hay chỉ mới comment out
- `defer resp.Body.Close()` sau khi gọi api và đọc response của http request.

# Checklist cho API

- Làm API trong tâm thế ai cũng có thể dùng POSTMAN, curl, etc để gọi api của mình.
  * Validate hết những input cần validate (không quan tâm FE có chặn chưa)
  * Uppercase/lowercase input nếu logic yêu cầu (không quan tâm FE có uppercase/lowercase chưa)
- Có đang dùng lại struct params của API khác không? Nếu dùng lại thì có cần đổi tên struct cho hợp lý không
- Có đang query database quá nhiều không
  * query lấy ra list data, rồi loop qua query từng dòng trong list
  * nhiều hơn 50 queries trong một API
  * trả về nhiều hơn 10000 rows
  * nếu cần thiết phải làm như trên, đã thông báo với Lead chưa

# Checklist cho Database

- Chắc chắn là migration sql chạy dưới 1 phút
  * Nếu trên 1 phút, cần thông báo với Lead
- Chắc chắn là migration data chạy dưới 1 phút
  * Nếu trên 1 phút, cần chia nhỏ chạy lần lượt từng cụm 1000/10000 dòng.
  * Đã làm chức năng stop/resume migration data nặng
  * In ra mỗi lần chạy một cụm 1000/10000 dòng mất bao lâu
- Đã nắm rõ việc migration có ảnh hưởng tới live system hay không
  * Nếu có ảnh hưởng, đã có kế hoạch tính toán giải quyết vấn đề này chưa
- Đã thông báo với Lead về việc cần maintenance hay không
- rows từ Query function đã defer rows.Close()
- Cột mới data type không có `INT, INT32, FLOAT, DOUBLE, VARCHAR`
- Tạo index cho cột khi query cần `WHERE`
- Index tạo cho table cũ cần phải là migration data, dùng `CREATE INDEX CONCURRENTLY IF NOT EXISTS`
