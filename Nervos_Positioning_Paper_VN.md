# Tài liệu định vị Nervos Network

Bấm vào [đây để](https://github.com/nervosnetwork/rfcs/pull/138/commits/ea246c0d21c31a8195678658931cacdd4db37be5#diff-616b7ea527e9b7586b9062a18c620018) liên kết ban đầu

## 1. Mục đích của bài viết này

Nervos Network là sự kết hợp của một số loại giao thức và sự cách tân. Việc có một tài liệu, thông số kỹ thuật rõ ràng về việc thiết kế và triển khai giao thức chính là rất quan trọng. Tuy nhiên, để cộng đồng hiểu được những gì chúng tôi đang cố gắng thực hiện, những gì dự án phải đánh đổi và lý do đưa đến các quyết định hiện tại cũng cần được quan tâm.

Tài liệu này được xây dựng bắt nguồn từ việc kiểm tra chi tiết các vấn đề mà blockchain mở đang gặp phải và các biện pháp đang được sử dụng để giải quyết các vấn đề đó. Chúng tôi hi vọng rằng, bài viết sẽ giúp cho cộng đồng hiểu sơ bộ về cách mà chúng tôi tiếp cận những thách thức cũng như các thiết kế cơ bản của mạng lưới. Sau đó, chúng tôi sẽ cung cấp một hướng dẫn chi tiết về tất cả các phần của Nervos Network, với trọng tâm là cách chúng phối hợp cùng nhau để đạt được sứ mệnh chung của mạng.

## 2. Bối cảnh

Khả năng mở rộng, tính bền vững và khả năng tương tác là một trong những thách thức lớn nhất mà các blockchain mở phải đối mặt hiện nay. Mặc dù nhiều dự án tuyên bố họ đã tìm ra giải pháp cho những vấn đề này, nhưng điều quan trọng là phải hiểu những vấn đề này đến từ đâu và đặt giải pháp trong bối cảnh có thể đánh đổi.

### 2.1 Khả năng mở rộng

Bitcoin [1] là blockchain mở đầu tiên được thiết kế để sử dụng làm tiền điện tử ngang hàng. Ethereum [2] ra đời với nhiều ứng dụng hơn, tạo ra một nền tảng điện toán phi tập trung cho mục đích chung. Tuy nhiên, cả hai nền tảng này đều giới hạn khả năng giao dịch Bitcoin giới hạn kích thước mỗi block còn Ethereum giới hạn phí gas Đây là các bước cần thiết để đảm bảo tính phi tập trung dài hạn, tuy nhiên vô hình chung, chúng cũng hạn chế khả năng của cả hai nền tảng.

Cộng đồng blockchain đã đề xuất nhiều giải pháp mở rộng trong những năm gần đây. Về cơ bản, chúng ta có thể chia các giải pháp này thành hai loại: mở rộng trên chuỗi và mở rộng quy mô ngoài chuỗi.

Các giải pháp mở rộng trên chuỗi nhằm mục đích mở rộng thông lượng của quá trình đồng thuận và tạo ra các blockchain với thông lượng cao. Các giải pháp mở rộng quy mô ngoài chuỗi chỉ sử dụng blockchain như là nền tảng thanh toán và quản lý tài sảntài sản trong khi chuyển hầu hết giao dịch sang các lớp trên.

#### 2.1.1 Mở rộng quy mô trên chuỗi với blockchain đơn

Cách đơn giản nhất để tăng thông lượng của blockchain là tăng nguồn cung không gian block. Với không gian block bổ sung, nhiều giao dịch có thể thông qua mạng và được xử lý. Việc tăng nguồn cung của không gian block để đáp ứng với nhu cầu giao dịch tăng vẫn cho phép phí giao dịch ở mức thấp.

Bitcoin Cash (BCH) áp dụng phương pháp này để mở rộng mạng lưới thanh toán ngang hàng của mình. Giao thức Bitcoin Cash bắt đầu với kích thước block tối đa là 8 MB, sau đó được tăng lên 32 MB và sẽ tiếp tục được tăng lên vô thời hạn khi nhu cầu giao dịch tăng lên. Để tham khảo thêm, sau khi Bitcoin (BTC) triển khai Segregated Witness vào tháng 8 năm 2017, giao thức Bitcoin hiện cho phép kích thước block trung bình khoảng 2 MB.

Trong phạm vi của một trung tâm dữ liệu, theo tính toán, nếu 7,5 tỷ người mỗi người tạo 2 giao dịch trực tuyến mỗi ngày, mạng sẽ yêu cầu sản xuất các block 26 GB cứ sau 10 phút, dẫn đến tốc độ tăng trưởng blockchain là 3,75 TB mỗi ngày hoặc 1,37 PB mỗi năm [3]. Các yêu cầu lưu trữ và băng thông này là hợp lý cho bất kỳ dịch vụ đám mây nào hiện nay.

Tuy nhiên, việc hạn chế hoạt động của nút trong môi trường trung tâm dữ liệu dẫn đến một cấu trúc liên kết mạng duy nhất và buộc các thỏa hiệp về bảo mật (tỉ lệ fork của blockchain sẽ tăng lên khi yêu cầu truyền dữ liệu trên mạng tăng lên), cũng như tính phi tập trung (số lượng full node sẽ được giảm khi chi phí tham gia đồng thuận tăng).

Dưới góc nhìn kinh tế, kích thước block ngày càng tăng sẽ giảm bớt áp lực mà người dùng cảm nhận. Nhà phân tích mạng Bitcoin đã chỉ ra rằng các khoản phí sẽ về 0 khi một block được lấp đầy 80% sau đó tăng theo cấp số nhân [4].

Mặc dù việc đặt nặng chi phí phát triển mạng lưới lên những người vận hành như có thể được xem là khá hợp lý nhưng nó có thể trở nên thiển cận vì hai lý do sau:

- Việc khi chi phí giao dịch bằng 0, nguồn thu của người đào chủ sẽ đến từ phần thưởng block. Ngoại trừ trường hợp lạm phát là một phần không tách rời của giao thức, việc phát hành token mới một lúc nào đó sẽ dừng lại (khi toàn bộ token được phát hành) và những người đào sẽ không nhận được phần thưởng cũng như phí giao dịch đáng kể. Tác động kinh tế của việc này sẽ làm tổn hại nghiêm trọng đến mô hình bảo mật của mạng.

- Chi phí để chạy một nút đầy đủ trở nên đắt đỏ. Điều này loại bỏ khả năng người dùng thường xuyên xác minh lịch sử và giao dịch của blockchain, buộc phải phụ thuộc vào các nhà cung cấp dịch vụ như sàn và các bên thanh toán để đảm bảo tính toàn vẹn của blockchain. Yêu cầu về tính tin cậy vào các bên này phủ nhận giá trị cốt lõi của các blockchain mở ngang hàng và không cần dựa vào sự tin cậy.

Các nền tảng được tối ưu hóa chi phí giao dịch như Bitcoin Cash phải đối mặt với sự cạnh tranh đáng kể từ các blockchain khác (blockchain đóng và blockchain mở), cũng như các hệ thống thanh toán truyền thống. Các quyết định thiết kế cải thiện khả năng bảo mật hoặc kiểm duyệt sẽ phát sinh chi phí liên quan và lần lượt làm tăng chi phí sử dụng nền tảng. Tính đến bối cảnh cạnh tranh cũng như các mục tiêu đã nêu của mạng, rất có khả năng chi phí thấp hơn sẽ là mục tiêu bao trùm của mạng bằng bất kỳ giá nào.

Theo quan sát của chúng tôi, mục tiêu này phù hợp mạng chuyên dùng để giao dịch. Người dùng của các hệ thống này thờ ơ với sự đánh đổi dài hạn vì họ sẽ chỉ sử dụng mạng trong một thời gian ngắn. Một khi hàng hóa hoặc dịch vụ của họ đã được nhận và vấn đề thanh toán của họ đã được giải quyết thì những người dùng này không còn gì đáng lo ngại về hoạt động hiệu quả của mạng nữa. Việc chấp nhận các sự đánh đổi này rất rõ ràng thông qua việc các sàn tập trung cũng như các blockchain tập trung được sử dụng rộng rãi. Các hệ thống này trở nên phổ biến chủ yếu vì sự thuận tiện và hiệu quả giao dịch.

Một số nền tảng hợp đồng thông minh đã thực hiện các cách tiếp cận tương tự để mở rộng thông lượng blockchain, chỉ cho phép một bộ xác thực "siêu máy tính" giới hạn tham gia vào quy trình đồng thuận và xác thực độc lập blockchain.

Mặc dù các thỏa hiệp liên quan sự phi tập trung và bảo mật mạng cho phép giao dịch rẻ hơn và thuận tiện cho một nhóm người dùng, tuy nhiên mô hình bảo mật dài hạn bị xâm phạm, rào cản chi phí để xác minh độc lập các giao dịch và khả năng tập trung và cố định của những người đào node khiến chúng tôi tin rằng đây không phải là một cách tiếp cận phù hợp để nhân rộng các blockchain mở.

#### 2.1.2 Mở rộng quy mô chuỗi thông qua nhiều chuỗi

Việc mở rộng quy mô chuỗi nhờ kết hợp nhiều chuỗi có thể được thực hiện thông qua sharding, như đã được thực hiện trong Ethereum 2.0 hoặc chuỗi ứng dụng trong Polkadot. Các thiết kế này phân chia hiệu quả state toàn cầu và các giao dịch của mạng thành nhiều chuỗi, cho phép mỗi chuỗi nhanh chóng đạt được sự đồng thuận cục bộ và sau đó toàn bộ mạng lưới đạt được sự đồng thuận toàn cầu thông qua sự đồng thuận của "Beacon Chain" hoặc "Reply Chuỗi".

Các thiết kế này cho phép nhiều chuỗi sử dụng mô hình bảo mật được chia sẻ, đồng thời cho phép thông lượng cao và giao dịch nhanh bên trong shard (Ethereum) hoặc chuỗi para (Polkadot). Mặc dù mỗi hệ thống này là một mạng gồm các blockchain được liên kết với nhau tuy nhiên chúng khác nhau về các giao thức chạy trên mỗi chuỗi. Trong Ethereum 2.0, mọi phân đoạn đều chạy cùng một giao thức, trong khi ở Polkadot, mỗi chuỗi para có thể chạy một giao thức tùy chỉnh, được tạo thông qua khung Substrate.

Trong các kiến ​​trúc đa chuỗi này, mỗi dApp chỉ nằm trên một chuỗi. Mặc dù các nhà phát triển ngày nay đã quen với khả năng xây dựng các dApp tương tác liền mạch với bất kỳ dApp nào khác trên blockchain nhưng các mẫu thiết kế cần phải thích ứng với các kiến ​​trúc đa chuỗi mới. Nếu một dApp được phân chia thành các trên các shard khác nhau thì các cơ chế sẽ được yêu cầu để giữ trạng thái đồng bộ hóa trên các phiên bản khác nhau của dApp (nằm trên các shard khác nhau). Ngoài ra, mặc dù các cơ chế lớp 2 có thể được áp dụng để hỗ trợ giao tiếp nhanh giữa các shard, giao dịch shard chéo sẽ đòi hỏi sự đồng thuận toàn cầu và đưa ra độ trễ xác nhận.

Với các giao dịch không đồng bộ này, vấn đề về "xe lửa và khách sạn" đã bắt đần nảy sinh gay gắt. Khi hai giao dịch cần được xử lý liên quan đến nhau (ví dụ đặt vé tàu và phòng khách sạn ở hai shard khác nhau), tất yếu phải có giải pháp xử lý mới. Ethereum giới thiệu hợp đồng " yanking ", trong đó hợp đồng phụ thuộc bị xóa khỏi 1 shard và được tạo trên shard thứ hai (nơi có chứa hợp đồng phụ thuộc khác) và cả hai giao dịch sau đó được thực hiện trên shard thứ hai. Tuy nhiên, hợp đồng kéo dài sau đó sẽ không có sẵn trên shard ban đầu dẫn tới vấn đề về khả năng ứng dụng và một lần nữa đặt ra yêu cầu thiết kế mới.

Phương pháp sharding những lợi thế và thách thức riêng. Nếu các shard là hoàn toàn độc lập và và lượng giao dịch qua sharding nhỏ, blockchain có thể mở rộng thông lượng bằng cách tăng số lượng shard. Điều này phù hợp nhất cho các ứng dụng độc lập không yêu cầu trạng thái bên ngoài hoặc kết hợp với các ứng dụng khác.

Một điểm lưu ý nữa về mặt kỹ thuật, sharding thường yêu cầu cấu trúc liên kết "1 + N", trong đó chuỗi N kết nối với một chuỗi meta, đưa ra giới hạn trên về số lượng phân đoạn mà chuỗi meta có thể hỗ trợ mà không gặp phải vấn đề về khả năng mở rộng.

Chúng tôi quan sát giá trị quan trọng trong state toàn cầu thống nhất, cho phép một hệ sinh thái các ứng dụng phụ thuộc lẫn nhau xuất hiện và đổi mới ở mọi góc cạnh tương tự như việc sử dụng thư viện của các nhà phát triển web cho các mối quan tâm cấp thấp hơn và API mở để tích hợp dịch vụ. Trải nghiệm phát triển đơn giản hơn được kích hoạt khi các nhà phát triển không phải xem xét tính đồng bộ (trong chuyển giao tài sản chéo hoặc chuyển tin nhắn), cũng như trải nghiệm người dùng vượt trội dẫn đến sự nhất quán trong mối quan tâm về kiến ​​trúc của các tương tác blockchain.

Chúng tôi nhận ra rằng sharding là một giải pháp có khả năng mở rộng đầy hứa hẹn (đặc biệt là các ứng dụng ít phụ thuộc lẫn nhau), tuy nhiên chúng tôi tin rằng sẽ có lợi khi thiết kế tập trung state có giá trị vào một blockchain duy nhất cho phép khả năng kết hợp. Với thiết kế này, các phương pháp mở rộng quy mô ngoài chuỗi được sử dụng để cho phép thông lượng cao hơn.

#### 2.1.3 Mở rộng quy mô ngoài chuỗi qua Lớp 2

Trong các giao thức lớp 2, blockchain lớp cơ sở hoạt động như một lớp thanh toán (hoặc cam kết), trong khi mạng lớp thứ hai định tuyến bằng chứng mật mã cho phép người tham gia "nhận" tiền điện tử. Tất cả các hoạt động của lớp thứ hai được bảo mật bằng mật mã bởi blockchain cơ bản và lớp cơ sở chỉ được sử dụng để giải quyết số tiền vào hoặc ra khỏi mạng lớp thứ hai và để giải quyết tranh chấp. Những thiết kế này hoạt động mà không cần ủy thác quyền nắm giữ (hoặc lo sợ rủi ro mất mát) tiền và cho phép giao dịch ngay lập tức gần như miễn phí.

Những công nghệ này cho thấy cách mạng lưu trữ giá trị như Bitcoin có thể được sử dụng cho các khoản thanh toán hàng ngày. Ví dụ điển hình nhất về giải pháp lớp 2 trong thực tế là kênh thanh toán giữa khách hàng và quán cà phê. Giả sử Alice ghé thăm cửa hàng cà phê Bitcoin mỗi sáng, vào đầu tháng cô gửi tiền vào một kênh thanh toán Lightning mà cô đã mở với quán cà phê. Khi cô đến thăm mỗi ngày, cô đã ký mật mã vào quyền của quán cà phê để lấy một số tiền, để đổi lấy cà phê của mình. Các giao dịch này xảy ra ngay lập tức và hoàn toàn ngang hàng, "off-chain" cho phép trải nghiệm khách hàng một cách trơn tru. Nếu kênh Lightning không đáng tin cậy, Alice hoặc quán cà phê có thể đóng kênh bất cứ lúc nào và lấy số tiền của họ còn lại trên kênh vào thời điểm đó.

Các công nghệ kênh thanh toán như Lightning chỉ là một ví dụ về kỹ thuật mở rộng quy mô ngoài chuỗi; có nhiều công nghệ lâu đời có thể mở rộng quy mô thông lượng blockchain một cách an toàn theo cách này. Trong khi các kênh thanh toán bao gồm các thỏa thuận ngoài chuỗi đến số dư kênh giữa hai bên thì state channel bao gồm các thỏa thuận ngoài chuỗi đến state tùy ý giữa những người tham gia kênh. Sự khái quát hóa này có thể là cơ sở của các ứng dụng có thể mở rộng, không tin cậy hay phi tập trung. Một kênh trạng thái đơn lẻ thậm chí có thể được sử dụng bởi nhiều ứng dụng, cho phép hiệu quả sử dụng còn cao hơn nữa. Khi một bên sẵn sàng thoát khỏi kênh, họ có thể gửi bằng chứng mật mã đã được thống nhất cho blockchain sau đó sẽ thực hiện các chuyển đổi state như đã thỏa thuận.

Chuỗi bên là một cấu trúc khác cho phép tăng thông lượng mặc dù thông qua bên thứ 3 là những người vận hành blockchain đáng tin cậy. Với một chốt hai chiều cho một blockchain với sự đồng thuận đáng tin cậy hay không tin cậy, tiền có thể được chuyển qua lại giữa chuỗi chính và chuỗi phụ. Điều này cho phép thực hiện một lượng lớn các giao dịch đáng tin cậy trên chuỗi bên, và ghi chép cuối cùng sẽ thực hiện trên chuỗi chính. Chi phí giao dịch chuỗi bên rất nhỏ, xác nhận nhanh và thông lượng cao. Mặc dù các chuỗi bên cung cấp một trải nghiệm vượt trội trong một số vấn, nó vẫn có thể gây tổn hại về tính bảo mật. Tuy nhiên, có rất nhiều nghiên cứu về các chuỗi bên không cần sự tin cậy cho rằng chúng có thể cung cấp các cải tiến hiệu suất tương tự mà không ảnh hưởng đến bảo mật.

Ví dụ về công nghệ chuỗi bên không cần sự tin cậy là Plasma (được nêu trong mục 5.3) là một kiến ​​trúc chuỗi bên tận dụng gốc tin cậy trên blockchain với sự đồng thuận toàn cầu rộng rãi. Chuỗi plasma cung cấp các cải tiến hiệu suất tương tự như chuỗi bên tập trung, và đồng thời đảm bảo được tính bảo mật. Trong trường hợp nhà điều hành chuỗi Plasma là độc hại hoặc gặp trục trặc, người dùng được cung cấp một cơ chế cho phép họ rút an toàn tài sản chuỗi bên của họ về chuỗi chính. Điều này được thực hiện mà cần kết hợp với nhà điều hành chuỗi Plasma, mang đến cho người dùng sự tiện lợi của các giao dịch chuỗi bên cũng như bảo mật của blockchain lớp 1.

Mở rộng quy mô ngoài chuỗi cho phép phân cấp, bảo mật và khả năng mở rộng. Bằng cách di chuyển mọi thứ trừ giao dịch thanh toán và tranh chấp ra ngoài chuỗi, sự đồng thuận toàn cầu của blockchain mở sẽ được sử dụng một cách hiệu quả. Các giao thức lớp 2 đa dạng có thể được thực hiện dựa trên các yêu cầu của từng ứng dụng, tạo sự linh hoạt cho các nhà phát triển và người dùng. Khi nhiều người tham gia được thêm vào mạng, hiệu suất không bị ảnh hưởng và tất cả các bên có thể đều được hưởng sự bảo mật được cung cấp bởi sự đồng thuận của lớp 1.

### 2.2 Tính bền vững

Việc duy trì hoạt động lâu dài của một blockchain công cộng tự chủ, không có chủ sở hữu gặp phải khá nhiều thách thức. Các ưu đãi phải được cân bằng giữa các bên liên quan khác nhau và hệ thống phải được thiết kế theo cách cho phép vận hành nút đầy đủ rộng rãi và kiểm chứng công khai. Yêu cầu phần cứng phải duy trì ở mức hợp lý mà vẫn đủ để hỗ trợ một mạng toàn cầu mở.

Phần thưởng và kiểm soát tài sản bản trên blockchain phải có khả năng cân bằng nhu cầu tăng lên về việc nắm giữ lâu dài với nhu cầu nhận được phần thưởng của các người đào hoặc người xác nhận – những người đang góp phần đảm bảo tính an toàn của mạng.

Ngoài ra, một khi blockchain mở đã đi vào hoạt động, rất khó có thể thay đổi các quy tắc cơ bản chi phối giao thức. Ngay từ đầu, hệ thống phải được thiết kế để phát triển bền vững. Do đó, chúng tôi đã tiến hành nghiên cứu kỹ lưỡng các thách thức trong việc xây dựng các blockchain mở và bền vững.

#### 2.2.1 Phi tập trung

Một trong những mối đe dọa lớn nhất đối mà các blockchain mở phải đối mặt là một rào cản ngày càng tăng về sự tham gia và xác minh giao dịch độc lập, được phản ánh trong chi phí vận hành nút đầy đủ. Các nút đầy đủ cho phép người tham gia blockchain xác minh độc lập trạng thái hay lịch sử trên chuỗi và giữ những người đào hoặc người xác nhận của mạng chịu trách nhiệm bằng cách từ chối định tuyến các block không hợp lệ. Khi chi phí của các nút đầy đủ tăng và số lượng của chúng giảm, những người tham gia vào mạng ngày càng dựa vào những người cung dịch vụ chuyên nghiệp để cung cấp cả lịch sử và trạng thái hiện tại, làm xói mòn mô hình tin cậy mà tính mở của blockchain.

Để một nút đầy đủ theo kịp sự phát triển của blockchain, nó phải có thông lượng tính toán đầy đủ để xác thực các giao dịch, thông lượng băng thông để nhận giao dịch và dung lượng lưu trữ để lưu trữ toàn bộ dữ liệu toàn cầu. Để kiểm soát chi phí vận hành của một nút đầy đủ, giao thức phải thực hiện các biện pháp để ràng buộc thông lượng hoặc tăng trưởng công suất của cả ba tài nguyên này. Hầu hết các giao thức blockchain ràng buộc thông lượng tính toán, nhưng rất ít ràng buộc sự tăng trưởng của dữ liệu toàn cầu. Khi các chuỗi này tăng kích thước và thời gian hoạt động, chi phí vận hành nút đầy đủ sẽ không ngừng tăng lên. 

#### 2.2.2 Mô hình kinh tế

Mặc dù đã có rất nhiều nghiên cứu về các giao thức đồng thuận trong những năm gần đây, chúng tôi tin rằng kinh tế học tiền điện tử là một lĩnh vực chưa được nghiên cứu kỹ. Nói chung, các mô hình kinh tế tiền điện tử hiện tại cho các giao thức lớp 1 chủ yếu tập trung vào các phần thưởng và trừng phạt để đảm bảo sự đồng thuận của mạng và token mạng chủ yếu được sử dụng để trả phí giao dịch hoặc để đáp ứng các yêu cầu stake là nguyên nhân của tấn công mạo nhận.

Chúng tôi tin rằng một mô hình kinh tế được thiết kế hiệu quả cần vượt ra ngoài quy trình đồng thuận và đảm bảo tính bên vững lâu dài của giao thức. Cụ thể là hình mẫu kinh tế cần được thiết kế với những mục tiêu sau:

- Mạng phải có hướng đi bền vững để tăng doanh thu bù cho các bên cung cấp dịch vụ (thường là những người đào hoặc người xác nhận giaio dịch), đảm bảo rằng mạng vẫn an toàn bền vững. 

- Mạng phải có hướng đi bền vững để duy trì rào cản thấp đối với sự tham gia, đảm bảo rằng mạng vẫn duy trì tính phi tập trung theo thời gian

- Các tài nguyên của mạng công cộng cần được phân bổ một cách hiệu quả, công bằng 

- Token của blockchain phải có giá trị nội

#### 2.2.3 Phân tích mô hình kinh tế của Bitcoin

Giao thức Bitcoin giới hạn kích thước của các block và cố định thời gian cho 1 block. Điều này làm cho thông lượng băng thông của mạng trở thành một nguồn tài nguyên khan hiếm mà người dùng phải đặt giá thầu thông qua phí giao dịch. Bitcoin Script không cho phép các vòng lặp, làm cho độ dài của tập lệnh gần đúng với độ phức tạp tính toán của nó. Tóm lại, nhu cầu lớn hơn về không gian block sẽ chuyển thành phí giao dịch cao hơn cho người dùng. Ngoài ra, càng có nhiều đầu vào, đầu ra hoặc các bước tính toán có liên quan đến giao dịch, người dùng càng phải trả nhiều phí giao dịch.

Giá trị nội tại của Bitcoin gần như hoàn toàn đến từ tính chất tiền tệ của nó (xã hội chấp nhận coi nó như tiền tệ) và trong trường hợp đặc biệt, sự tự nguyện nắm giữ nó như là 1 công cụ lưu trữ giá trị. Mô hình bảo mật của Bitcoin là một vòng lặp - nó phụ thuộc vào niềm tin rằng mạng sẽ an toàn bền vững và do đó có thể được sử dụng như một kho lưu trữ tiền tệ có giá trị.

Giới hạn kích thước block của Bitcoin lập rào cản cho sự tham gia của mạng - giới hạn kích thước block càng thấp thì càng tạo khó khăn cho những người không chuyên nghiệp chạy các full node. State toàn cầu Bitcoin là tập hợp UTXO với tốc độ tăng trưởng cũng được giới hạn bởi giới hạn kích thước block. Người dùng được khuyến khích tạo và sử dụng UTXO một cách hiệu quả; tạo ra nhiều UTXO sẽ làm tăng chi phí giao dịch. Tuy nhiên, phần thưởng nào khuyến khích kết hợp UTXO và giảm quy mô của state toàn cầu; một khi UTXO được tạo, nó sẽ chiếm không gian dữ liệu toàn cầu miễn phí cho đến khi nó được sử dụng.

Mô hình kinh tế dựa trên phí giao dịch của Bitcoin là một mô hình công bằng để phân bổ thông lượng băng thông và nguồn lực khan hiếm được áp đặt bởi giao thức. Mô hình kinh tế này phù hợp cho hệ thống thanh toán ngang hàng nhưng là một lựa chọn kém cho nền tảng lưu trữ giá trị thực sự. Người dùng bitcoin sử dụng blockchain để lưu trữ giá trị chỉ cần thanh toán phí giao dịch chỉ một lần và chiếm không gian lưu trữ mãi mãi, được hưởng bảo mật liên tục bởi người đào. 

Bitcoin có tổng hardcap và phát hành mới thông qua phần thưởng block – phần thưởng này sẽ dần trở về 0. Điều này có thể gây ra hai vấn đề:

Thứ nhất, nếu Bitcoin vẫn tiếp tục nắm giữ vai trò lưu trữ giá trị giá trị đơn vị của BTC sẽ tiếp tục tăng và tổng giá trị mà mạng đang lưu trữ cũng sẽ tăng theo. Một nền tảng lưu trữ giá trị phải có khả năng tăng ngân sách bảo mật khi giá trị mà nó lưu trữ tăng theo thời gian, nếu không, nó sẽ thu hút hacker tạo ra các giao dịch double spending (chi tiêu 2 lần) và ăn cắp tài sản của mạng. 

Khi chi phí gian lận nhỏ hơn lợi nhuận mà họ có thể kiếm được bằng hành động trung thực, các cuộc tấn công sẽ diễn ra thường xuyên. Điều này tương tự như việc một thành phố phải tăng chi tiêu về quân sự khi thu nhập của những người trong thành phố tăng lên. Nếu không có khoản đầu tư này, sớm muộn gì thành phố cũng sẽ bị tấn công và cướp phá.

Với sự tồn tại của phần thưởng block, Bitcoin có thể mở rộng bảo mật theo giá trị tổng hợp mà nó lưu trữ - nếu giá của Bitcoin tăng gấp đôi, thu nhập mà người khai thác nhận được từ phần thưởng block cũng sẽ tăng gấp đôi, do đó họ có thể đủ khả năng để tạo ra tỷ lệ băm gấp đôi, làm cho mạng đắt gấp đôi để tấn công.

Tuy nhiên, điều này thay đổi khi phần thưởng block dự đoán giảm xuống bằng không. Người khai thác sẽ phải phụ thuộc hoàn toàn vào phí giao dịch; thu nhập của họ sẽ không còn quy mô theo giá trị của tài sản Bitcoin mà sẽ được xác định bởi nhu cầu giao dịch của mạng. Nếu nhu cầu giao dịch không đủ cao để lấp đầy không gian block có sẵn, tổng phí giao dịch sẽ rất nhỏ. Vì phí giao dịch hoàn toàn là một chức năng của nhu cầu không gian block và độc lập với giá của Bitcoin nên điều này sẽ có tác động sâu sắc đến mô hình bảo mật của Bitcoin.

Thứ hai, khi phần thưởng block dự đoán dừng lại, chênh lệch thu nhập trên mỗi block cho người khai thác sẽ tăng lên và tạo ra lợi ích của các người đào khi fork mạng lưới thay vì phát triển blockchain hiện tại. Trong trường hợp cực đoan, khi mempool (khu vực giữa các nút cho tất cả các giao dịch đang chờ xử lý) của một máy đào trống rỗng và họ nhận được một block chứa đầy giá trị, động cơ của họ là fork mạng lưới và đánh cắp các khoản phí này thay hỗ trợ mạng lưới tạo ra một block không có thu nhập [5]. Đây được gọi là thử thách "fee nipping" trong Bitcoin. Hiện chưa tìm thấy giải pháp thỏa mãn thách thức nêu trên mà không cần loại bỏ hardcap của Bitcoin.

Hiện chưa tìm thấy giải pháp thỏa mãn thách thức nêu trên mà không cần loại bỏ hardcap của Bitcoin.

#### 2.2.4 Phân tích mô hình kinh tế của các nền tảng hợp đồng thông minh

Mô hình kinh tế điển hình của nền tảng hợp đồng thông minh phải đối mặt với nhiều thách thức hơn. Hãy lấy Ethereum làm ví dụ. Kịch bản của Ethereum cho phép các vòng lặp do đó độ dài của tập lệnh không phản ánh độ phức tạp tính toán của tập lệnh. Đây là lý do Ethereum không giới hạn kích thước block hoặc thông lượng băng thông mà là thông lượng tính toán.

Để các giao dịch được ghi lại trên blockchain Ethereum, người dùng đặt giá thầu cho mỗi chi phí tính toán mà họ sẵn sàng trả bằng phí giao dịch. Ethereum sử dụng khái niệm "gas" để đo lường chi phí tính toán được định giá bằng ETH và kiểm soát tỷ lệ "phí gas" đảm bảo rằng chi phí cho mỗi bước tính toán không phụ thuộc vào biến động giá của token. Giá trị nội tại của ETH xuất phát từ vị trí của nó là token thanh toán của nền tảng tính toán phi tập trung; nó là loại tiền duy nhất có thể được sử dụng để thanh toán cho tính toán trên Ethereum.

State toàn cầu của Ethereum được thể hiện bằng state trie EVM, cấu trúc dữ liệu chứa số dư và state nội bộ của tất cả các tài khoản. Khi tài khoản mới hoặc giá trị hợp đồng được tạo, kích thước của state toàn cầu sẽ càng mở rộng. Ethereum tính 1 lượng phí cố định để thêm các giá trị mới vào kho lưu trữ state và cung cấp "gas stipend" cố định bù đắp chi phí gas của giao dịch khi giá trị bị loại bỏ.

Mô hình lưu trữ "pay once, occupy forever " không phù hợp với cấu trúc chi phí liên tục của người đào, full node và mô hình không khuyến khích người dùng tự nguyện xóa trạng thái hoặc xóa trạng thái sớm hơn. Kết quả là Ethereum đã trải qua sự tăng trưởng nhanh chóng về quy mô state. Kích thước state lớn hơn làm chậm quá trình xử lý giao dịch và tăng chi phí vận hành của các full node. Nếu không có chính sách khuyến khích để xóa trạng thái, tình trạng trên sẽ vẫn tiếp tục lâu dài.

Tương tự như Bitcoin, phí gas dựa theo nhu cầu của Ethereum là một mô hình công bằng để phân bổ thông lượng tính toán, tài nguyên khan hiếm của nền tảng. Mô hình này cũng phục vụ mục đích của Ethereum như một hệ thống tính toán phi tập trung. Tuy nhiên, mô hình phí lưu trữ dữ liệu của nó không phù hợp với đề xuất tiềm năng như là một nền tảng lưu trữ tài sản hoặc dữ liệu phi tập trung. Nếu không có chi phí cho việc lưu trữ dữ liệu dài hạn, người dùng sẽ hưởng lợi ích miễn phí mãi mãi. Không tạo ra được sự khan hiếm trong không gian lưu trữ dữ liệu, cả thị trường và động lực cung cầu đều không thể được thiết lập.

Không giống như Bitcoin chỉ giới hạn kích thước block trong giao thức cốt lõi, Ethereum cho phép các nhà khai thác tự động điều chỉnh giới hạn block gas khi họ tạo ra các block. Các công cụ khai thác với phần cứng tiên tiến và băng thông lớn có thể tạo ra nhiều block hơn, chi phối hiệu quả quá trình voting. Mong muốn của họ là điều chỉnh giới hạn block gas lên cao, nâng cao mức độ tham gia và buộc các người đào nhỏ phải tử bỏ. Đây là một yếu tố khác biệt góp phần làm tăng nhanh chi phí vận hành full node.

Các nền tảng hợp đồng thông minh như Ethereum là nền tảng đa tài sản. Nó hỗ trợ phát hành và giao dịch tất cả các loại tài sản tiền điện tử, thường được biểu thị là "tokens". Các nền tảng này cũng cung cấp bảo mật cho không chỉ các token riêng mà cả giá trị của tất cả các tài sản tiền điện tử trên đó. Do đó, "lưu trữ giá trị" trong ngữ cảnh đa tài sản ám chỉ sự bảo toàn giá trị tài sản có lợi cho cả token và tài sản tiền điện tử được lưu trữ trên nền tảng này.

Với phần thưởng block, Bitcoin có một mô hình kinh tế "lưu trữ giá trị" vô cùng tuyệt vời. Những người đào được trả phần thưởng block cố định bằng BTC, do đó thu nhập của họ tăng lên cùng với giá BTC. Bởi vậy, nền tảng này có khả năng tăng doanh thu cho các người đào để tăng cường bảo mật trong khi vẫn duy trì mô hình kinh tế bền vững.

Đối với các nền tảng đa tài sản, việc thực hiện yêu cầu này trở nên khó khăn hơn nhiều, bởi vì "giá trị" có thể được biểu thị bằng tài sản tiền điện tử ngoài token. Nếu giá trị của tài sản tiền điện tử được bảo mật bởi nền tảng tăng lên nhưng giá trị token không đổi thì bảo mật mạng sẽ không tăng và việc tấn công double spend tài sản mã hóa trên nền tảng sẽ tăng lên.

Để một nền tảng hợp đồng thông minh đa tài sản hoạt động như một kho lưu trữ giá trị, nhu cầu sở hữu tài sản trên chuỗi phải có một lộ trình rõ ràng để tạo ra nhu cầu sở hữu token. Hay nói cách khác, token của nền tảng phải có giá trị để nắm bắt giá trị tổng hợp của nền tảng. Nếu giá trị nội tại của token của nền tảng bị giới hạn trong thanh toán phí giao dịch, giá trị của nó sẽ chỉ được xác định bởi nhu cầu giao dịch. Giá của token sẽ không đáp ứng nhu cầu sở hữu nền tảng lưu trữ tài sản crypto.

Các nền tảng hợp đồng thông minh không được thiết kế để hoạt động như một kho lưu trữ giá trị (phải dựa vào sự sẵn lòng nắm giữ token dựa vào giá trị cốt lõi của nó để duy trì tính bảo mật). Điều này chỉ khả thi nếu nền tảng thống trị sở hữu các tính năng độc đáo không thể được tìm thấy ở nền tảng khác hoặc cạnh tranh với các nền tảng khác bằng cách cung cấp chi phí giao dịch thấp nhất có thể.

Ethereum hiện đang có được sự thống trị như vậy từ đó nó có thể có được giá trị về mặt tiền tệ. Tuy nhiên, với sự gia tăng của các nền tảng cạnh tranh, nhiều nền tảng được thiết kế với tốc độ giao dịch cao hơn và cung cấp chức năng tương tự, câu hỏi đặt ra ở đây là liệu tin tưởng vào đặc tính tiền tệ có thể duy trì tính bảo mật của blockchain hay không, đặc biệt trong trường hợp token rõ ràng không được thiết kế hoặc được tin rằng nó là tiền. Hơn nữa, ngay cả khi một nền tảng cung cấp các tính năng độc đáo, tính tiền tệ của nó có thể được trừu tượng hóa bởi giao diện người dùng thông qua các giao dịch hoán đổi hiệu quả (rất có thể khi blockchain được ứng dụng rộng rãi). Người dùng sẽ nắm giữ các tài sản mà họ cảm thấy quen thuộc nhất, chẳng hạn như Bitcoin hoặc stable coin và chỉ dùng token nền tảng khi cần trả phí giao dịch. Trong cả hai trường hợp này, nền tảng của nền kinh tế tiền điện tử sẽ sụp đổ.

Các nền tảng đa tài sản lớp 1 phải cung cấp sự bảo mật bền vững cho tất cả các tài sản tiền điện tử mà chúng lưu trữ. Nói cách khác, chúng phải có một mô hình kinh tế được thiết kế như là một nơi lưu trữ giá trị. 

#### 2.2.5 Tài trợ cho phát triển giao thức cốt lõi

Blockchains mở là cơ sở hạ tầng công cộng. Sự phát triển ban đầu của các hệ thống này đòi hỏi rất nhiều kinh phí và một khi chúng hoạt động đòi hỏi phải bảo trì và nâng cấp liên tục. Nếu không có những người tận tâm duy trì các hệ thống này, chúng sẽ có nguy cơ gặp phải những lỗi nghiêm trọng và hoạt động không tối ưu. Bởi các giao thức Bitcoin và Ethereum không đưa ra một cơ chế riêng để đảm bảo tài trợ cho sự phát triển trong tương lai, do vậy, chúng dựa vào sự tham gia liên tục của các doanh nghiệp có lợi ích phù hợp và cộng đồng nguồn mở vị tha.

Dash là dự án đầu tiên sử dụng ngân quỹ để đảm bảo sự phát triển liên tục trong giao thức. Trong khi hỗ trợ liên tục sự phát triển của giao thức, thiết kế này tạo ra sự thỏa hiệp liên quan đến tính bền vững của giá trị tiền điện tử. Giống như hầu hết các kho bạc blockchain, mô hình này làm hao mòn giá trị của việc nắm giữ dài hạn.

Mạng lưới Nervos sử dụng mô hình ngân quỹ cung cấp kinh phí bền vững cho phát triển cốt lõi. Các quỹ ngân quỹ đến từ tính lạm phát của những người nắm giữ token ngắn hạn, trong khi những tác động của lạm phát này được giảm thiểu cho những người nắm giữ dài hạn. Thông tin thêm về cơ chế này được mô tả trong mục (4.5).

### 2.3 Khả năng tương Tác

Khả năng tương tác giữa các blockchains là một chủ đề thường được thảo luận và đã có nhiều dự án được đề xuất để giải quyết thách thức này. Với các giao dịch đáng tin cậy trên các blockchain, hiệu ứng mạng thực sự có thể được nhận ra trong nền kinh tế phi tập trung.

Ví dụ đầu tiên về khả năng tương tác blockchain là hoán đổi nguyên tử giữa Bitcoin và Litecoin. Việc trao đổi Bitcoin đáng tin cậy cho Litecoin và ngược lại được thực hiện không phải thông qua các cơ chế trong giao thức mà thông qua một tiêu chuẩn mã hóa được chia sẻ (cụ thể là sử dụng hàm băm SHA2-256).

Tương tự như vậy, thiết kế của Ethereum 2.0 cho phép kết nối nhiều shard, tất cả đều chạy cùng một giao thức và sử dụng cùng các nguyên tắc mã hóa. Tính đồng nhất này sẽ có giá trị khi tùy chỉnh giao thức cho giao tiếp giữa các phân đoạn, tuy nhiên Ethereum 2.0 sẽ không thể tương tác với các blockchai khác không sử dụng cùng các nguyên tắc mã hóa.

Mạng của các blockchain như Polkadot hoặc Cosmos đã tiến xa thêm một bước, cho phép các blockchain được xây dựng với cùng một khung (SDK Cosmos cho Cosmos và Substrate cho Polkadot) để giao tiếp và tương tác với nhau. Các khung này cung cấp cho các nhà phát triển một số tính linh hoạt trong việc xây dựng các giao thức của riêng họ và đảm bảo tính sẵn có của các nguyên hàm mã hóa giống hệt nhau, cho phép mỗi chuỗi phân tích các block của nhau và xác thực các giao dịch. Tuy nhiên, cả hai giao thức đều dựa vào các cầu nối hoặc "vùng peg" để kết nối với các blockchain không được xây dựng với mạng của chúng. Để chứng minh: mặc dù Cosmos và Polkadot cho phép "mạng lưới của các blockchain" hoạt động nhưng 2 mạng này không được thiết kế để có thể tương tác với nhau.

Kinh tế học tiền điện tử của các mạng lưới chuỗi chéo cũng có thể cần phải được nghiên cứu thêm. Đối với cả Cosmos và Polkadot, token gốc được sử dụng để stake, quản trị và giao dịch. Gác lại các động lực kinh tế tiền điện tử mang lại bởi stake – không thể 1 mìnhmang lại giá trị nội tại của token (thảo luận ở phần 4.2.4), việc phụ thuộc vào các giao dịch chuỗi chéo để nắm bắt giá trị hệ sinh thái có thể là một mô hình yếu. Cụ thể, giao dịch chuỗi chéo là một điểm yếu của mạng đa chuỗi, giống như giao dịch chéo là điểm yếu của cơ sở dữ liệu bị phân tách. Nó tại ra tính chậm trễ, cũng như mất tính nguyên tử và khả năng kết hợp. Có một xu hướng tự nhiên là các ứng dụng cần tương tác với nhau để cuối cùng sẽ chuyển sang cư trú trên cùng một blockchain để giảm chi phí chuỗi chéo, giảm nhu cầu giao dịch chuỗi chéo và cuối cùng dẫn tới giảm nhu cầu token.

Các mạng lưới chuỗi chéo được hưởng lợi từ các hiệu ứng mạng - càng có nhiều chuỗi kết nối trong mạng, mạng càng có giá trị và càng hấp dẫn hơn đối với những người tham gia mới. Lý tưởng nhất là chúng tôi muốn thấy giá trị này tích lũy vào token gốc, khuyến khích hơn nữa sự phát triển của mạng. Tuy nhiên, trong một mạng bảo mật được gộp chung như Polkadot, giá token gốc cao hơn làm tăng chi phí tham gia và trở thành yếu tố ngăn chặn mạng tích lũy thêm giá trị. Trong một mạng được kết nối lỏng lẻo như Cosmos, giá token cao hơn làm tăng chi phí trên 1 đồng thu được từ xác nhận giao dịch, làm giảm lợi nhuận kỳ vọng của vốn stake, không khuyến khích tham gia stake.

Với cách tiếp cận nhiều lớp, Nervos Network cũng là một mạng đa chuỗi. Về mặt kiến ​​trúc, Nervos sử dụng mô hình cell và một máy ảo cấp thấp để hỗ trợ tùy chỉnh và các nguyên tắc mã hóa do người dùng tạo ra, cho phép khả năng tương tác giữa các blockchain không đồng nhất (được nêu trong mục 4.4.1). Về mặt kinh tế, tiền điện tử, Nervos Network tập trung giá trị (thay vì truyền dữ liệu) vào chuỗi gốc của nó. Thông qua việc nắm bắt giá trị, token gốc của Nervos Network có thể được đánh giá cao, làm tăng ngân sách bảo mật của mạng khi giá trị tổng hợp được bảo mật bởi mạng tăng lên. Cuối cùng, giá token gốc tăng lên làm tăng giá trị cốt lõi của mạng. Điều này sẽ được đề cập chi tiết trong (mục 4.4).

## 3. Nguyên tắc cốt lõi của Nervos Network

Nervos là một mạng lưới các lớp được xây dựng để hỗ trợ nhu cầu của nền kinh tế phi tập trung. Có một số lý do mà chúng tôi tin rằng cách tiếp cận nhiều lớp là cách phù hợp để xây dựng mạng blockchain. Có nhiều sự đánh đổi phổ biến trong việc xây dựng các hệ thống blockchain chẳng hạn như phân cấp và khả năng mở rộng, trung lập so với tuân thủ, riêng tư so với công khai, lưu trữ giá trị so với chi phí giao dịch. Chúng tôi tin rằng tất cả các xung đột này phát sinh do các nỗ lực giải quyết các mối quan tâm hoàn toàn trái ngược với một blockchain duy nhất.

Chúng tôi tin rằng cách tốt nhất để xây dựng một hệ thống không phải là xây dựng một lớp duy nhất bao gồm tất cả, mà là để giải quyết các mối quan tâm và giải quyết chúng ở các lớp khác nhau. Bằng cách này, blockchain lớp 1 có thể tập trung vào việc bảo mật, trung lập, phi tập trung và cơ sở hạ tầng công cộng mở, trong khi đó các mạng lớp 2 nhỏ hơn có thể được thiết kế đặc biệt để phù hợp nhất với bối cảnh sử dụng của chúng.

Trong Nervos Network, giao thức lớp 1 (Common Knowledge Base) là lớp bảo toàn giá trị của toàn bộ mạng. Nó được truyền cảm hứng về mặt triết học bởi Bitcoin và là một blockchain mở, công khai và bằng chứng về công việc POW được thiết kế để bảo mật tối đa và chống kiểm duyệt cũng như được sử dụng như người cai quản phi tập trung về giá trị và tài sản tiền điện tử. Các giao thức lớp 2 tận dụng tính bảo mật của blockchain lớp 1 để cung cấp khả năng mở rộng không giới hạn và phí giao dịch tối thiểu, đồng thời cho phép tùy biến theo ứng dụng liên quan đến các mô hình đáng tin, quyền riêng tư và dứt khoát.

Dưới đây là các nguyên tắc cốt lõi dẫn đến việc thiết kế Nervos Network:

- Một blockchain lớp 1 đa tài sản bền vững phải được thiết kế dựa trên góc nhìn kinh tế để lưu trữ giá trị

- Lớp 2 cung cấp các tùy chọn mở rộng tốt nhất, mang lại khả năng giao dịch gần như không giới hạn, chi phí giao dịch tối thiểu và trải nghiệm người dùng được cải thiện. Các blockchains lớp 1 nên được thiết kế để bổ sung, không cạnh tranh với các giải pháp lớp 2.

- Proof of Work hay một phương pháp chống lại tấn công Sybil là yếu tố cần thiết cho các blockchain lớp 1.

- Blockchain lớp 1 phải cung cấp một mô hình lập trình chung cho các giao thức tương tác và khả năng tương tác của blockchain để cho phép giao thức có thể tùy chỉnh tối đa và dễ dàng nâng cấp.

- Để phân bổ tốt nhất các nguồn lực và tránh "thảm kịch chung", bộ lưu trữ dữ liệu phải có một mô hình sở hữu rõ ràng và chi tiết. Để cung cấp phần thưởng dài hạn nhất quán cho các thợ đào, và việc chiếm giữ bộ nhớ phải chịu chi phí liên tục.

## 4. Nervos Common Knowledge Base

### 4.1 Tổng quan

"Common knowledge" được định nghĩa là kiến ​​thức được mọi người hoặc gần như mọi người biết đến, thường có liên quan đến cộng đồng sử dụng thuật ngữ này. Trong bối cảnh của các blockchain nói chung và Nervos Network nói riêng, "kiến thức chung" đề cập đến trạng thái được xác minh bởi sự đồng thuận toàn cầu và được tất cả mọi người trong mạng chấp nhận.

Các thuộc tính của kiến ​​thức phổ thông cho phép chúng ta coi chung tiền điện tử được lưu trữ trên các blockchain công khai là tiền. Ví dụ: số dư và lịch sử của tất cả các địa chỉ trên Bitcoin là kiến ​​thức phổ biến cho người dùng Bitcoin, bởi vì họ có thể sao chép sổ cái, xác minh trạng thái toàn cầu kể từ block đầu tiên. Kiến thức chung này cho phép mọi người giao dịch hoàn toàn ngang hàng mà không đặt niềm tin vào bất kỳ bên thứ ba nào.

Cơ sở tri thức chung Nervos (CKB) được thiết kế để lưu trữ tất cả các loại kiến ​​thức chung, không giới hạn tiền bạc. Ví dụ: CKB có thể lưu trữ các tài sản tiền điện tử do người dùng xác định, chẳng hạn như token có thể thay thế và không thể thay thế, cũng như bằng chứng mật mã có giá trị cung cấp bảo mật cho các giao thức lớp cao hơn, như kênh thanh toán mục (5.2) và chuỗi Plasma mục (5.4).

Cả Bitcoin và Nervos CKB đều là hệ thống lưu trữ và xác minh kiến ​​thức chung. Bitcoin lưu trữ dữ liệu toàn cầu của nó dưới dạng UTXO và xác minh chuyển đổi trạng thái thông qua các quy tắc và tập lệnh được mã hóa cứng nhúng trong các giao dịch. Nervos CKB tổng quát hóa cấu trúc dữ liệu và khả năng lên script của Bitcoin, lưu trữ dữ liệu toàn cầu dưới dạng tập hợp các cell và xác nhận di chuyển state thông qua tập lệnh Turing – complete xác định bởi người dùng chạy trong máy ảo.

Mặc dù Nervos CKB đầy đủ tính năng hợp đồng thông minh như Ethereum và các nền tảng khác, mô hình kinh tế của nó được thiết kế để bảo tồn kiến ​​thức chung thay vì thanh toán cho tính toán phi tập trung.

### 4.2 Đồng thuận

Đồng thuận Nakamoto của Bitcoin (NC) được đón nhận do tính đơn giản và chi phí truyền thông thấp. Tuy nhiên, NC tồn tại hai nhược điểm: 1) thông lượng xử lý giao dịch của nó không đạt yêu cầu và 2) dễ bị tấn công khai thác, trong đó kẻ tấn công có thể nhận được phần thưởng block bổ sung bằng cách làm lệch khỏi hành vi quy định của giao thức.

