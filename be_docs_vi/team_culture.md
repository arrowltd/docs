# Tổng quan

- Team BE là riêng biệt, những gì liên quan tới văn hoá cách làm việc của team BE có thể không giống với team khác. Kệ. Nếu ở team BE thì theo văn hoá team BE.
- Mục đích cuối cùng của bộ docs, coding convention, văn hoá team, là để làm việc được hiệu quả, trách lỗi lầm làm công ty sập và ngưng chạy. Khi đã ok những việc mình làm ko nguy hiểm để công ty sập, thì sẽ đi tới việc mình làm có đóng góp cho công ty không.
- Công ty chúng ta có sản phẩm riêng, không giống công ty khác, từ sản phẩm đó dẫn tới những hệ quả về quản lý, cách làm việc, cách bảo trì bảo dưỡng, dẫn tới bộ docs này. Không có cái gì trong docs này là tự nhiên xuất hiện mà không có nguyên do.
- Quan trọng nhất là những gì mình làm ra (theo thứ tự quan trọng nhất trước):
	* 1. Code dễ đọc, sửa nhanh, không phức tạp.
	* 2. Hạn chế tối đa bug ngay từ lúc viết code.
	* 3. Code chạy được, làm đúng chức năng nó cần làm.
	* 4. Dễ test
	* 5. Performance của code tốt.
- Giao tiếp là cái quan trọng, giao tiếp để code, chứ không phải code rồi giao tiếp.
- Công việc của bạn được đánh giá dựa trên
  * 1. Code của bạn ở production tốt, khi cần sửa nhanh, đọc hiểu, người khác làm vào cũng trơn tru không vấn đề.
  * 2. Bạn làm theo đúng checklist, coding convention, guideline, giao tiếp tốt.
  * 2. Khối lượng công việc bạn làm được.
- Đây là công việc có trả lương, và có benefit. Nó như bao công việc khác, không phải cái gì cũng hoàn hảo, hay theo ý bạn. Mỗi lần cảm thấy không ổn, hãy so sánh giữa benefit và những cái bạn phải chịu. Cụm từ "Ráng chịu thôi" sẽ thường xuyên xuất hiện trong guideline này. Không chịu được thì nghỉ vậy. Việc bạn nghỉ thường không phải là bạn sai, hay công ty sai, mà chỉ là bạn không phù hợp thôi.

# Vai vế trong team

- Tech Lead là người cao nhất, có quyết định sau cùng
  * Được quyền đóng góp ý kiến, bảo là cái này sai nè cái này đúng nè cái này nên làm vầy nè
  * Nhưng nếu bạn và Tech Lead đã thống nhất là vấn đề hiện tại không có sai lệch về giao tiếp, Tech Lead đúng là muốn A, bạn muốn B, Tech Lead bảo không, phải làm A, không làm B, vậy thì bạn làm A.
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
- 1. Transaction là tiền thật, sai sót, bug, crash là mất tiền. Như sản phẩm ngân hàng. Lượng khách hàng và dòng tiền hiện tại cực kì lớn. Hệ luỵ:
  * **Khi có bug/problem/incidents, cần sửa code/fix bug/thêm code an toàn nhất và nhanh nhất có thể.**
- 2. Khó tìm/tuyển người. Những người có thâm niên và giỏi thì có nhiều lựa chọn, vậy họ sẽ không chọn công ty, sản phẩm của mình. Những người còn mới, muốn học hỏi, khi học hỏi đầy đủ rồi sẽ có nhiều lựa chọn, vậy họ sẽ chọn không ở lại công ty mình. Hệ luỵ:
  * Team mình bao gồm người mới, beginner level nhiều.
  * Code mình làm vào, sửa vào, là của người đã nghỉ.
- 3. Khách hàng (người đưa requirement), là một nhóm nhiều người, và thiếu chuyên nghiệp. Hệ luỵ:
  * Logic lan man chồng chéo
  * Có những feature thật ra đã có rồi, chỉ cần sửa lại thêm điều kiện, làm thêm vào feature cũ nhưng họ ko chịu, ko hiểu feature cũ, ko muốn xài feature đó vì là của khách hàng khác, làm cho có 2 features na ná nhau nhưng thật ra là cùng 1 dạng.
  * Họ thích requirement cực kì phức tạp, vì họ cần tới đâu nghĩ tới đó, ko nghĩ xong rồi dừng lại xem lại tổng thể để lượt bớt.
