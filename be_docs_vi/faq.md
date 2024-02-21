## Nên đọc theo thứ tự nào?
1. Coding convention
2. Defensive Coding
3. Code Review Checklist
4. Git
5. Structure/Tổng quan
6. Structure/Center Pattern
7. Structure/Các cách viết code thường gặp
8. Structure/Gộp lại chia ra khi nào
9. Structure/FAQ
10. FAQ
11. Văn hoá Team
12. Checklist cho giao tiếp
13. Todo list

- 1, 2, 3, 4 là bắt buộc để code qua được review (cũng là về code, cái quen thuộc với bạn)
- 5, 6, 7, 8 là nâng cao, vì không đọc thì vẫn fix bugs, sửa code được. Structure là dành cho khi phải viết một feature mới hoàn toàn, chứ ko phải thêm tính năng cho cái sẵn có
- 9, 10 FAQ là những câu hỏi thường gặp.
- 11 quan trọng, cần phải theo, nhưng nên đọc qua hết 9 mục trước để ý ra biết được sẽ cần phải theo thế nào.
- 12 rất quan trọng nhưng vì bạn mới vào nên có thể bạn không hiểu được điều đó (do đã quen với công ty cũ etc), đọc cuối cùng để tránh khó chịu.
- 13 là cách tạo, sử dụng Todo list, là cái bắt buộc bạn phải có.
- Không cần đọc folder Pattern (Nhưng nếu đọc thì vẫn tốt). Khi tới lúc cần, Lead sẽ chỉ bạn đọc đúng file để biết cách viết.

## Có thể từ chối làm vì code sẽ phức tạp, sao sướng vậy?

Thật ra bạn không thể từ chối làm, mà bạn phải giao tiếp với Lead là code sẽ phức tạp nếu làm cái này. Lead sẽ xem có đúng là code sẽ phức tạp khi làm không. Nếu đúng là code sẽ phức tạp khi làm, Tech Lead sẽ là người duy nhất được xin không làm chức năng này với Project Manager, Business Analyst Team. Nhưng vẫn có trường hợp không xin được. Lúc đó deadline, cách code sẽ phải thay đổi để làm code không phức tạp nữa.

## Có thật là được xem youtube trong khi làm việc?

Bạn muốn làm gì thì làm. Ở đây Lead không đứng sau lưng canh bạn làm, trừ khi cần chỉ bạn code. Nhưng Lead sẽ hỏi nếu một công việc đó bình thường bạn chỉ làm tốn 1 tiếng nay tốn 4 tiếng, hoặc chất lượng code của bạn đi xuống khi được Lead review. Lead có thể yêu cầu bạn thử không làm cái bạn đang làm trong lúc code nữa để xem nó có giúp cho công việc của bạn được tốt lên hay không. Nếu bạn từ chối tìm cách làm công việc của bạn tốt lên, bạn sẽ gặp vấn đề lớn với Lead của bạn.

## Có thật sự được dùng ChatGPT?

- Bạn muốn làm gì thì làm. Ở team BE, chúng ta thật sự chỉ có một mục tiêu duy nhất: làm tốt công việc được giao. Lead sẽ hỏi bạn nếu code của bạn đặt tên sai, đọc không hiểu gì hết, dư comment. Bạn sẽ phải sửa. Còn lại nó tới từ đâu, team BE không quan tâm.
- Việc dùng ChatGPT không giới hạn ở code. Nếu bạn khó chịu, hỏi ChatGPT cách nói chuyện, trả lời sao cho đỡ khó chịu cũng là một giải pháp tốt.

## Bạn khác viết thêm code làm hư code tôi đã viết. Nên làm thế nào?

- "Hư" chắc là bug. Có lẽ bạn đang được giao công việc fix bug này, bạn thấy đây là phần bạn viết trước đây, nhưng giờ nó có bug (lúc bạn viết thì không có). Như đã nói, ở team BE chúng ta không trách người viết code nếu code bị bug ở Production (mà bạn cũng đang không phải người viết code đó, bạn khác viết). Vậy nên bạn không cần quan tâm ai khác làm hư code bạn viết. Bạn chỉ cần quan tâm làm sao fix bug này khi được giao công việc fix bug ở code đó (chứ cũng không cần quan tâm ai tạo ra bug đó)
- Việc quan tâm ai tạo ra bug đó sẽ là một công việc của Tech Lead và Lead, để tránh việc đó xảy ra lần nữa.