Giao thức đồng thuận CKB là một biến thể của NC làm tăng giới hạn hiệu suất và khả năng chống tấn công trong khi vẫn giữ được giá trị của nó. Bằng cách xác định và loại bỏ nút cổ chai trong việc chậm trễ lan truyền block của NC, giao thức của chúng tôi hỗ trợ các khoảng block rất ngắn mà không làm mất đi tính bảo mật. Khoảng thời gian block được rút ngắn không chỉ làm tăng thông lượng mà còn giảm độ trễ xác nhận giao dịch. Bằng cách kết hợp tất cả các block hợp lệ vào tính toán điều chỉnh khó khăn, selfish mining (khai thác ích kỉ) không còn có lợi trong giao thức của chúng tôi.

#### 4.2.1 Tăng thông lượng

Nervos CKB làm tăng thông lượng của đồng thuận PoW với thuật toán đồng thuận có nguồn gốc từ đồng thuận Nakamoto. Thuật toán sử dụng tỷ lệ “khối bị bỏ rơi” (Orphan block xuất hiện khi cả 2 thợi mỏ cùng tìm thấy một khối hợp lên nhưng chỉ có một khối được nối tiếp vào hệ thống blockchain, khối còn lại sẽ bị bỏ rơi) của blockchain làm phép đo kết nối trên toàn mạng.