Từ những hệ luỵ trên chúng ta suy ra được cái quan trọng nhất khi làm ở đây là:
  * 1. Code dễ đọc, khi cần hotfix thì sửa nhanh. Code không phức tạp, ai cũng đọc hiểu và sửa được.

# Coding Convention

- Đây không phải là "khuyến khích làm theo", mà là phải theo.
- Bất kì bất đồng, ko chắc ăn là vầy đúng không, mang đi hỏi Lead của bạn.
- Phải theo ko có nghĩa là Members làm sai là lỗi của Members, Lead phải tìm hiểu tại sao lại sai, và hướng dẫn, đảm bảo hoặc ít ra hướng tới sau này ko lặp lại lỗi sai như vậy.
- Những cái sai là lỗi của Members:
  * Không hiểu, không chắc ăn nhưng không hỏi, mà làm đại.
  * Không check đủ theo checklist, mà đã đưa code cho Lead review.
  * lúc review nhận feedback thì nói (chú ý lúc làm mà hỏi, nói, giao tiếp với lead về những vấn đề dưới đây thì ko phải là lỗi):
    - "mình thấy làm vầy cũng được" => sai, đây không phải là khuyến khích làm theo, mà là phải theo. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
    - "ở công ty cũ của mình toàn làm vậy" => sai, đây không phải là công ty cũ của bạn. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
    - "đây là cách viết theo coding convention của công ty cũ/thầy/bạn etc" => sai, đây không phải là công ty cũ của bạn, Lead hiện tại cũng không phải là thầy/bạn của bạn. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
    - "mình đọc trên mạng thấy viết vầy tốt hơn" => sai. Nếu có bất đồng, nếu bạn tin đây là cách viết tốt hơn, thì phải mang đi hỏi, không đợi tới lúc review code.
    - "code chạy mà" => sai. Như phần tổng quát, code chạy được đứng vị trí thứ ba, không phải thứ nhất.
    - "mình thấy code cũ viết vậy" => sai. Code cũ có thể sai coding convention, vì coding convention được update liên tục, code cũ ko được update liên tục, vì update code cũ đồng nghĩa với việc test lại, kiểm tra lại hết. Ở đây việc giữ code cũ dù sai coding convention, nhưng nó đã chạy và đảm bảo tính ổn định rồi quan trọng hơn là viết lại cho đúng coding convention. Code mới thì phải đúng coding convention.

# Review code

- Có checklist cho việc review code. Dùng nó cho cả lúc review và lúc trước khi đưa code của bạn cho người khác review.
- Không đợi tới lúc review code, bị hỏi tại sao làm vậy mới bắt đầu hỏi ngược lại làm vậy được không.
- Người review code (Lead, Tech Lead) không phải thánh thần, cũng không phải người không bao giờ sai, cũng không phải Developers 5 sao vô địch hoàn hảo. Vì vậy nên:
  * Cái gì nó lạ, thì note lại cho người review code biết, để xem chỗ đó kĩ hơn, đảm bảo không bị thiếu sót.
  * Việc giúp đỡ người review code ko bị thiếu hụt miss một vấn đề nào trong code của bạn cũng là nghĩa vụ của bạn.
  * Review code vẫn có thể có bất đồng, coding convention sẽ được update liên tục, nhưng không tránh khỏi lỗ hổng sai sót, lúc đó thì bàn bạc thảo luận. Nếu vẫn không nhất trí được với nhau thì đưa lên Tech Lead hỏi. Tech Lead là người có quyết định sau cùng.
- Review code ko phải là test, code sau khi review kĩ nhất có thể không có nghĩa là code đúng về logic
  * Người nắm logic kĩ nhất của feature/code đang được review là người code, không phải người review
  * Dù vậy không phải review code là không review logic, chỗ nào lạ đều cần bàn bạc trao đổi

# Giao tiếp

- Cách giao tiếp này áp dụng cho việc giao tiếp trong team BE, và giao tiếp ngoài team BE.
- Có checklist cho việc giao tiếp.
- Chú trọng giao tiếp, ko biết ko hiểu ko chắc thì giao tiếp rồi code, ko phải là code rồi mới giao tiếp.
  * Các bạn có thể thấy phiền vì phải nói/chat quá nhiều. Ráng chịu thôi. Khi quen rồi thích cách giao tiếp cách nói chuyện sẽ tốt hơn, tỉ lệ người nghe đọc hiểu sẽ tăng lên, tốt cho công việc và cho cuộc sống
