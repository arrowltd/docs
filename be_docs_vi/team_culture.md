# Tổng quan

- Team BE là riêng biệt, những gì liên quan tới văn hoá cách làm việc của team BE có thể không giống với team khác. Kệ. Nếu ở team BE thì theo văn hoá team BE.
- Mục đích cuối cùng của bộ docs, coding convention, văn hoá team, là để làm việc được hiệu quả, trách lỗi lầm làm công ty sập và ngưng chạy. Khi đã ok những việc mình làm ko nguy hiểm để công ty sập, thì sẽ đi tới việc mình làm có đóng góp cho công ty không.
- Công ty chúng ta có sản phẩm riêng, không giống công ty khác, từ sản phẩm đó dẫn tới những hệ quả về quản lý, cách làm việc, cách bảo trì bảo dưỡng, dẫn tới bộ docs này. Không có cái gì trong docs này là tự nhiên xuất hiện mà không có nguyên do.
- Quan trọng nhất là những gì mình làm ra (theo thứ tự quan trọng nhất trước):
	* Dễ đọc, sửa nhanh, không phức tạp.
	* Hạn chế tối đa bug ngay từ lúc viết code.
	* Code chạy được, làm đúng chức năng nó cần làm.
	* Dễ test
	* Performance của code tốt.
- Giao tiếp là cái quan trọng, giao tiếp để code, chứ không phải code rồi giao tiếp.

# Vai vế trong team

- Tech Lead là người cao nhất, có quyết định sau cùng
  * Được quyền đóng góp ý kiến, bảo là cái này sai nè cái này đúng nè cái này nên làm vầy nè
  * Nhưng nếu bạn và Tech Lead đã thống nhất là vấn đề hiện tại không có sai lệch về giao tiếp, Tech Lead đúng là muốn A, bạn muốn B, Tech Lead bảo không, làm A, không làm B, vậy thì bạn làm A.
  * Việc này đồng nghĩa với việc Tech Lead là người chịu trách nhiệm cuối cùng. Chứ không phải bạn

- Team BE có chia nhỏ theo "bộ"
  * Một bộ bao gồm "Lead" và "Members".
  * Lead có quyết định sau cùng, trên Members. Việc này đồng nghĩa với việc Lead chịu trách nhiệm cho cái Members của Lead làm.
  * Nếu người khác làm việc chung với Member, vả cần feedback về Member đó, thì feedback với Lead.
  * Tech Lead cũng là Lead của một bộ, bộ này bao gồm Members của Tech Lead và Lead của tất cả bộ khác.
  * Tech Lead ko làm việc, đánh giá trực tiếp Members của bộ khác.
  * Members đưa code cho Lead review. Khi task/feature đã ổn, bắt đầu đưa cho QC được, thì Lead đưa code cho Tech Lead review cuối để chốt.

# Công ty và sản phẩm

Những đặc thù và hệ luỵ của sản phẩm mà chúng ta làm là
- 1. Transaction là tiền thật, sai sót, bug, crash là mất tiền. Như sản phẩm ngân hàng. Hệ luỵ:
  * **Khi có bug/problem/incidents, cần sửa code/fix bug/thêm code an toàn nhất và nhanh nhất có thể.**
- 2. Khó tìm/tuyển người. Những người có thâm niên và giỏi thì có nhiều lựa chọn, vậy họ sẽ không chọn công ty, sản phẩm của mình. Những người còn mới, muốn học hỏi, khi học hỏi đầy đủ rồi sẽ có nhiều lựa chọn, vậy họ sẽ chọn không ở lại công ty mình. Hệ luỵ:
  * Team mình bao gồm người mới, beginner level nhiều.
  * Code mình làm vào, sửa vào, là của người đã nghỉ.

# Coding Convention

- Đây không phải là "khuyến khích làm theo", mà là phải theo.
- Bất kì bất đồng, ko chắc ăn là vầy đúng không, mang đi hỏi Lead của bạn.
- Phải theo ko có nghĩa là Members làm sai là lỗi của Members, Lead phải tìm hiểu tại sao lại sai
- Những cái sai là lỗi của Members:
  * Không hiểu, không chắc ăn nhưng không hỏi, mà làm đại.
  * Không check đủ theo checklist, mà đã đưa code cho Lead review.
  * "mình thấy làm vầy cũng được" => sai, đây không phải là khuyến khích làm theo, mà là phải theo. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
  * "ở công ty cũ của mình toàn làm vậy" => sai, đây không phải là công ty cũ của bạn. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
  * "đây là cách viết theo coding convention của công ty cũ/thầy/bạn etc" => sai, đây không phải là công ty cũ của bạn, Lead hiện tại cũng không phải là thầy/bạn của bạn. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
  * "mình đọc trên mạng thấy viết vầy tốt hơn" => sai. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.

# Review code
- Không đợi tới lúc review code, bị hỏi tại sao làm vậy mới bắt đầu hỏi ngược lại làm vậy được không.
- Người review code (Lead, Tech Lead) không phải thánh thần, cũng không phải người không bao giờ sai, cũng không phải Developers 5 sao vô địch hoàn hảo. Vì vậy nên:
  * Cái gì nó lạ, thì note lại cho người review code biết, để xem chỗ đó kĩ hơn, đảm bảo không bị thiếu sót.
  * Review code vẫn có thể có bất đồng, coding convention sẽ được update liên tục, nhưng không tránh khỏi lỗ hổng sai sót, lúc đó thì bàn bạc thảo luận. Nếu vẫn không nhất trí được với nhau thì đưa lên Tech Lead hỏi.