Giao thức nhắm đến một tỷ lệ khối bị bỏ rơi cố định. Khi tỉ lệ khối bị bỏ rơi thấp, chỉ tiêu độ khó được giảm xuống (tăng tỷ lệ sản xuất block) và khi tỉ lệ khối bị bỏ rơi vượt qua mực nhất định, chỉ tiêu độ khó được tăng lên (giảm tốc độ sản xuất block).

Điều này cho phép sử dụng toàn bộ khả năng băng thông của mạng. Tỷ lệ khối bị bỏ rơi thấp cho thấy mạng được kết nối tốt và có thể xử lý việc truyền dữ liệu lớn hơn; từ đó, giao thức sẽ tăng dần thông lượng.

#### 4.2.2 Loại bỏ cản trở sự lan truyền giữa các block

Sự lan truyền block là yếu tố cản trở trong bất kỳ mạng blockchain nào. Giao thức đồng thuận Nervos CKB loại bỏ những cản trở này bằng cách sửa đổi cách xác nhận giao dịch thành 2 bước: 1) đề xuất và 2) cam kết.

Một giao dịch trước tiên phải được đề xuất trong "vùng đề xuất" của một block (hoặc một trong các uncle block – tương tự như như khối bị bỏ rơi của Bitcoin). Giao dịch sau đó sẽ được cam kết nếu nó xuất hiện trong một cửa sổ được xác định thộc "vùng cam kết" của một block theo đề xuất của nó. Thiết kế này giúp loại bỏ tắc nghẽn lan truyền block, vì các giao dịch đã cam kết của một block mới sẽ được nhận và xác minh bởi tất cả các nút khi được đề xuất.