- Ở công ty mình, việc giao tiếp (với Lead, với QC) chiếm thời gian nhiều nhất của bạn, chứ ko phải code.
  * Vì ở công ty mình là làm thêm vào cái cũ, là thêm feature cho cái cũ, sửa vào cái cũ. Nơi code nhiều hơn giao tiếp sẽ là ở công ty startup mới hình thành, ít người, viết code từng đầu.
- Vì giao tiếp chiếm thời gian nhiều nhất của bạn, không nên khó chịu khi phải giao tiếp quá nhiều. Luôn tìm cách để cái mình nói dễ hiểu, người nghe đọc hiểu nắm được trọn vẹn vấn đề trước khi tiến hành suy nghĩ về vấn đề.
- Hiểu được người bạn đang giao tiếp không phải là bạn, họ có quá khứ riêng tính cách riêng kỹ năng riêng, cái bạn hiểu nhanh ko có nghĩa là họ sẽ hiểu nhanh và ngược lại. Đôi khi cái kẹt trong cuộc giao tiếp ko nằm ở cái bạn đang ráng giải thích cho họ, mà là ở một vấn đề khác.
- Ai cũng có lúc không vui, khó chịu. Việc này ko nhất thiết phải đến từ vấn đề hiện tại đang giao tiếp, mà có thể là do tối qua thức khuya, vừa cãi nhau với ai ở nhà, có chuyện gì đó khác làm cho stress. Chúng ta sẽ hiểu và thông cảm. Đợi 5 phút, 15 phút, 1 ngày, rồi quay lại vấn đề hiện tại không phải là điều cấm kỵ. Cái quan trọng là 2 bên nắm rõ vấn đề đang cần giải quyết, hiểu được chính xác vấn đề thật sự nằm ở đâu, và cùng suy nghĩ để giải quyết nó. Nếu việc đó cần thời gian thì chúng ta cho thêm thời gian.
- Luôn nhớ rằng mình là con người, mình có thể sai. Chấp nhận việc mình có thể sai nghĩa là chấp nhập cái mình đang nói/cãi có thể sai
  * Ai cũng có thể sai, kể cả Tech Lead, Lead, người dang giao với bạn, bản thân bạn
  * Đôi khi cái sai không phải là cái đang nói, mà là hiểu sai ý từ trước đó
  * Nếu bạn đang trong trạng thái mình đúng hoàn toàn rồi, thì thiệt ra không nên giao tiếp nữa (giao tiếp nữa chi khi mình đúng rồi mà nhỉ). Đợi 1 lúc rồi nói chuyện tiếp.
- Khi bạn không vui, đang khó chịu, không nghĩ ra cái gì dễ chịu để nói (ai cũng vậy, chúng ta là con người, có cảm xúc, đây không phải là lỗi của bạn), một cách có thể làm là kêu bạn kia đợi chút, để check, hay kêu đợi tí đang bận, rồi đi đâu đó làm gì làm cho cái cảm xúc nó giảm bớt, thay đổi đi, rồi quay lại sau. Bình thường việc này chỉ mất từ 5-15 phút, nhưng hiệu quả sẽ rất rõ rệt
  * Sẽ không ai đánh giá performance của bạn khi bạn làm vậy. Luôn có thể đợi.
  * Cái không đợi được chỉ có hotfix. Lúc đó thì ráng chịu thôi, tập trung trong đầu là làm sao hotfix nhanh gọn lệ, kệ tất cả những cái khó chịu khác.

# Code

- Ở team BE, chúng ta không quan tâm code này trước đây là ai viết, code cũ này đẹp xấu sao. Tất cả code trong project là do team BE viết, dựa trên coding convention ở thời điểm đó. Code mới viết cũng theo coding convention, và cũng của tất cả mọi người.
- Việc này dựa trên việc tất cả code sẽ được review ít nhất 2 lần, từ Lead và Tech Lead.
- Việc này giúp cho một bạn có thể làm tất cả các feature, chạm vào vào sửa tất cả các code của tất cả các bạn khác (vì thiệt ra ko có code của bạn ở đây, đây là code của team BE).
- Việc này cũng có nghĩa là bạn sẽ phải đọc hiểu được tất cả code, của người khác hay của bạn. Không ngại đọc code không phải mình viết.
- Chúng ta quan tâm code cũ ai viết khi:
  * Sau khi xử lý xong vấn đề, cần tìm hiểu tại sao lỗi này lại xảy ra, và tìm cách chống nó xảy ra lần nữa.
