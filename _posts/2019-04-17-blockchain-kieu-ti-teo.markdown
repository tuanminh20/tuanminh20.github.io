---
layout: "single"
title: "Blockchain theo kiểu Tí Tèo"
date: "2019-04-17 18:35:56 +0700"
---

Giả sử bây giờ chúng ta có hai bạn Tí và Tèo. Một ngày nọ, Tí mượn Tèo 50k mua chuối, Tèo ghi một tờ giấy rằng Tí đang nợ 50k và có chữ kí của Tí trên đó. Vài ngày sau, Tí lật kèo, chối rằng không biết tờ giấy nợ đó và Tèo đã làm giả giấy nợ để tống tiền Tí. Okay, Tí chơi lầy thì Tèo biết làm sao để đòi nợ – tình bạn tan vỡ vì 50k.

> Trong ví dụ này, chúng ta sẽ gọi Tí và Tèo là 2 nút (node) của giao dịch.  

Nếu xét theo quy mô lớp 5A, trong đó cũng có rất nhiều bạn cho nhau mượn tiền cũng chỉ với một tờ giấy ghi nợ. Vì đã có khá nhiều vụ lật kèo xảy ra trong thời gian gần đây, Tũn mới nghĩ ra một giải pháp. Tũn đề xuất cả lớp mua một cuốn sổ chung để viết tất cả các giao dịch mượn tiền trong lớp. Mỗi khi Tí muốn mượn tiền Tèo, Tí phải tới lớp để nhận tiền và giao dịch sẽ được ghi cẩn thận vào cuốn sổ chung. Nếu có kiện tụng cứ lấy cuốn sổ đập vào mặt là hết cãi. Cả lớp hoan hô sáng kiến của Tũn và nhất trí bầu Tũn lên làm lớp trưởng =))

> Cuốn sổ đó sẽ được gọi là cơ sở dữ liệu (database).

Vào một ngày đẹp trời, bỗng có một đứa ăn vụng làm đổ mắm tôm vào cuốn sổ chung. Thế là không ai đọc được cái quái gì được ghi trong cuốn sổ chung đó nữa. Tí (lại đục nước béo cò) lật kèo không thèm trả tiền cho Tèo. Tình bạn lại tan vỡ.

> Lỗi này được gọi là lỗi cục bộ (one point failure) – sập tại 1 điểm là cả hệ thống sập luôn.

Tũn lại một lần nữa xuất hiện như một người hùng và đề xuất thay vì chỉ có 1 cuốn sổ chung, cả lớp sẽ có nhiều cuốn sổ chung. Với vai trò là lớp trưởng, Tũn sẽ chọn ra một vài bạn đáng tin cậy trong lớp và giao cho mỗi người một cuốn sổ. Bây giờ cứ mỗi lần có ai đó mượn tiền, giao dịch đó sẽ được Tũn ghi lại không chỉ ở một cuốn mà là ở một vài cuốn nhất định khác.  

> Lúc này hệ thống sổ được gọi là cơ sở dữ liệu phân tán (distributed database) với mỗi cuốn sổ là một nút

{% include figure image_path="http://www.the-glam-light.com/wp-content/uploads/2019/03/blockchain-3019120_640.png" caption="Cơ sở dữ liệu phân tán – với mỗi người là một cuốn sổ " %}

Nhưng vào một ngày đẹp trời khác, lại có một vấn đề khác xuất hiện. Tũn có một anh crush tạm gọi là Tụt. Anh Tụt này hay mượn tiền của các bạn trong lớp để đi bắn bi. Vì đã thua hết tiền nên Tụt mới nhờ Tũn giúp. Tũn nghe theo con tim ngu muội đã lén sửa nợ của Tụt ở tất cả các sổ nợ trong lớp. Và thế là Tụt hết nợ ai. Đơn giản như đang giỡn.

Vấn đề “đơn giản” để Tũn lách luật ở đây chính là lỗi hệ thống của cơ sở dữ liệu phân tán – tính tập trung. Hiểu đơn giản là mặc dù cơ sở dữ liệu được phân tán nhưng tất cả đều chịu sự chi phối của một chủ thể duy nhất (trong bối cảnh bình thường thì có thể hiểu là ngân hàng, hoặc các tổ chức tín nhiệm).

Nhưng cây kim trong bọc kiểu gì cũng lòi ra. Khi mọi người phát hiện được việc làm sai trái trên, Tũn liền bị cách chức lớp trưởng và để ngăn vụ việc tương tự. Cả lớp nhất trí quy định:

* Mỗi thành viên trong lớp đều sẽ giữ một cuốn sổ. Mỗi khi có một giao dịch xảy ra, các thành viên trong lớp sẽ họp lại và đồng loạt ghi giao dịch đó vào sổ của bản thân. Giả sử sỉ số cả lớp là 20 bạn, sẽ có 20 cuốn sổ tương ứng và sẽ không ai có thể kiểm soát/sửa đổi thông tin của giao dịch nào nữa. Đây là **tính phân quyền**
* Không bao giờ tẩy xóa, ghi đè hay loại bỏ bất kì thông tin giao dịch nào đã được ghi nhận. Đây là **tính bất biến**
* Với 2 quy định trên, giả sử như Tũn và Tụt muốn thay đổi các khoản nợ của Tụt. Thì cả lớp cũng sẽ phải đồng loạt thay đổi; những chủ nợ của Tụt sẽ nhận ra rằng Tụt chưa trả tiền và phát hiện rằng Tũn và Tụt đang muốn gian lận và cả lớp quyết định nghỉ chơi với cả 2 đứa. Tũn và Tụt từ này không được tham dự các buổi họp ghi nhận giao dịch của cả lớp nữa. Đây chính là **Cơ chế đồng thuận** (Consensus) nhằm bỏ phiếu để quyết định tính hợp lệ của các giao dịch trong hệ thống
* Tèo – người từ nay không bị ai xù tiền nữa cảm thấy hệ thống này quá chi là hay ho. Tèo nhận ra rằng các giao dịch được ghi nhận trong hệ thống hình thành nên một chuỗi nên đề nghị cả lớp gọi hệ thống sổ sách phân quyền, bất biến này là **Blockchain** (Chuỗi khối)

Okay, đây là ví dụ dễ hiểu nhất về Blockchain mà mình nghĩ ra. Túm cái quần lại, Blockchain là mạng lưu trữ phi tập trung, ngang hàng, không bị kiểm duyệt và không bị kiểm soát về luật bởi bất kỳ Chính phủ nào vì hệ thống này không tập trung quyền lực kiểm soát vào bất kì cá nhân/thực thể nào. Mọi giao dịch được ghi nhận đều được bỏ phiếu đồng thuận bởi rất nhiều nút và để thay đổi bất kỳ thứ gì trong hệ thống có thể nói là bất khả thi.

> Lược dịch: https://qr.ae/TUfJyZ