#### 4.2.3 Giảm thiểu các cuộc tấn công selfish mining

Một trong những cuộc tấn công cơ bản nhất vào Đồng thuận Nakamoto là “khai thác ích kỷ” (selfish mining). Trong cuộc tấn công này, những người khai thác độc hại có được phần thưởng block không công bằng bằng cách cố tình làm cho block được đào bởi người khác trở thành block bị bỏ rơi.

Các nhà nghiên cứu nhận thấy rằng cơ hội thu nhập không công bằng bắt nguồn từ cơ chế điều chỉnh sự khó khăn của Đồng thuận Nakamoto, trong đó đồng thuận này sẽ bỏ qua các block bị bỏ rơi khi ước tính sức mạnh tính toán của mạng. Điều này dẫn đến độ khó khai thác thấp hơn và làm tăng phần thưởng block bình quân theo thời gian.

Giao thức đồng thuận Nervos CKB kết hợp các uncle block (block bị bỏ rơi) vào tính toán điều chỉnh khó khăn, khiến việc khai thác ích kỷ không còn có lãi. Một người khai thác không thể đạt được những phần thưởng không công bằng thông qua bất kỳ sự kết hợp khai thác trung thực và ích kỷ nào.

Phân tích của chúng tôi cho thấy rằng với quy trình xác nhận giao dịch hai bước, việc khai thác ích kỷ trên thực tế cũng bị loại bỏ thông qua cửa sổ thời gian tấn công hạn chế.

Để hiểu chi tiết về giao thức đồng thuận của chúng tôi, xin vui lòng đọc [tại đây](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0020-ckb-consensus-protocol/0020-ckb-consensus-protocol.md).

#### 4.2.4 Proof of Work và Proof of Stake

Các hệ thống Proof of Work (PoW) và Proof of Stake (PoS) đều dễ bị ảnh hưởng bởi sự tập trung quyền lực, tuy nhiên đặc tính của mỗi hệ thống sẽ tạo ra cách thức hoạt động trên thực tế rất khác nhau cho những người nắm quyền lực.

Khai thác PoW các chi phí thực tế có thể lớn hơn số tiền khai thác. Những người nắm quyền phải luôn đổi mới, theo đuổi các chiến lược kinh doanh đúng đắn và tiếp tục đầu tư vào cơ sở hạ tầng để duy trì ưu thế. Thiết bị khai thác, vận hành pool đào và tiếp cận năng lượng giá rẻ đều phụ thuộc vào sự đổi mới về mặt công nghệ. Rất khó để duy trì sự độc quyền của cả ba trong thời gian dài.

Ngược lại, những người tạo block trong các hệ thống PoS được thưởng theo cách xác định, dựa trên số tiền stake, với yêu cầu vốn hoạt động rất thấp. Trong một hệ thống PoS, quyền lực có thể chỉ tập trung trong tay của một vài người stake. Mặc dù các hệ thống sử dụng PoW cũng gặp vấn đề tương tự, tuy nhiên chi phí để duy trì hệ thống PoS thấp hơn rất nhiều.

Ngoài ra, người xác nhận giao dịch PoS có khả năng đặc biệt: kiểm soát bộ những người xác nhận. Việc chấp nhận giao dịch cho phép 1 người xác nhận tham gia nhóm đồng thuận đang được kiểm soát bởi những người xác nhận hiện có. Những nỗ lực thông đồng để gây ảnh hưởng đến nhóm người xác nhận thông qua kiểm duyệt và thao túng giao dịch sẽ khó bị phát hiện, cũng như khó trừng phạt. Ngược lại, đồng thuận PoW thực sự mở và không phụ thuộc vào cấu trúc quyền lực hiện tại. Người tham gia vào mạng lưới dù sớm hay muộn cũng sẽ bình đẳng với nhau.

Về nền kinh tế token, trong khi mọi người tin rằng việc stake có thể thu hút các nguồn vốn có nhu cầu kiếm thêm lợi nhuận (từ đó làm tăng nhu cầu về token), tuy nhiên, đây không phải là tất cả. Tất cả các dự án PoS cuối cùng sẽ nhận thấy rằng tỷ lệ stake của họ giữ ở mức ổn định, lượng vốn đổ vào và rời khỏi stake pool sẽ dần cân bằng. Cơ chế stake sẽ không làm tăng nhu cầu đối với token. Nói cách khác, stake có thể làm tăng nhu cầu token trong giai đoạn ban đầu của dự án (khi tỷ lệ stake tăng), tuy nhiên nó không thể tạo ra nhu cầu token trong dài hạn. 

Người sở hữu token dài hạn trong hệ thống PoS có 3 lựa chọn: họ có thể 1) quản lý cơ sở hạ tầng và tự mình chạy một node xác nhận giao dịch để nhận thưởng 2) ủy thác token cho bên thứ ba, tin tưởng vào sự trung thực và cơ sở hạ tầng của họ, hoặc 3) có giá trị token bị giảm đi do các đợt phát hành mới. Không có lựa chọn nào ở trên là hấp dẫn đối với những người mong muốn giữ token lâu dài.

Chúng tôi tin rằng cơ chế tham gia không cần cho phép của PoW là một yêu cầu đối với cơ sở hạ tầng của nền tảng phục vụ hoạt động của kinh tế toàn cầu. Mục tiêu quan trọng nhất của lớp 1 là đảm bảo rằng blockchain được phân cấp, an toàn và trung lập nhất có thể. Mặc dù các hệ thống PoS có vai trò trong nền kinh tế phi tập trung, nhưng theo chúng tôi, chúng không đáp ứng các yêu cầu của lớp 1 thực sự mở và phi tập trung.