- Một cách để làm được những việc trên là coi như hôm nay mình với vô làm công ty outsource, phải sửa code của người khác để lại. Hôm sau cũng lại tiếp tục nghĩ y như vậy.
  * Thật sự thì dù là code của bạn viết, thì 6 tháng sau nó cũng thành code của người khác viết thôi, vì bạn ko nhớ được hết đâu.

# Bug

- Tất cả chúng ta ko phải thánh thần, không phải máy móc, bug là điều không thể tránh khỏi.
- Chúng ta hạn chế bug bằng cách
  * Trước khi đưa người khác review code, chúng ta tự đọc lại code một lần (dùng git/sourcetree, etc)
  * Viết test, thay vì test manually bằng postman
  * Viết code càng đơn giản càng tốt, chú ý là việc code đơn giản quan trọng hơn performance (mục Tổng Quan)
  * Không giấu code lạ khó với người review, không để làm gì. Bạn làm vậy thì code bạn chạy được (tầm quan trọng số ba), nhưng nó không tốt (tầm quan trọng số 1)
  * Chỗ nào cần test/check kĩ, cần giao tiếp với QC thật rõ ràng, ko giấu. Cái quan trọng không phải là cái bạn làm có pass QC test hay không, mà là cái bạn làm không có bug
- Code bạn viết xong, nhưng bị QC báo bug nhiều, vậy thì nên nhìn lại xem mình có thật sự viết code theo đúng "Defensive Coding" chưa.
  * Việc bạn tự hạn chế bug trước khi đưa QC test, nó giúp cho bạn đỡ phải giao tiếp nhiều với người khác, và giúp bạn code được nhiều hơn, là cái trên lý thuyết là developers luôn muốn.
- Code bạn viết xong, rồi bị bug ở Production
  * Nó có phần là lỗi của bạn, nhưng cũng có phần lỗi của Lead, Tech Lead, QC. Ở team BE, chúng ta không trách móc người viết code khi code bị bug ở Production.
  * Dù vậy chúng ta xem lại tại sao lại bị vậy, lủng ở chỗ nào, tiến tới đảm bảo nó không xảy ra lại (ko phải xảy ra cái bug đó lại, mà không xảy ra việc để lọt bug).
- Bug ở production sẽ là lỗi của bạn khi:
  * Bạn không làm theo checklist, coding convention, guideline, dẫn tới bug đó.
  * Bạn không nói với ai vị trí đó phức tạp, dẫn tới người review, QC ko dành nhiều thời gian cho nó, dẫn tới bug ở đó.

# Hotfix

- Không phải ai cũng được hotfix ở Production
- Khi hotfix, cần đảm bảo:
  * 1. Không tạo bug mới
  * 2. Sửa được bug cũ
  * 3. Tốc độ
- Khi hotfix, có quyền bỏ qua coding convention.
- Cần chú ý tốc độ đứng thứ 3, không phải thứ 1. Cái đứng thứ 1 là không tạo bug mới
  * Việc này xảy ra rất nhiều, vì dưới áp lực, chúng ta dễ mắc sai lầm hơn
  * Code hotfix có thể tạo ra bug, tạo ra vấn đề về performance, cộng với việc code này sẽ được test rất nhanh để cho lên, dẫn tới hệ luỵ khó lường
- Việc cái quan trọng nhất là không tạo bug mới làm cho chúng ta thà không hotfix, tìm cách khác giải quyết cái bug đó thay vì code.
- Khi đã xác định sẽ hotfix, thì cứ làm càng an toàn càng tốt
  * chỗ nào không chắc, thì cứ thêm code để check
  * chỗ nào có thể xảy ra gây vấn đề lớn, và mình không chắc 100% có thể xảy ra, thì nghĩa là nó sẽ xảy ra, cứ viết code chặn nó lại
  * thêm log vào code, để lỡ code hotfix chưa giải quyết được vấn đề, thì chúng ta tiếp tục hotfix thêm, và có thêm thông tin từ log
- Code hotfix sẽ được clean up, sửa lại, test thêm, để đúng coding convention và qua được QC test.

# Quan hệ giữa mọi người