## Nếu members của team khác không tốt, làm ảnh hưởng tới công việc của tôi, tôi nên làm gì?

- Bạn hãy nói với Lead của bạn
- Dù vậy vì đây là team khác, vấn đề của bạn có thể không được giải quyết. Ở team BE, chúng ta luôn cố gắng để đánh giá khách quan nhất khi có sự cố xảy ra khiến công việc bạn được giao không tốt, và sẽ không trách bạn nếu lỗi không phải là do bạn.
- Điều đó không có nghĩa bạn không cố gắng hết sức có thể để làm tốt công việc đó, dù là đang có khó khăn (khó khăn đến từ những cái không thể thay đổi được, như tình huống hiện tại). Không cố gắng, buông bỏ, từ chối làm việc sẽ làm cho việc này thành lỗi của bạn.
- Mặc dù vậy, đa phần khi có vấn đề với members của team khác, thường Lead và Tech Lead sẽ có hướng giải quyết bằng cách chỉnh sửa cách làm việc trong nội bộ team BE.

## Có được quan hệ tình dục với người trong team BE? Ngoài team BE?

Bạn muốn làm gì thì làm. Ở team BE, chúng ta thật sự chỉ có một mục tiêu duy nhất: làm tốt công việc được giao. Nếu quan hệ tình dục giúp bạn làm được điều đó, thì làm thôi.

## Khi rảnh, không có việc, tôi được phép làm gì?

- Bạn muốn làm gì thì làm. Ở team BE, chúng ta thật sự chỉ có một mục tiêu duy nhất: làm tốt công việc được giao. Bạn không được giao việc, nghĩa là bạn muốn làm gì cũng được.
- Dù vậy bạn có thể
  * Đọc lại code mình viết xem còn rõ nghĩa không
  * Xem mục Pattern, Structure của team BE docs
  * Học ngôn ngữ code mới, xem thêm về golang, postgres database, unix system
- Dù bạn làm gì, luôn cần giữ thời gian trả lời sau khi được hỏi dưới 3 phút.

## Tôi đã đọc hết Structure, Pattern docs, nhưng vẫn rất khó viết code theo hướng như vậy.

- Nếu bạn làm theo được hết những gì Structure, Pattern ghi, hiểu và tự áp dụng vào cái mới dựa trên cách code đó, bạn đã có thể nghỉ ở đây đi làm Tech Lead ở chỗ khác.
- Nói như vậy không có nghĩa là không cần cố gắng làm theo. Tất cả chúng ta sẽ cố gắng hết sức hiểu và làm theo những gì được ghi ở Structure và Pattern, từ bây giờ tới mãi về sau. Bạn sẽ luôn có sự hỗ trợ của Lead và Tech Lead cho việc đó.

## Hôm qua tôi đọc thấy coding convention ghi khác, mà hôm nay lại thấy ghi khác

- Xin lỗi bạn, đây là lỗi của Tech Lead. Bình thường khi chỉnh sửa bất kì vấn đề gì trong docs này, sau đó sẽ có thông báo tới toàn team
  * Việc này có thể là do khách quan, hoặc Tech Lead quên thông báo thật.
  * Việc này cũng có thể do bạn miss tin nhắn đó (hôm đó bạn nghỉ, etc)
- Nếu bạn trình bày rõ ràng lý do làm sai coding convention là do nó thay đổi, Lead sẽ không trách bạn
  * Bình thường Lead cũng sẽ không trách bạn nếu bạn sai coding convention, chỉ nhắc bạn sửa. Khi bạn sai lặp đi lặp lại liên tục cùng một vấn đề thì mới có câu hỏi "Sao bạn lại làm sai phần này liên tục vậy? Bạn có cần giúp đỡ gì không?". Nếu bạn không cố gắng giao tiếp và khắc phục điều đó, thì đó mới là lỗi của bạn.
  * Đôi khi việc bạn làm sai liên tục là có lý do đúng, vậy thì Lead và Tech Lead sẽ giao tiếp với nhau và dẫn tới coding convention được chỉnh sửa.