#### 4.2.5 Bằng chứng về chức năng làm việc

Các block CKB của Nervos có thể được đào bởi bất kỳ nút nào, miễn là 1) block đó hợp lệ; và 2) người đề xuất đã giải được câu đố (bằng chứng công việc). Câu đố POW này được xác định theo từng block đang được giải; đảm bảo rằng đáp án của mỗi câu đố tại mỗi block là khác nhau. 

Proof-of-work của Bitcoin đòi hỏi phải tìm ra một nonce hợp lệ sao cho kết quả của việc áp dụng hàm băm trên tiêu đề block thỏa mãn độ khó nhất định. Đối với Bitcoin, hàm băm là SHA2-256 lặp lại hai lần. Mặc dù SHA2 là một lựa chọn hoàn hảo cho Bitcoin, nhưng công dụng của nó sẽ không được phát huy khi áp dụng vào các loại hình tiền mã hóa sau này. Hàng loạt phần cứng chuyên dụng đã được phát triển để khai thác Bitcoin, hầu hết trong số đó bị lãng phí, đã lỗi thời.

Một loại tiền điện tử mới ra đời sử dụng proof-of-work sẽ một lần nữa tận dụng sức mạnh phần cứng. Thậm chí, các phần cứng hiện đại cũng có thể được cho thuê và tái sử dụng để khai thác một loại tiền mới. Việc phân phối sức mạnh khai thác cho một đồng tiền dựa trên SHA2 sẽ rất khó dự đoán và dễ bị thay đổi đột ngột. Lý do này cũng áp dụng cho việc tối ưu hóa thuật toán phù hợp với SHA2 – hiện cũng được được phát triển để giúp cho tính toán phần mềm trở nên rẻ hơn.

Đối với một loại tiền điện tử mới, việc hình thành câu đố POW chưa từng dược dùng bởi bất kỳ một đồng tiền mã hóa nào là cần thiết. Nervos CKB đã tiến 1 bước xa hơn và chọn định nghĩa nó theo chức năng bằng chứng công việc (POW) – chưa từng được tối ưu hóa bởi vì nó còn quá mới. 

Dự kiến rằng, không có nhiều phần cứng tham gia đào sẽ chỉ xảy ra ở thời điểm ban đầu. Về lâu dài, việc triển khai đào phần cứng chuyên dụng rất có lợi, nó làm tăng đáng kể các thách thức tấn công mạng. Do đó, ngoài việc nó là giải pháp mới, cơ chế POW lý tưởng cho một loại tiền điện tử mới cũng rất đơn giản, góp phần làm giảm đáng kể rào cản phát triển phần cứng.

Tính bảo mật hiển nhiên là mục tiêu thứ 3 trong quá trình xây dựng. Mặc dù biết rằng, lỗ hổng được biết tới có thể khai thác bởi bất kỳ người đào nào, đồng nghĩa với việc độ khó sẽ tăng theo. Một lỗ hổng chưa tiết lộ sẽ tối ưu hóa việc đào và cung cấp những người đào lợi thế vượt qua sức mạnh đào mà họ đóng góp. 

#### 4.2.6 Eaglesong

Eaglesong là một hàm băm mới được phát triển dành riêng cho proof-of-work của Nervos CKB, nhưng cũng phù hợp trong các trường hợp ứng dụng khác khi các trường hợp này cần có hàm băm an toàn. Các tiêu chí thiết kế chính xác như được liệt kê ở trên như: tính mới, đơn giản và bảo mật. Chúng tôi muốn một thiết kế đủ mới lạ để tạo ra một bước tiến nhỏ cho khoa học, đồng thời đủ gần với các thiết kế hiện có để tạo ra tranh luận về tính bảo mật.

Để làm được điều đó, chúng tôi đã chọn khởi tạo cấu trúc bọt biển (như được sử dụng trong Keccak / SHA3) với hoán vị được xây dựng từ các hoạt động ARX (thêm, xoay và xor); bảo mật của nó dựa trên chiến lược đường rộng (cùng một đối số nằm dưới AES).

Theo chúng tôi, Eaglesong là hàm băm đầu tiên kết hợp thành công cả ba nguyên tắc thiết kế.