- Bảo là ai cũng như ai, giỏi ngang nhau là không đúng. Không ai giống ai cả. Nếu ai cũng giống nhau thì đã không cần phải giao tiếp.
- Không ai giống ai dẫn tới giao tiếp. Giao tiếp dẫn tới tranh cãi. Đây là điều không thể tránh khỏi.
- Tranh cãi là điều không thể tránh khỏi, cũng như bug vậy. Việc đó nghĩa là nó có thể được giảm bớt. Khi xảy ra tranh cãi, không có nghĩa là đó là lỗi của bạn,
- Bạn hãy coi những người trong team, những người ngoài team, là một phần trong công việc của bạn. Công việc này có trả lương. Công việc sẽ luôn có lúc khó khăn. Ráng chịu thôi.
- Khi khó chịu với cách làm việc của ai, có thể nói chuyện với Lead của bạn, Lead sẽ có cách giải quyết, hoặc chỉ bạn cách làm việc chung, hoặc Lead đi nói chuyện với Lead của người bạn đang gặp khó khăn trong cách làm việc
  * Việc này áp dụng cho khi bạn gặp khó khăn với người ở trong team BE và người ngoài team
- Một cách để làm được những việc trên là coi tất cả mọi người ko phải bạn là khách hàng của bạn. Khách hàng có đúng có sai, thì bạn vẫn phải tìm cách làm việc được với họ, và dù sao họ cũng trả lương cho bạn.
- Toxic sẽ không được chấp nhận dưới mọi hình thức. Những hành vi toxic bao gồm
  * Chửi bới, la mắng, lên giọng với người khác (trước mặt họ hay sau lưng họ)
  * Lên mặt thể hiện, tìm cách chứng tỏ mình giỏi hơn người khác, ép họ vào thế bí, rồi chê bai
  * Chê bai, bàn tán về những gì không tốt của người khác ở trong và ngoài công việc
  * Từ chối làm việc chung với người khác với lý do họ không giỏi/thông minh

# Công việc

- Quan trọng nhất là những gì mình làm ra (theo thứ tự quan trọng nhất trước):
  * 1. Code dễ đọc, sửa nhanh, không phức tạp.
  * 2. Hạn chế tối đa bug ngay từ lúc viết code.
  * 3. Code chạy được, làm đúng chức năng nó cần làm.
  * 4. Dễ test
  * 5. Performance của code tốt.
  * 6. Làm xong nhanh.
- Cái quan trọng nhất là code dễ đọc, sửa nhanh, không phức tạp. Việc này đồng nghĩa với việc
  * Nếu bạn thấy code bạn viết ít bug, nhưng đọc ko hiểu gì hết, và vô cùng phức tạp, thì cần phải viết lại (lệch với số 1)
  * Code chạy, nhưng cũng lệch với số 1, thì viết lại
  * Dễ test, performance tốt, nhưng lệch với số 1, thì viết lại
- Bạn sử dụng cách suy nghĩ này cho những mục sau (performance tốt nhưng sẽ dễ bug, ko kiểm soát được, nghĩa là viết lại etc)
- Từ đây bạn nhận ra là ở team BE chúng ta ko quan trọng code chạy được không, giải quyết được nhiều feature không, mà quan trọng code có tốt không.
  * Toàn bộ lý do là để khi có vấn đề ở Production, thì người hotfix đọc hiểu nhanh, fix nhanh gọn, giảm thiểu tối đa rủi ro
  * Nó khác hoàn toàn với những công ty đang xây dựng từ đầu, yêu cầu chạy đua feature, code càng nhanh nhiều càng tốt, chỉ cần chạy được để đưa tới khách hàng, người dùng sớm, rồi sẽ chỉnh sửa sau.
- Làm đúng chức năng nó cần làm không quan trọng bằng code dễ đọc ít bug (số 1 và 2), việc này dẫn tới:
  * Chúng ta có quyền từ chối nếu để làm được chức năng này cần phải viết code quá phức tạp, quá nhiều, không đảm bảo được ít bug ở chức năng này hay những chức năng liên qua.
  * Việc từ chối này sẽ phải giao tiếp với Lead, Lead sẽ suy nghĩ hướng dẫn cách code tốt hơn để không bị quá phức tạp, hoặc Lead công nhận cái phức tạp này không thể giải quyết bằng code, lúc đó Lead sẽ giao tiếp với Tech Lead
  * Tech Lead là người duy nhất có quyền xin không làm một chức năng với các team khác khi chức năng đó làm code phức tạp, không quản lý được bug (lệch với số 1 và 2)
