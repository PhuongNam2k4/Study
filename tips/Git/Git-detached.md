## Mở rộng: biết về HEAD và Detached HEAD trong GIT để tránh bị mất code

Rất nhiều trường hợp các bạn không biết code của mình mới commit đã đi đâu? các bạn nên biết về branch HEAD trong GIT để có thể tìm lại được những commit của mình, tránh việc mất code phải code lại.

Đây là kinh nghiệm xương máu của mình về cách dùng Git, hồi đó còn non và xanh không biết gì nên cũng hay dính Detached HEAD mà mỗi lần dính thì ôi dồi, tìm lại code mệt thí mịa luôn. Nên mình viết lại bài này để giới thiệu với các bạn.

__Khái niệm con trỏ HEAD__

_HEAD là con trỏ dùng để xác định vị trí của commit hiện tại. Kể cả khi chúng ta xóa bớt commit đi hoặc dùng lệnh `git reset` để quay lại commit trước đó thì HEAD vẫn luôn là commit cuối cùng_

![](/pictures/reset.png)
\
Mỗi một branch có một con trỏ HEAD khác nhau vì số lượng commit của mỗi branch là khác nhau, nhưng vẫn theo nguyên lý trên, HEAD luôn là commit cuối cùng

Git ghi chú về HEAD trong một tệp nằm bên trong kho lưu trữ Git, trong `.git / HEAD` (Đây là tệp nội bộ, vì vậy không nên thao tác thủ công!)

Chúng ta có thể kiểm tra head và lịch sử commmit thông qua lệnh: `git log --all --oneline`. 

__Detached HEAD__

Trong một số trường hợp hiếm hoi, tệp HEAD KHÔNG chứa tham chiếu branch mà là giá trị SHA-1 của một version cụ thể. Điều này xảy ra khi bạn kiểm tra một commit, tag hoặc chia branch xa cụ thể. Kho lưu trữ của bạn sau đó ở trạng thái được gọi là detached HEAD.

Rất có thể bạn sẽ không bao giờ gặp phải trạng thái "bí ẩn" này trong sự nghiệp Git của mình. Tuy nhiên, nếu bạn làm vậy vào một ngày nào đó, bạn có thể muốn biết "Detached HEAD"- và phải làm gì khi ở trạng thái đó.

__Hiểu về cách thức hoạt động của "checkout"__

Với lệnh "git checkout", bạn xác định version nào của dự án mà bạn muốn thực hiện. Sau đó, Git đặt tất cả các tệp của version đó vào thư mục bản sao đang làm việc của bạn.

Thông thường, bạn sử dụng tên chi branch để "checkout":

    $ git checkout development

Tuy nhiên, bạn cũng có thể cung cấp hàm băm SHA1 của một commit cụ thể để thay thế:


    $ git checkout 56a4e5c08
    Note: checking out '56a4e5c08'.


    You are in 'detached HEAD' state...

Trạng thái chính xác này - khi một commit cụ thể được kiểm tra thay vì một branch - được gọi là "Detached HEAD".

![](/pictures/head-detatched.png)

Một số vấn đề khi ở trạng thái Detached HEAD
Con trỏ HEAD trong Git xác định version làm việc hiện tại của bạn (và do đó các tệp được đặt trong thư mục làm việc của dự án của bạn). Thông thường, khi kiểm tra tên branch thích hợp, Git sẽ tự động di chuyển con trỏ HEAD khi bạn tạo một commit mới. Bạn tự động có commit mới nhất của branch đã chọn.

Thay vào đó, khi bạn chọn kiểm tra một commit hash, Git sẽ không làm điều này cho bạn. Hậu quả là khi bạn thực hiện các thay đổi và commit chúng, những thay đổi này KHÔNG thuộc về bất kỳ nhánh nào.

Điều này có nghĩa là chúng có thể dễ dàng bị lạc khi bạn kiểm tra một bản sửa đổi hoặc branch khác: không được ghi lại trong ngữ cảnh của một branch, bạn không có khả năng truy cập trạng thái đó một cách dễ dàng (trừ khi bạn có một trí nhớ tuyệt vời và có thể nhớ commit hash của commit mới đó ...).

Khi nào thì trạng thái Detached HEAD xuất hiện
Có một số tình huống mà trạng thái Detached HEAD là phổ biến:

Submodules: thực sự được kiểm tra tại các commit cụ thể thay vì một branch.
Rebase hoạt động bằng cách tạo trạng thái HEAD tạm thời tách rời trong khi nó chạy.
Làm thế nào để tránh trạng thái Detached HEAD
Ngoài ra, bạn có thể nghĩ đến một tình huống khác: quay ngược thời gian để thử version cũ hơn của dự án của bạn thì sao? Ví dụ: trong bối cảnh có lỗi, bạn muốn xem mọi thứ hoạt động như thế nào trong bản sửa đổi cũ hơn.

Đây là một trường hợp sử dụng hoàn toàn hợp lệ và phổ biến. Tuy nhiên, bạn không cần phải chuyển mình sang trạng thái Detached HEAD để đối phó với nó. Thay vào đó, hãy nhớ rằng toàn bộ khái niệm rẽ nhánh trong Git đơn giản và rẻ như thế nào: bạn có thể chỉ cần tạo một nhánh (tạm thời) và xóa nó sau khi hoàn tất.

```
$ git checkout -b test-branch 56a4e5c08

...làm bất cứ thứ gì bạn muốn...

$ git checkout master

$ git branch -d test-branch
```

Nguồn: [viblo](https://viblo.asia/p/biet-ve-head-va-detached-head-trong-git-de-tranh-bi-mat-code-m68Z04vMZkG)