Bạn có thể đọc thêm về Eaglesong [ở đây](https://medium.com/nervosnetwork/the-proof-of-work-function-of-nervos-ckb-3cc8364464d9).

### 4.3 Mô hình tế bào (mô hình Cell)

Nervos CKB sử dụng mô hình Cell một cấu trúc mới có thể cung cấp nhiều lợi ích của mô hình Account (được sử dụng trong Ethereum), trong khi vẫn giữ quyền sở hữu tài sản và các thuộc tính xác minh dựa trên bằng chứng của mô hình UTXO (được sử dụng trong Bitcoin).

Mô hình Cell tập trung vào state. Cell chứa dữ liệu bất kỳ, chẳng hạn như số lượng token và chủ sở hữu hoặc phức tạp hơn, chẳng hạn như mã chỉ định các điều kiện xác minh để chuyển token. Máy state của CKB thực thi các tập lệnh được liên kết với các cell để đảm bảo tính toàn vẹn của quá trình di chuyển state.

Ngoài việc lưu trữ dữ liệu trong chính ô đó, các ô có thể tham chiếu dữ liệu trong các ô khác. Điều này cho phép các tài sản thuộc sở hữu của người dùng và logic quản lý được phân tách. Điều này trái ngược với các nền tảng hợp đồng thông minh dựa trên tài khoản, trong đó state là tài sản nội bộ của hợp đồng thông minh và phải được truy cập thông qua giao diện hợp đồng thông minh. Trên Nervos CKB, cell là các đối tượng state độc lập được sở hữu và có thể được tham chiếu và chia sẻ trực tiếp. 

Giao dịch mô hình cell cũng là bằng chứng chuyển tiếp trạng thái. Các cell lưu trữ đầu vào của 1 giao dịch được xóa khỏi tập hợp các cell đang hoạt động và các cell đầu ra được thêm vào nhóm. Các cell hoạt động bao gồm state toàn cầu của Nervos CKB và không bao giờ thay đổi một khi các cell đã được tạo.

Mô hình Cell được thiết kế để có thể thích ứng, bền vững và linh hoạt. Nó có thể được mô tả như một mô hình UTXO tổng quát và có thể hỗ trợ các token do người dùng xác định, hợp đồng thông minh và các giao thức lớp 2 đa dạng.

Để hiểu sâu hơn về Mô hình Cell, vui lòng xem [tại đây](https://medium.com/nervosnetwork/https-medium-com-nervosnetwork-cell-model-7323fca57571).

### 4.4 Máy ảo

Trong khi nhiều dự án blockchain thế hệ mới sử dụng WebAssugging làm nền tảng của máy ảo blockchain, Nervos CKB lựa chọn thiết kế độc đáo của máy ảo (CKB-VM) dựa trên tập lệnh RISC-V.

RISC-V là một kiến ​​trúc tập lệnh RISC mã nguồn mở được tạo ra vào năm 2010 để tạo điều kiện phát triển phần cứng và phần mềm mới, hoàn toàn miễn phí, được biết tới rộng rãi.
Chúng tôi nhận ra nhiều lợi thế khi ứng dụng RISC-V vào blockchain:

- Tính ổn định: Bộ hướng dẫn RISC-V đã được hoàn thiện và cố định, cũng như được triển khai và thử nghiệm rộng rãi. Tập lệnh RISC-V cốt lõi được cố định và sẽ không bao giờ yêu cầu cập nhật.

- Cấu trúc mở và được hỗ trợ: RISC-V được cấp giấy phép BSD và được hỗ trợ bởi các trình biên dịch như GCC và LLVM, ứng dụng ngôn ngữ Rust và Go đang được xây dựng. Đội ngũ của RISC-V bao gồm hơn 235 tổ chức thành viên vẫn đang hoàn thiện thêm tập lệnh và hướng dẫn.

- Tính đơn giản và mở rộng: Tập lệnh RISC-V rất đơn giản. Với sự hỗ trợ cho số nguyên 64 bit, bộ chỉ chứa 102 tập hướng dẫn. RISC-V cũng cung cấp một cơ chế mô-đun cho các tập lệnh mở rộng, cho phép khả năng tính toán véc tơ hoặc số nguyên 256 bit cho các thuật toán mã hóa hiệu suất cao.

- Định giá tài nguyên chính xác: Tập lệnh RISC-V có thể được chạy trên CPU vật lý, cung cấp ước tính chính xác về chu kỳ máy cần thiết để thực hiện từng lệnh và thông báo giá tài nguyên máy ảo.

CKB-VM là một máy ảo RISC-V cấp thấp cho phép tính toán linh hoạt, Turning-complete. Thông qua việc sử dụng rộng rãi định dạng ELF, các tập lệnh CKB-VM có thể được phát triển với bất kỳ ngôn ngữ nào được biên dịch theo tập lệnh của RISC-V.

#### 4.4.1 Máy ảo CKB và mô hình Cell

Sau khi được triển khai, các blockchains công cộng hiện tại ít nhiều được cố định. Nâng cấp các yếu tố nền tảng, chẳng hạn như các thành phần mật mã cơ sở, cần rất nhiều năm hoặc đơn giản là không thể làm được.

CKB-VM lùi một bước và di chuyển các cơ sở được tích hợp trước đó vào các cell trên các máy ảo tùy chỉnh. Mặc dù các tập lệnh CKB ở mức độ thấp hơn các hợp đồng thông minh trong Ethereum nhưng chúng mang lại lợi ích đáng kể về tính linh hoạt.

Cell thể lưu trữ mã thực thi và tham chiếu các ô khác dưới dạng phụ thuộc. Hầu như tất cả các thuật toán và cấu trúc dữ liệu được triển khai dưới dạng các tập lệnh CKB trong các ô. Đơn giản hóa VM nhất có thể và giảm lưu trữ chương trình cho các ô, cập nhật thuật toán chính cũng đơn giản như tải thuật toán vào 1 cell mới và cập nhật các tham chiếu hiện có.

#### 4.4.2 Chạy các máy ảo khác trên CKB-VM

Nhờ tính chất cấp thấp của CKB-VM và tính khả dụng của công cụ trong cộng đồng RISC-V, bạn có thể dễ dàng thiết lập các VM khác (như EVM của Ethereum) trực tiếp vào CKB-VM. Điều này có một số lợi thế như:

- Các hợp đồng thông minh được viết bằng ngôn ngữ chuyên dụng chạy trên các máy ảo khác có thể dễ dàng được chuyển sang chạy trên CKB-VM. (Nói đúng ra, chúng sẽ chạy trên máy ảo được biên dịch để chạy bên trong CKB-VM.)

- CKB có thể xác minh chuyển đổi trạng thái giải quyết tranh chấp của các giao dịch lớp 2, ngay cả khi các quy tắc chuyển đổi state được viết để chạy trong một máy ảo khác với CKB-VM. Đây là một trong những yêu cầu chính để hỗ trợ chuỗi bên không cần sự tin cậy. 

Để biết hướng dẫn kỹ thuật của CKB-VM, vui lòng xem [tại đây](https://medium.com/nervosnetwork/an-introduction-to-ckb-vm-9d95678a7757).

### 4.5 Mô hình kinh tế 

Token của Nervos CKB là " Common Knowledge Byte " hay viết tắt là CKByte. CKBytes cho phép chủ sở hữu token chiếm một phần trong tổng lưu trữ trạng thái của blockchain. Ví dụ: bằng cách giữ 1000 CKBytes, người dùng có thể tạo một ô có dung lượng 1000 byte hoặc nhiều ô có dung lượng tối đa 1000 byte.

Sử dụng CKBytes để lưu trữ dữ liệu trên CKB tạo ra chi phí cơ hội cho chủ sở hữu CKByte; chúng sẽ không thể gửi các CKBytes đã sử dung vào NervosDAO để nhận một phần của đợt phát hành thứ cấp. CKBytes có giá thị trường, và do đó phần thưởng được cung cấp cho người dùng để tự nguyện tạo ra lưu trữ stage đáp ứng nhu cầu phát sinh. Sau khi người dùng giải phóng bộ nhớ state, họ sẽ nhận được một lượng CKBytes tương đương với kích thước state (tính bằng byte) mà dữ liệu của họ đang chiếm giữ.

Mô hình kinh tế của CKB cho phép phát hành cho sự tăng trưởng state, duy trì rào cản tham gia mức thấp và đảm bảo phân cấp. Khi CKBytes trở thành một nguồn tài nguyên khan hiếm, chúng có thể được định giá và phân bổ theo cách hiệu quả nhất.

Block đầu tiên của Nervos Network sẽ chứa 33,6 tỷ CKBytes, trong đó 8.4 tỷ sẽ bị đốt cháy ngay lập tức. Phát hành mới của CKBytes bao gồm hai phần - phát hành cơ sở và phát hành thứ cấp. Phát hành cơ sở được giới hạn trong tổng nguồn cung hữu hạn (33,6 tỷ CKBytes), với lịch phát hành tương tự như Bitcoin. Phần thưởng block giảm một nửa sau mỗi 4 năm. Tất cả các phát hành cơ sở được trao cho các thợ mỏ dưới dạng phần thưởng duy trì mạng. Phát hành thứ cấp có tỷ lệ phát hành không đổi 1,344 tỷ CKBytes mỗi năm và được thiết kế để áp đặt chi phí cơ hội cho nắm giữ state. Sau khi phát hành cơ sở kết thúc, sẽ chỉ có phát hành thứ cấp sản sinh ra token.

Nervos CKB bao gồm một hợp đồng thông minh đặc biệt gọi là NervosDAO, có chức năng như một "nơi trú ẩn lạm phát" chống lại các tác động của đợt phát hành thứ cấp. Chủ sở hữu CKByte có thể gửi token của họ vào NervosDAO và nhận một phần phát hành thứ cấp chính xác bù đắp hiệu ứng lạm phát từ phát hành thứ cấp. Đối với những người nắm giữ token dài hạn, miễn là họ khóa token của họ trong NervosDAO, hiệu ứng lạm phát của việc phát hành thứ cấp chỉ là danh nghĩa. 

Trong khi CKBytes đang được sử dụng để lưu trữ trạng thái, chúng không thể được sử dụng để kiếm phần thưởng phát hành thứ cấp thông qua NervosDAO. Điều này làm cho việc ban hành thứ cấp trở thành một loại thuế lạm phát liên tục, hoặc "tiền thuê nhà nước" đối với nghề lưu trữ nhà nước. Mô hình kinh tế này áp đặt phí lưu trữ nhà nước tỷ lệ thuận với cả không gian và thời gian chiếm đóng. Nó bền vững hơn mô hình "trả một lần, chiếm mãi mãi" được sử dụng bởi các nền tảng khác khả thi và thân thiện với người dùng hơn các giải pháp thuê nhà nước khác yêu cầu thanh toán rõ ràng.

Người đào được nhận cả phần thưởng block và phí giao dịch. Đối với phần thưởng block, khi người đào giải được một block, họ sẽ nhận được phần thưởng phát hành của block và một phần phát hành thứ cấp. Phần này dựa trên lưu trữ dữ liệu của state, ví dụ: nếu một nửa số token được sử dụng để lưu trữ state, người đào sẽ nhận được một nửa phần thưởng phát hành thứ cấp cho block. Thông tin bổ sung về việc phân phối phát hành thứ cấp được bao gồm trong phần tiếp theo (4.6) Về lâu dài, khi việc phát hành cơ sở dừng lại, các người đào vẫn sẽ nhận được thu nhập "cho thuê sstate" mà không phụ thuộc vào giao dịch, nhưng gắn liền với việc ứng dụng của Nervos Common Knowledge Base. 

Tương tự, CKBytes có thể xem như là đất, trong khi tài sản tiền điện tử được lưu trữ trên CKB có thể được coi là nhà. Cần có đất để xây nhà, và CKBytes được yêu cầu lưu trữ tài sản trên CKB. Khi nhu cầu lưu trữ tài sản trên CKB tăng, nhu cầu về CKBytes cũng tăng theo. Khi giá trị của tài sản được lưu trữ tăng lên, giá trị của CKBytes cũng tăng theo.

Với thiết kế này, nhu cầu về vô số tài sản được chuyển thành nhu cầu cho một tài sản duy nhất, và hình thức thưởng tương tự như Bitcoin đang sử dụng để bảo toàn hệ thống cũng có thể được áp dụng. Người đào được trả phần thưởng khối bằng CKB, sẽ tăng tương ứng khi nhu cầu tăng. Đồng nghĩa với việc, ngân sách bảo mật của Nervos Network cũng tăng lên.

Để được giải thích chi tiết hơn về mô hình kinh tế, vui lòng xem [tại đây](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0015-ckb-cryptoeconomics/0015-ckb-cryptoeconomics.md).

### 4.6 Kho bạc

Phần phát hành thứ cấp không thuộc về 1) người đào hoặc 2) người nắm giữ dài hạn với token bị khóa trong NervosDAO thì sẽ được sẽ chuyển sang quỹ ngân quỹ. Ví dụ: nếu 60% các CKBytes phát hành được sử dụng để lưu state và 30% các CKBytes được gửi vào NervosDAO, người đào sẽ nhận được 60% phát hành thứ cấp, NervosDAO (người nắm giữ dài hạn) sẽ nhận được 30% phát hành thứ cấp, và 10% phát hành thứ cấp sẽ được đưa vào kho bạc.

Kho bạc sẽ được sử dụng để tài trợ cho nghiên cứu và phát triển giao thức đang hoạt động cũng như phát triển hệ sinh thái của mạng lưới. Việc sử dụng ngân quỹ sẽ được công khai, minh bạch và trực tuyến cho mọi người. So với mô hình tài trợ ngân quỹ dựa trên khía cạnh lạm phát, mô hình này không làm giảm tài sản các chủ sở hữu token dài hạn (những người đã gửi token của họ vào NervosDAO). Tài trợ cho việc phát triển giao thức được xây đựng dựa theo chi phí cơ hội cho những người nắm giữ token ngắn hạn.

Kho bạc sẽ không được kích hoạt ngay khi ra mắt mainnet của Nervos Common Knowledge Base. Sau này, nó sẽ được kích hoạt bằng một hard-fork và nền tảng Nevos đã sử dụng hết quỹ hệ sinh thái bao gồm trong block Genesis. Trước khi kích hoạt kho bạc, phần phát hành thứ cấp này sẽ bị đốt cháy.

### 4.7 Quản trị

Quản trị là cách xã hội hoặc các nhóm trong một tổ chức đưa ra quyết định. Mỗi bên liên quan quan tâm đến hệ thống nên được tham gia vào quá trình này. Trong blockchain, nhóm này chỉ bao gồm người dùng, chủ sở hữu, người đào, nhà nghiên cứu và nhà phát triển mà còn cả các nhà cung cấp dịch vụ như ví, sàn và các bể đào. Các nhóm khác nhau có lợi ích đa dạng và gần như việc tổng hòa lợi ích của mọi người là gần như không thể. Đây là lý do tại sao quản trị blockchain là một chủ đề phức tạp và gây tranh cãi. Nếu chúng ta coi blockchain là một thử nghiệm xã hội lớn, quản trị đòi hỏi một thiết kế tinh vi hơn bất kỳ phần nào khác của hệ thống. Sau mười năm phát triển, chúng ta vẫn chưa xác định được các thực tiễn chung tốt nhất hoặc các quy trình bền vững để quản trị blockchain.

Một số dự án tiến hành quản trị thông qua một "nhà độc tài" (như Linus Torvalds cho Linux). Chúng tôi thừa nhận rằng điều này làm cho một dự án đạt hiệu quả cao, gắn kết và cũng hấp dẫn: mọi người yêu thích các anh hùng; tuy nhiên, điều này mâu thuẫn với tính phân cấp - giá trị cốt lõi của blockchain.

Một số dự án ủy thác một ủy ban ngoài chuỗi quyền quyết định tối cao, chẳng hạn như ECAF (Diễn đàn trọng tài cốt lõi của EOSIO) trên EOS. Tuy nhiên, các ủy ban này thiếu sức mạnh thiết yếu để đảm bảo những người tham gia sẽ tuân thủ các quyết định của họ, điều này có thể là lý do then chốt cho việc đóng cửa ECAF vào đầu năm nay.

Một số dự án chẳng hạn như Tezos, đi xa hơn và thực hiện quản trị trên chuỗi để đảm bảo tất cả những người tham gia tuân thủ bỏ phiếu theo quyết định. Điều này cũng tránh mọi tác động của sự bất hòa giữa nhà phát triển và người đào (hay người dùng full node). Lưu ý rằng quản trị trên chuỗi khác với bỏ phiếu theo chuỗi đơn giản, nếu một tính năng được đề xuất đã có đủ phiếu thông qua quản trị trên chuỗi, mã chuỗi sẽ được cập nhật tự động, người đào hoặc nút đầy đủ sẽ không thể có cách nào quản lý sự thay đổi này. Polkadot thực hiện một cách tiếp cận thậm chí tinh vi hơn đối với quản trị theo chuỗi, sử dụng một hội đồng được bầu, quy trình trưng cầu dân ý để bỏ phiếu có trọng số và các cơ chế thiên vị tích cực / tiêu cực để giải quyết cho cử tri đi bầu.

Tuy nhiên, bất chấp sự đơn giản của nó, quản trị theo chuỗi trong thực tế không rõ ràng như nó được trình bày. Trước hết, phiếu bầu chỉ phản ánh sự quan tâm của người nắm giữ và bỏ qua tất cả các bên liên quan còn lại. Thứ hai, tỷ lệ bỏ phiếu thấp là một vấn đề tồn tại lâu dài trong cả thế giới blockchain và thế giới thực. Làm thế nào kết quả có thể là lợi ích tốt nhất nếu chỉ có một cuộc bỏ phiếu? Và quan trọng nhất, một hard fork phải luôn được coi là sự cầu viện cuối cùng cho tất cả các bên liên quan. Với tính khả dụng của dữ liệu tuyệt vời được cung cấp bởi sự sao chép rộng rãi của một blockchain mở, việc fork chuỗi hiện có mà vẫn bảo toàn dữ liệu đầy đủ sẽ luôn là một giải pháp. Một hard fork không bao giờ có thể được thực hiện thông qua quản trị trên chuỗi.

Vẫn chưa có câu trả lời khả thi cho các câu hỏi về quản trị, vì vậy đối với Nervos Network, chúng tôi sẽ có một cách tiếp cận phát triển hơn. Trong những ngày đầu, quỹ Nervos sẽ đảm nhận vai trò là cơ quan chủ quản của dự án. Quỹ Nervos là một quỹ Panama với một hội đồng độc lập. Nền tảng được giao nhiệm vụ tiếp tục phát triển mạng lưới Nervos và xây dựng hệ sinh thái.

Theo thời gian, khi nhiều token được đào, việc đào trở nên phân tán hơn và nhiều nhà phát triển tham gia hơn, trách nhiệm quản trị sẽ dần chuyển sang phạm vi toàn cộng đồng. Về lâu dài, quản trị dựa vào cộng đồng sẽ quản lý quá trình nâng cấp giao thức và phân bổ nguồn lực từ kho bạc.
Nervos CKB được thiết kế để trở thành cơ sở hạ tầng tự trị phi tập trung có thể tồn tại hàng trăm năm, điều đó có nghĩa là có những điều nhất định đòi hỏi nỗ lực tốt nhất của chúng ta như một cộng đồng để giữ vững sự thật, bất kể mạng lưới này phát triển như thế nào. 3 bất biến cốt lõi là:

- Lịch phát hành là hoàn toàn cố định, do đó sẽ không bao giờ thay đổi.

- Trạng thái / dữ liệu được lưu trữ trong các ô sẽ không bị làm giả.

- Ngữ nghĩa hiện tại của kịch bản sẽ không được thay đổi.

Quản trị dựa trên cộng đồng cho blockchains là một lĩnh vực rất mới và có nhiều thử nghiệm đáng giá đang diễn ra. Chúng tôi nhận ra rằng đây không phải là chủ đề bình thường mà nó cần có thời gian để nghiên cứu, quan sát và lặp lại đầy đủ để đi đến một phương pháp tối ưu. Chúng tôi đang thực hiện một cách tiếp cận thận trọng đối với quản trị dựa vào cộng đồng trong ngắn hạn trong khi vẫn hoàn toàn cam kết phát triển dài hạn theo hướng đi này

## 5. Tổng quan về các giải pháp layer 2 (lớp 2)

### 5.1 Layer 2 (lớp 2) là gì? 

Lớp 1 của mạng blockchain được xác định bởi các ràng buộc. Một blockchain lớp 1 (layer 1) lý tưởng không làm ảnh hưởng đến bảo mật, phân cấp và tính bền vững, tuy nhiên, nó tạo ra những thách thức liên quan đến khả năng mở rộng và chi phí giao dịch. Các giải pháp lớp 2 được xây dựng dựa trên các giao thức lớp 1, cho phép tính toán được chuyển ra ngoài chuỗi (tính toán được chuyển qua off-chain) và sau đó được chuyển trở lại chuỗi khối 1 một cách an toàn. 

Điều này tương tự như thanh toán ròng trong hệ thống ngân hàng ngày nay hoặc hồ sơ pháp lý bắt buộc của SEC. Bằng cách giảm lượng dữ liệu cần sự đồng thuận toàn cầu, mạng lưới có thể phục vụ nhiều người tham gia hơn và tạo điều kiện cho nhiều hoạt động kinh tế hơn, trong khi vẫn duy trì các thuộc tính phi tập trung.

Người dùng lớp 2 phụ thuộc vào bảo mật được cung cấp bởi blockchain lớp 1 và sử dụng bảo mật này khi di chuyển tài sản giữa các lớp hoặc giải quyết tranh chấp. Chức năng này tương tự như một hệ thống “tòa án”: “tòa án” không phải giám sát và xác nhận tất cả các giao dịch, mà chỉ đóng vai trò là nơi ghi lại bằng chứng quan trọng và giải quyết tranh chấp. Tương tự, blockchain lớp 1 cho phép người tham gia giao dịch ngoại tuyến và trong trường hợp bất đồng xảy ra, họ có khả năng cung cấp bằng chứng mật mã vào blockchain và giao dịch không trung thực sẽ bị xử phạt.

### 5.2 Thanh toán và State channel

Các kênh thanh toán được tạo ra giữa hai bên giao dịch thường xuyên. Chúng cung cấp trải nghiệm thanh toán ngay lập tức, độ trễ thấp mà các giao dịch được thực hiện trực tiếp trên blockchain toàn cầu không bao giờ có thể cung cấp. Các kênh thanh toán hoạt động tương tự như hình thức tab trên các quầy bar - bạn có thể mở 1 tab với nhân viên pha chế và gọi đồ uống, sau đó thanh toán tổng số tiền đồ uống khi bạn rời khỏi quán. Trong hoạt động của kênh thanh toán, những người tham gia trao đổi tin nhắn có chứa các cam kết mật mã về số dư của họ và có thể cập nhật số dư này không giới hạn số lần ngoài chuỗi (off-chain), trước khi họ sẵn sàng chốt lại và giải quyết số dư trên blockchain.

Các kênh thanh toán hai chiều thường phức tạp hơn, tuy nhiên nó lại mở ra khả năng ứng dụng của các công nghệ lớp 2. Trong các kênh thanh toán này, dòng tiền chảy qua lại giữa các bên, cho phép "tái cân bằng" các kênh thanh toán và mở ra khả năng thanh toán trên các kênh thông qua một đối tác chia sẻ. Điều này cho phép kết nối các mạng thanh toán, chẳng hạn như Mạng Lightning của Bitcoin. Tiền có thể được chuyển từ Bên A sang Bên B mà không cần một kênh trực tiếp giữa họ, miễn là Bên A có thể tìm thấy trung gian với các kết nối mở cho cả hai bên.

Giống như các kênh thanh toán có thể mở rộng quy mô thanh toán trên chuỗi, state channel có thể mở rộng bất kỳ giao dịch trực tuyến nào. Trong khi một kênh thanh toán bị giới hạn trong việc quản lý số dư giữa hai bên, kênh trung tâm sẽ là đồng thuận của state tùy ý, cho phép ứng dụng kể cả trò chơi cờ vua không cần niềm tin (hai bên có thể yên tâm vào kết quả mà không cần niềm tin về việc đối thủ sẽ không gian lận) đến các ứng dụng phi tập trung có thể mở rộng.

Tương tự như kênh thanh toán, các bên mở ra một kênh, trao đổi chữ ký mã hóa theo trong một khoảng thời gian và gửi “trạng thái” cuối cùng (hay kết quả) cho hợp đồng thông minh trên chuỗi. Hợp đồng thông minh sau đó sẽ thực hiện dựa trên đầu vào này, giải quyết giao dịch theo các quy tắc được mã hóa trong hợp đồng.

"State channel tổng quát” là một cấu trúc mạnh, cho phép một state channel chính hỗ trợ chuyển state qua nhiều hợp đồng thông minh. Điều này làm giảm nguy cơ “phình to” state vốn có trong kiến trúc "một kênh trên mỗi ứng dụng" và cũng cho phép dễ dàng để nhập dữ liệu mới với khả năng sử dụng các state channel mà người dùng đã mở sẵn.

### 5.3 Chuỗi phụ

Chuỗi phụ là một chuỗi khối riêng biệt được gắn với chuỗi khối không cần niềm tin (chuỗi chính) và chốt hai chiều. Để sử dụng chuỗi phụ, người dùng sẽ gửi tiền đến một địa chỉ được chỉ định trên chuỗi chính, khóa các khoản tiền này dưới sự kiểm soát của các người quản lý chuỗi phụ. Khi giao dịch này được xác nhận, một bằng chứng chi tiết về việc gửi tiền được chuyển tới người chuỗi phụ. Những người quản lý chuỗi phụ sẽ tạo ra một giao dịch trên chuỗi phụ, phân phối các khoản tiền phù hợp. Những khoản tiền này sau đó có thể được sử dụng trên chuỗi phụ với phí thấp, xác nhận nhanh và thông lượng cao.

Hạn chế chính của chuỗi phụ là chúng yêu cầu các cơ chế bảo mật và giả định bảo mật bổ sung. Chuỗi bên liên kết - cấu trúc chuỗi bên đơn giản nhất - đặt niềm tin vào một nhóm người quản lý dưới hình thức đa chữ ký. Trên nền tảng hợp đồng thông minh, các mô hình bảo mật có thể được điều chỉnh với các ưu đãi token hoặc liên kết / thử thách / cắt giảm các trò chơi kinh tế.

So với các giải pháp mở rộng sử dụng giải pháp off-chain khác, chuối phụ dễ hiểu và dễ thực hiện hơn. Đối với các loại ứng dụng cho phép tạo mô hình tin cậy được chấp nhận bởi người dùng, chuỗi phụ có thể là một giải pháp có thể thực hiện được.

### 5.4 Chuỗi cam kết

Trên các chuỗi cam kết (Commit – chain) [6], chẳng hạn như Plasma [7], chuỗi 2 lớp được xây dựng nhằm tận dụng gốc tin cậy trên chuỗi khối 1 (chuỗi gốc) với sự đồng thuận rộng rãi trên toàn cầu. Các chuỗi cam kết này rất an toàn; trong trường hợp một người quản lý chuỗi bị phát hiện là độc hại hoặc rối loạn chức năng, người dùng luôn có thể rút tài sản của họ thông qua một cơ chế trên chuỗi gốc.

Một nhà điều hành chuỗi cam kết được tin cậy để thực hiện các giao dịch một cách chính xác và cho ra các bản cập nhật định kỳ cho chuỗi gốc. Trong mọi điều kiện, ngoại trừ cuộc tấn công kiểm duyệt kéo dài vào chuỗi gốc, tài sản trên chuỗi cam kết sẽ vẫn an toàn. Tương tự như chuỗi bên liên kết, thiết kế chuỗi cam kết cung cấp trải nghiệm người dùng vượt trội so với blockchain không cần niềm tin. Tuy nhiên, họ làm như vậy trong khi duy trì đảm bảo an ninh mạnh mẽ hơn.

Chuỗi cam kết được bảo mật bằng một tập hợp các hợp đồng thông minh chạy trên chuỗi gốc. Người dùng gửi tài sản vào hợp đồng này và người điều hành chuỗi cam kết sẽ cung cấp cho họ tài sản trên chuỗi cam kết. Người điều hành sẽ định kỳ công bố các cam kết với chuỗi gốc, mà sau đó người dùng có thể sử dụng để chứng minh quyền sở hữu tài sản thông qua bằng chứng Merkle, trong đó tài sản chuỗi cam kết được rút về chuỗi gốc.

Trên đây là mô tả khái niệm chung về thiết kế chuỗi cam kết, cơ sở của sự kết hợp các protocol bao gồm cả Plasma. Plasma white paper [7] do Vitalik Buterin và Joseph Poon phát hành năm 2017 đã đưa ra một tầm nhìn đầy tham vọng. Mặc dù tất cả các chuỗi Plasma hiện đang dựa trên tài sản và chỉ có thể lưu trữ (chuyển) quyền sở hữu quyền sở hữu token fungible (tiền mã hóa) và non - fungible token (chứng nhận kỹ thuật số), thực thi code không cần sự tin cậy (hoặc hợp đồng thông minh) là một lĩnh vực đang được tích cực nghiên cứu.

### 5.5 Các tính toán ngoài chuỗi có thể kiểm chứng

Hệ thống bằng chứng tương tác cung cấp một công cụ phù hợp với xác minh trực tuyến tốn kém và tính toán ngoài chuỗi tiết kiệm. Hệ thống bằng chứng tương tác là một giao thức với hai người tham gia, người cần xác minh và người xác minh. Bằng cách gửi tin nhắn qua lại, người cần xác minh sẽ gửi thông tin để thuyết phục người xác minh rằng một yêu cầu nhất định là đúng, trong khi người xác minh sẽ kiểm tra những gì được cung cấp và từ chối nếu phát hiện yêu cầu giả mạo. Yêu cầu nào mà người xác minh không xác định là giả mạo sẽ được chấp thuận.

Lý do chính khiến việc người xác minh tự xác minh yêu cầu một cách kỹ lưỡng là hiệu quả- bằng cách tương tác với người cần xác minh, việc xác minh có thể trở nên vô cùng đắt đỏ nếu lựa chọn một phương pháp khác. Lý do của sự khác biệt này đến từ nhiều nguồn khác nhau: 1) Người xác minh có thể đang chạy phần cứng quá nhẹ, chỉ có thể hỗ trợ các tính toán giới hạn không gian hoặc giới hạn thời gian (hoặc cả hai), 2) xác minh giản đơn có thể yêu cầu quyền truy cập vào một chuỗi dài các lựa chọn không có điều kiện, 3) xác minh giản đơn có thể không thực hiện được bởi vì người xác minh không thực hiện không xử lý thông tin bí mật nhất định.