- Mục cuối cùng là làm xong nhanh, cũng có nghĩa là thật sự ko cần làm xong nhanh. Tốc độ sẽ tới khi bạn làm nhiều, quen tay. Khi bạn bị kẹt ở một vấn đề, nên hỏi Lead, chứ ko nên dậm chân tại chỗ quá lâu.
- Ở team BE, chúng ta quản lý thời gian bằng cách
  * khi Lead giao việc, Lead tự cảm nhận được công việc này tốn bao lâu, tự biết được công việc này gấp hay không gấp
  * Nếu sau một khoảng thời gian X, Lead ko thấy bạn hỏi hay update gì, Lead sẽ hỏi bạn làm có vấn đề gì không
  * Nếu Lead cảm thấy có vấn đề làm cho deadline mà Lead nhẫm trong đầu ko kịp, Lead sẽ phải suy nghĩ hoặc phụ bạn hoặc bỏ bớt feature.
  * Việc giao tiếp để bỏ bớt feature sẽ phải được làm với Tech Lead. Tech Lead cũng là người duy nhất có thể xin bỏ bớt/không làm/làm theo hướng khác với requirements với team khác (team lo về requirements, manager, etc).
  * Lead hạn chế tối đa việc nói trước là cái này cần bao lâu xong (tuỳ Lead quyết định, cảm nhận khi nào nên nói khi nào không). Việc này để tránh bạn nghĩ đây là task gấp, và bỏ qua hết các mục quan trọng từ 1 tới 5
- Bạn được quyền xài ChatGPT, hỏi thầy hỏi bạn.
  * Ở đây không quan trọng code này ai viết, nhưng bạn đưa nó cho Lead review nghĩa là bạn chịu trách nhiệm cho phần code này. Nếu phần code này pass review (đúng coding convention, hợp lý đầy đủ rõ ràng dễ đọc etc), vậy là nó tốt. Vậy là hết.
  * Google, copy paste, ChatGPT, bạn nên dùng tất cả mọi loại tools có thể dùng được để sản xuất ra code tốt.
  * Bạn thuê người khác viết code cho bạn cũng được luôn, nhưng bạn tự trả tiền.
- Ở team BE, chúng ta không quan tâm làm sao bạn code được nhiêu đó, chúng ta chỉ quan tâm code đó có tốt không.
  * Bạn vừa nghe nhạc chơi game xem phim, etc bla bla. Team BE không quan tâm
  * Bạn ngồi ở nhà làm, bạn làm vào buổi tối, buổi sáng, ngoài giờ làm việc. Team BE không quan tâm
  * Code bạn xấu, không rõ ràng. Bạn không chịu giao tiếp để công việc được tốt lên, tránh hiểu sai ý nhau. Bạn làm ẩu vội chưa xong mà báo xong, và những việc tương tự. Team BE cực kì quan tâm và sẽ đánh giá bạn không tốt nếu những chuyện trên xảy ra
- Mặc dù không quan trọng bạn ngồi ở đâu làm, và bạn làm khi nào trong ngày, nhưng trong giờ làm việc, khi được Lead hay bất kì ai nhắn hỏi, bạn phải giữ thời gian trả lời trong 3 phút.
  * Có thể chỉ là 1 câu confirm, nhận task, nhưng cũng phải trả lời.
  * Không cấm bạn đi vệ sinh, đi dạo giảm stress, nhưng phải cầm điện thoại theo.
  * Giờ làm việc là giờ làm của cả công ty, không riêng gì team BE. Bạn phải có mặt sẵn sàng hỗ trợ team BE hay team khác những vấn đề về công việc. Không được lấy lý do quen làm buổi tối, và ko chịu hỗ trợ buổi sáng
  * Ngoài giờ làm việc, trừ trường hợp khẩn cấp, bạn không nên hỏi, nhắn về công việc với người khác trong công ty. Bạn luôn có thể note lại vào todo list của bạn để khi vào giờ làm việc thì hỏi/nói
- Giao tiếp là một cái cực kì quan trọng để bổ trợ cho công việc. Nếu không giao tiếp thì không thể đảm bảo các mục từ 1 tới 6 tốt được.
- Tất cả mọi người trong team BE phải giữ cho mình một cái Todo list và một bộ note.