Trong khi sự bảo mật của thông tin là một trong những yếu tố ràng buộc trong thị trường crypto, thì một yếu tố còn quan trọng hơn trong bối cảnh mở rộng là chi phí xác minh trên chuỗi, đặc biệt quan trọng khi đặt trong tương quan giữa chi phí tính toán ngoài chuối giá rẻ.

Trong bối cảnh tiền điện tử, sự chú ý gần như đổ dồn về zk-SNARKs (đối số tri thức không tương tác cô đọng, đối số tri thức không minh bạch cô đọng). Tập hợp của hệ thống bằng chứng không tương tác này xoay quanh mạch số học cho phép mã hóa một phép tính tùy ý như một mạch bổ sung và được nhân bản trên một trường hữu hạn. Chẳng hạn, mạch số học có thể mã hóa "Tôi biết một chiếc lá trong cây Merkle “.

Bằng chứng zk-SNARK có kích thước không đổi (hàng trăm byte) và có thể kiểm chứng được trong khoảng thời gian không đổi, mặc dù các sự hiệu quả của một người xác minh dẫn tới phát sinh một số chi phí: yêu cầu về thiết lập tin cậy và chuỗi tham chiếu có cấu trúc, ngoài việc số học dựa trên ghép nối (trong đó độ cứng mật mã cụ thể vẫn là một yếu tố cần được quan tâm).

Hệ thống bằng chứng thay thế nảy sinh ra các một số đánh đổi. Ví dụ, Bulletproof không có thiết lập đáng tin cậy và dựa phần lớn vào giả định logarit rời rạc, tuy nhiên hiện nay đã có bằng chứng kích thước logarit (mặc dù vẫn khá nhỏ) và người xác minh thời gian tuyến tính. zk-STARK cung cấp một giải pháp thay thế cho zk-SNARK về khả năng mở rộng mà không có thiết lập đáng tin cậy và chỉ dựa vào các giả định về mật mã học, mặc dù bằng chứng được tạo ra có kích thước logarit (khá lớn: hàng trăm kilobyte).

Trong bối cảnh của một hệ sinh thái tiền điện tử nhiều lớp như Nervos Network, bằng chứng tương tác cung cấp khả năng giảm tải các tính toán của bên xác minh cho lớp 2 trong khi chỉ yêu cầu công việc một lượng nhỏ tính toán từ lớp 1. Chẳng hạn, trong giao thức ZK Rollup của Vitalik Buterin [8]: lớp không cần cấp phép (permissionless) tập hợp các giao dịch ngoài chuỗi và cập nhật định kỳ vào một gốc Merkle được lưu trữ trên chuỗi. Mỗi bản cập nhật gốc như vậy được kèm theo zk-SNARK cho thấy chỉ các giao dịch hợp lệ được tích lũy vào cây Merkle mới. Hợp đồng thông minh xác minh bằng chứng và cho phép cập nhật vào gốc Merkle nếu bằng chứng hợp lệ.

Cấu trúc được phác thảo ở trên có thể hỗ trợ các giao dịch phức tạp bao gồm cả sàn phi tập trung, đa token và tính toán bảo vệ quyền riêng tư.

### 5.6 Mô hình kinh tế của các giải pháp lớp 2

Giải pháp lớp 2 mang lại khả năng mở rộng vô cùng ấn tượng, tuy nhiên thiết kế mô hình kinh tế của token cho các hệ thống này có thể gặp một vài thách thức.

Mô hình kinh tế của token lớp 2 có thể liên quan đến việc bồi thường cho cơ sở hạ tầng quan trọng (như người xác nhận và các watchtower), cũng như thiết kế khuyến khích dành riêng cho các ứng dụng. Cơ sở hạ tầng lớp 2 có xu hướng hoạt động tốt hơn với mô hình đăng ký, mô hình dựa trên trên thời lượng. Trong Nervos Network, cấu trúc giá này có thể được thực hiện dễ dàng thông qua phương thức thanh toán dựa trên chi phí cơ hội của CKB. Các nhà cung cấp dịch vụ có thể thu lãi trên "tiền gửi bảo mật" của người dùng thông qua NervosDAO. Các nhà phát triển lớp 2 sau đó có thể tập trung các mô hình kinh tế token dựa theo các ưu đãi dành riêng cho các ứng dụng của họ.

Theo một cách nào đó, mô hình định giá này cũng gần giống như cách người dùng trả tiền lưu trữ trên CKB. Về cơ bản, họ đang trả phí đăng ký cho các người đào với việc phân phối phần thưởng lạm phát do NervosDAO phát hành.

## 6. Nervos Network

### 6.1 Lớp 1 dưới dạng lưu trữ nhiều tài sản giá trị của nền tảng

Chúng tôi tin rằng một blockchain lớp 1 phải được xây dựng như một kho lưu trữ giá trị. Để tối đa hóa tính phi tập trung dài hạn, lớp này phải hoạt động dựa trên đồng thuận POW với một mô hình kinh tế được thiết kế dựa vào sở hữu về bộ nhớ (state), thay vì phí giao dịch. Common Knowledge Base (CKB) là một bằng chứng về công việc, tài sản, kho lưu trữ giá trị blockchain với mô hình kinh tế và lập trình được thiết kế xung quanh state.

CKB là lớp cơ sở của Nervos Network, với độ bảo mật cao nhất và mức độ phi tập trung cao nhất. Việc sở hữu và giao dịch tài sản trên CKB đi kèm với chi phí cao, tuy nhiên Nervos Network cung cấp giải pháp lưu trữ tài sản an toàn và dễ truy cập nhất trong mạng và cho phép khả năng tổng hợp tối đa. CKB phù hợp nhất cho các tài sản có giá trị cao và lưu trữ tài sản dài hạn.

Common Knowledge Base là blockchain lớp 1 đầu tiên được xây dựng đặc biệt để hỗ trợ các giao thức lớp 2:

- CKB được thiết kế để bổ sung cho các giao thức lớp 2, tập trung vào bảo mật và phi tập trung, thay vì các chồng chéo với các điểm nổi trội của lớp 2 như khả năng mở rộng.

- CKB xây dựng mô hình sổ cái của nó xung quanh các state, thay vì dựa vào tài khoản. Cell về cơ bản là các state khép kín có thể được tham chiếu bởi các giao dịch và được chuyển qua lại giữa các lớp. Điều này khá phù hợp với kiến trúc lớp, trong đó các đối tượng được tham chiếu và chuyển qua giữa các lớp là các phần trạng thái, thay vì các tài khoản.

- CKB được thiết kế như một máy xác minh tổng quát, thay vì công cụ tính toán. Điều này cho phép CKB hoạt động như một tòa án mật mã, xác minh các giao dịch state ngoài chuỗi.

- CKB cho phép các nhà phát triển dễ dàng thêm các nguyên hàm mã hóa tùy chỉnh. Điều này chứng minh tương lai cho CKB, cho phép xác minh bằng chứng được tạo bởi nhiều giải pháp lớp 2.

Common Knowledge Base đặt mục tiêu trở thành cơ sở hạ tầng lưu trữ kiến thức có giá trị nhất thế giới, với hệ sinh thái lớp 2 tốt nhất cung cấp giải pháp xử lý giao dịch blockchain hiệu quả và có khả năng mở rộng nhất.

### 6.2 Mở rộng với giải pháp 2 lớp 

Với kiến trúc phân lớp, Nervos Network có thể mở rộng trên lớp 2 cho bất kỳ số lượng người tham gia nào, trong khi vẫn duy trì các thuộc tính quan trọng là tính phi tập trung và lưu trữ tài sản. Các giao thức lớp 2 có thể sử dụng bất kỳ loại cam kết lớp 1 hoặc mật mã cơ sở nào, cho phép sự linh hoạt và sáng tạo trong việc thiết kế các hệ thống giao dịch để hỗ trợ người dùng lớp 2. Các nhà phát triển lớp 2 có thể chọn đánh đổi giữa mô hình thông lượng, quyền riêng tư và tính tin cậy dựa theo bối cảnh ứng dụng và người dùng của họ.

Trong Nervos Network, lớp 1 (CKB) được sử dụng để xác minh “state”, trong khi lớp 2 chịu trách nhiệm tạo “state”. Các state channel và side-chain là các ví dụ về tạo “state”, tuy nhiên bất kỳ loại hình xác minh nào cũng đều ứng dụng được, chẳng hạn như nhóm tạo bằng chứng không kiến thức. Ví cũng hoạt động ở lớp 2, sử dụng logic tùy ý, tạo state mới và chuyển state tới CKB để xác thực. Ví của Nervos Network có sức mạnh to lớn do chúng tạo ra state và toàn quyền kiểm soát giao dịch của state.

Side-chain thân thiện với nhà phát triển và cung cấp trải nghiệm người dùng tốt. Tuy nhiên, giải pháp này dựa vào sự trung thực của người xác nhận. Nếu người xác nhận có những biểu hiện không trung thực, người dùng có nguy cơ mất tài sản của họ. Nervos Network cung cấp mã nguồn mở và chuỗi bên dễ sử dụng để khởi chạy chuỗi bên trên CKB, bao gồm khung blockchain Proof-of-Stake gọi là "Muta" và giải pháp chuỗi bên dựa trên nó được gọi là “Axon".

Muta là một khung blockchain hiệu suất cao, tùy biến cao được thiết kế để hỗ trợ Proof-of-Stake, đồng thuận BFT và hợp đồng thông minh. Nó có tính năng đồng thuận cao và độ trễ đồng thuận BFT thấp và hỗ trợ các máy ảo khác nhau bao gồm CKB-VM, EVM và WASM. Các máy ảo khác nhau có thể được sử dụng đồng thời trong một blockchain Muta, với khả năng tương tác chéo các máy ảo. Muta giảm đáng kể rào cản cho các nhà phát triển để xây dựng các chuỗi khối hiệu suất cao, trong khi vẫn cho phép linh hoạt tối đa để tùy chỉnh các giao thức của họ.

Axon là một giải pháp hoàn chỉnh được xây dựng với Muta để cung cấp cho các nhà phát triển chìa khóa trao tay chuỗi bên trên Nervos CKB, với mô hình token kinh tế và bảo mật. Các giải pháp Axon sử dụng CKB để lưu giữ tài sản an toàn và sử dụng cơ chế quản trị dựa trên token để quản lý người xác nhận chuỗi bên. Các giao thức chuỗi chéo cho các tương tác giữa chuỗi bên Axon và CKB, cũng như giữa các chuỗi bên Axon. Với Axon, các nhà phát triển có thể tập trung vào việc xây dựng các ứng dụng, thay vì xây dựng cơ sở hạ tầng và các giao thức chuỗi chéo.

Cả Muta và Axon hiện nay đang trong quá trình phát triển. Chúng tôi sẽ sớm công khai sơ bộ và chương trình request for comment (Đề nghị duyệt thảo và bình luận) cũng đang dần được chuẩn bị. 

Các giao thức lớp 2 là một lĩnh vực đang dần được quan tâm nghiên cứu và phát triển. Chúng tôi tập trung vào tương lai trong đó tất cả các giao thức lớp 2 sẽ được chuẩn hóa và tương tác liền mạch. Tuy nhiên, cũng phải thừa nhận rằng các giải pháp lớp 2 vẫn đang hoàn thiện và chúng tôi đang cố gắng vượt qua giới hạn mà lớp 2 có thể làm được, cũng như tìm ra sự đánh đổi phù hợp. Giải pháp lớp 2 sẽ sớm trở thành một giải pháp đầy hứa hẹn, tuy nhiên vẫn cần có các nghiên cứu thêm về khả năng tương tác, bảo mật và mô hình kinh tế trong thiết kế lớp 2. Sau khi ra mainnet, chúng tôi sẽ dành nhiều nỗ lực nghiên cứu cho các giao thức lớp 2.

### 6.3 Tính bền vững

Vì lợi ích của tính bền vững lâu dài Nervos, Nervos Common Knowledge Base giới hạn state, áp đặt chi phí cho việc lưu trữ trên chuỗi và cung cấp các ưu đãi cho người dùng để xóa bộ nhớ state của họ. Một state giới hạn lưu trữ các yêu cầu cho sự tham gia của full node ở mức thấp, đảm bảo các node có thể chạy trên phần cứng chi phí thấp. Sự tham gia đầy đủ của nút mạnh làm tăng tính phi tập trung và bảo mật.

Bằng cách áp đặt chi phí "chi phí thuê state" theo tỷ lệ theo thời gian cho việc lưu trữ state, Nervos Common Knowledge Base đã giảm thiểu vấn đề mà nhiều blockchain phải đối mặt trong mô hình "trả một lần, lưu trữ mãi mãi". Được thực hiện thông qua "lạm phát mục tiêu", cơ chế cho thuê state này cung cấp trải nghiệm người dùng tốt hơn trong khi áp đặt chi phí cho lưu trữ state.

Chi phí lạm phát này được lấy làm mục tiêu vì người dùng sở hữu không gian đồng thuận mà dữ liệu của họ đang chiếm. Mô hình này cũng bao gồm một cơ chế riêng để người dùng xóa trạng thái của họ khỏi không gian đồng thuận. Cùng với các ưu đãi kinh tế của cho thuê state, điều này đảm bảo rằng quy mô state sẽ luôn được chuyển sang lượng dữ liệu tối thiểu theo yêu cầu của người tham gia mạng.

Các cá nhân sở hữu state cũng làm giảm phần lớn chi phí của các nhà phát triển. Thay vì bị yêu cầu mua CKByte để đáp ứng yêu cầu state của tất cả người dùng, các nhà phát triển chỉ cần mua đủ số lượng CKByte để lưu trữ code xác minh.

Cuối cùng, cho thuê state cung cấp phần thưởng liên tục cho các nhà khai thác thông qua phát hành token mới. Thu nhập có thể dự đoán này khuyến khích người đào phát triển blockchain, thay vì fork các khối có lợi nhuận để thu phí giao dịch.

### 6.4 Hài hòa về lợi ích

Mô hình kinh tế của Common Knowledge Base phân bố phần thưởng tương xứng tới tới tất cả những người tham dự trong hệ sinh thái. Cụ thể hơn, giá trị token tăng lên hỗ trợ mục tiêu của tất cả các thành phần của mạng:

- Người đào: Giá token tăng đồng nghĩa với việc thu nhập từ việc đào tăng lên

- Người dùng: giá token tăng thu hút nhiều người đào tham dự vào mạng lưới tăng tính bảo mật cho tài sản

- Nhà phát triển: Giá token tăng làm tăng tính bảo mật cho mạng lưới mà không làm tăng thêm nhiều chi phí 

- Người nắm giữ token: Giá token tăng làm tăng giá trị tài sản họ nắm giữ 

Nervos Common Knowledge Base được xây dựng để lưu trữ tài sản một cách an toàn, thay vì chạy theo mục tiêu phí giao dịch rẻ. Việc định vị dự án, tương tự như cộng đồng người dùng Bitcoin, thay vì phương tiện trao đổi người dùng.

Các sàn giao dịch tầm trung có xu hướng hướng mạng blockchain theo hướng tập trung để theo đuổi mục tiêu về hiệu suất và chi phí. Trường hợp người duy trì mạng lới (người đào, người xác nhận giao dịch) không nhận được các khoản fee đủ để duy trì mạng lưới, bảo mật phải được tài trợ thông qua lạm phát tiền tệ, hoặc cũng có thể những người duy trì mạng lưới chỉ được trả dưới mức và họ đóng góp. Lạm phát tiền tệ gây bất lợi cho những người nắm giữ lâu dài và việc trả thưởng quá thấp cho người duy trì mạng lưới sẽ gây bất lợi cho bất kỳ ai là một phần của mạng lưới đó. 

Với nhóm người dùng mong muốn duy trì tài sản có giá trị, họ sẽ có nhu cầu cao đối với sự kiểm duyệt và bảo mật tài sản. Người đào sẽ là đối tượng đáp ứng được yêu cầu này là đồng nghĩa với việc đó, họ sẽ được trả phần thưởng tương xứng. Trong mạng lưới duy trì giá trị, các bên sẽ nhận được lợi ích tương xứng. 

Đối với các mạng khác, người nắm giữ token dài hạn có thể được coi là "nhà đầu cơ", tuy nhiên, đối với Nervos Network, những người nắm giữ token sẽ đóng góp trực tiếp vào tổng giá trị của mạng lưới. Những người dùng này tạo ra nhu cầu về token, điều này góp phần tăng ngân sách duy trì bảo mật của mạng.

Bằng cách tổng hòa lợi ích của các bên tham gia, cộng đồng mạng Nervos sẽ ngày càng phát triển và hệ thống kinh tế hài hòa sẽ chống lại nhu cầu hard-fork mạng lưới. 

### 6.5 Nắm bắt giá trị và sản sinh giá trị

Đối với bất kỳ blockchain nào, để duy trì tính bảo mật khi giá trị của tài sản được bảo mật bởi nền tảng tăng lên, hệ thống phải có cơ chế nắm bắt giá trị. Bằng cách giới hạn state, CKB làm cho không gian đồng thuận trở thành nguồn lực khan hiếm và có giá thị trường. Khi nhu cầu lưu trữ tài sản trên mạng tăng lên, giá trị của không gian lưu trữ trạng thái (và token của CKB) cũng sẽ tăng theo. CKB là nền tảng đa tài sản đầu tiên tích lũy trực tiếp giá trị vào mã thông báo gốc của nó.

Là một nền tảng bảo tồn giá trị, giá trị nội tại của CKB được xác định bởi mức độ bảo mật mà nó cung cấp cho các tài sản nó lưu trữ. Khi giá trị tài sản được bảo đảm tăng lên, mô hình kinh thế của CKB cho phép quỹ an toàn CKB tăng lên một cách tự động để thu hút nhiều người đào, góp phần làm cho mạng lưới trở nên an toàn hơn, cũng như tăng giá trị nội tại của nền tảng. Điều này không chỉ quan trọng trong việc tăng tính bền vững mà còn cung cấp một lộ trình tăng trưởng cho giá trị nội tại của nền tảng - khi nền tảng trở nên an toàn hơn, nó cũng trở nên hấp dẫn hơn đối với việc lưu trữ các tài sản có giá trị cao hơn, tạo ra nhiều nhu cầu hơn. Rõ ràng, điều này bị ràng buộc bởi tổng giá trị tổng thể cuối cùng sẽ chuyển sang không gian blockchain để lữu trữ, nhưng chúng tôi tin rằng CKB sẽ thu hút một phần đáng kể nhu cầu này.

Theo thời gian, chúng tôi hy vọng mật độ kinh tế của CKB sẽ tăng lên. CKBytes sẽ được sử dụng để lưu trữ tài sản có giá trị cao và tài sản có giá trị thấp sẽ chuyển sang các chuỗi khối được kết nối với CKB, chẳng hạn như chuỗi bên 2 lớp. Thay vì trực tiếp bảo mật tài sản, CKB có thể được sử dụng như một gốc tin cậy để bảo đảm toàn bộ hệ sinh thái của chuỗi bên ví dụ thông qua vài trăm byte bằng chứng mật mã. Mật độ kinh tế của các bằng chứng này rất cao, hỗ trợ thêm cho nhu cầu về không gian lưu trữ khi giá của CKBytes tăng đáng kể: tương tự như một thửa đất nhỏ làm tăng đáng kể mật độ kinh tế của nó bằng cách hỗ trợ một tòa nhà chọc trời.

Cuối cùng, thông qua thiết kế của NervosDAO và chức năng "nơi trú ẩn lạm phát", người sở hữu token sẽ luôn giữ một tỷ lệ cố định trong tổng số phát hành, biến token trở thành một kho lưu trữ giá trị tuyệt vời.

### 6.6 Thu hẹp khoảng cách với luật lệ 

Blockchains mở cho phép tính phi tập trung trong phát hành và giao dịch tài sản. Đây là điểm làm nên giá trị của chúng, nhưng cũng là lý do khiến các mạng lưới này không thể đối đầu lại với các hệ thống tài chính cũng như và hệ thống pháp luật hiện tại. 

Sự xuất hiện của một kiến trúc phân lớp mở ra cơ hội cho một phần công nghệ blockchain mở vốn không tuân theo một quy định nào có thể tuân thủ theo quy định pháp luật. Ví dụ: người dùng có thể lưu trữ tài sản phi tập trung của họ trên lớp 1, được hưởng quyền sở hữu tài sản tuyệt đối của các tài sản này và cũng có thể quản lý hoạt động kinh doanh trên lớp 2, nơi chúng phải chịu các ràng buộc pháp lý.

Lấy ví dụ sàn điện tử - các quốc gia như Nhật Bản và Singapore đã cấp giấy phép cho các sàn giao dịch và tạo ra các quy định pháp luật. Sàn hoặc một nhánh của một sàn giao dịch toàn cầu có thể xây dựng chuỗi giao dịch lớp 2, nhập danh tính người dùng và tài sản và sau đó tiến hành kinh doanh hợp pháp theo các yêu cầu quy định pháp luật của từng vùng/nước.

Phát hành và giao dịch các tài sản trên thế giới sẽ trở thành hiện thực nếu ứng dụng cấu trúc blockchain theo lớp. Các tài sản trong thế giới thực có thể chảy vào hệ sinh thái blockchain thông qua chuỗi bên lớp 2 sang blockchain mở lớp 1, cho phép các tài sản này truy cập vào hệ sinh thái lớn nhất của các dịch vụ tài chính phi tập trung và tối đa hóa giá trị.

Trong tương lai, Nervos Network sẽ sử dụng chuỗn bên lớp 2 và các ứng dụng như nền tảng ứng dụng người dùng quy mô lớn, hợp tác với với các công ty hàng đầu trong lĩnh vực.

## Tài liệu tham khảo

[1] Satoshi Nakamoto. "Bitcoin: A Peer-to-Peer Electronic Cash System". 31 Oct 2008, https://bitcoin.org/bitcoin.pdf

[2] Vitalik Buterin. "Ethereum White Paper: A Next Generation Smart Contract & Decentralized Application Platform". Nov 2013 http://blockchainlab.com/pdf/Ethereum_white_paper-a_next_generation_smart_contract_and_decentralized_application_platform-vitalik-buterin.pdf

[3] Bitcoin có kích thước trung bình 250 byte cho mỗi giao dịch:

Kích thước cho mỗi khối (cứ sau 10 phút): (2 * 250 * 7.500.000) / (24 * 6) = 26.041.666.666 byte;

Kích thước của blockchain tăng lên hàng ngày: 26.041.666.666 * (24 * 6) = 3.750.000.000.000 byte;

Kích thước của blockchain tăng lên mỗi năm: 3.750.000.000.000 * 365.25 = 1.369.687.500.000.000 byte

[4] Gur Huberman, Jacob Leshno, Ciamac C. Moallemi. "Monopoly Without a Monopolist: An Economic Analysis of the Bitcoin Payment System". Bank of Finland Research Discussion Paper No. 27/2017. 6 Sep 2017, https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3032375

[5] Miles Carlsten, Harry Kalodner, S. Matthew Weinberg, Arvind Narayanan. "On the Instabiliity of Bitcoin Without the Block Reward". Oct 2016, https://www.cs.princeton.edu/~smattw/CKWN-CCS16.pdf

[6] Lewis Gudgeon, Perdo Moreno-Sanchez, Stefanie Roos, Patrick McCorry, Arthur Gervais. "SoK: Off The Chain Transactions". 17 Apr 2019, https://eprint.iacr.org/2019/360.pdf

[7] Joseph Poon, Vitalik Buterin. "Plasma: Scalable Autonomous Smart Contracts". 11 Aug 2017, https://plasma.io/plasma.pdf

[8] Vitalik Buterin. "On-chain scaling to potentially ~500 tx/sec through mass tx validation". 22 Sep 2018, https://ethresear.ch/t/on-chain-scaling-to-potentially-500-tx-sec-through-mass-tx-validation/3477


