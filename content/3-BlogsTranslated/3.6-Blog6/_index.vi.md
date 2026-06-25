---
title: "Blog 3"
date: "2026-06-24"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# How AWS DevOps Agent uses multi-agent reasoning to find root causes

Chắc hẳn ai cũng từng trải qua cảnh này: Nửa đêm hệ thống báo lỗi, API trả về 500, bạn lao vào check log các container và thấy ngay một cái Exception quen quen. Dựa vào kinh nghiệm, bạn lập tức chốt luôn nguyên nhân, fix vội rồi deploy lại. Nhưng rốt cuộc, server vẫn sập.

Hiện tượng này gọi là **"Confirmation Bias"** (Thiên kiến xác nhận). Khi gặp sự cố, chúng ta thường có xu hướng bám lấy giả thuyết đầu tiên xuất hiện trong đầu, tìm được một bằng chứng ủng hộ là dừng lại ngay, khiến nguyên nhân gốc rễ thực sự bị bỏ sót. 

Để giải quyết bài toán này, AWS đã giới thiệu **AWS DevOps Agent** – một AI áp dụng kiến trúc **"multi-agent reasoning"** (suy luận đa đặc vụ). Nó không mò mẫm log một cách mù quáng, mà hoạt động bài bản và tư duy logic như một kỹ sư SRE thực thụ. 

---

### TÓM TẮT CỐT LÕI (TL;DR)
* **Vấn đề:** Các hệ thống phân tán hiện đại không thiếu dữ liệu giám sát (telemetry) mà thiếu **khả năng tư duy logic (reasoning)** để chủ động phản bác và loại trừ các giả thuyết sai khi điều tra sự cố.
* **Kiến trúc:** Hoạt động trong một không gian logic cô lập (**Agent Space**) và dựa trên nền tảng **Bản đồ kiến trúc động (Topology Graph)** để hiểu rõ bối cảnh toàn hệ thống trước khi rà soát log.
* **Quy trình 4 bước:** 
  1. *Triage:* Gom nhóm các cảnh báo trùng lặp để giảm nhiễu ở tốc độ cao.
  2. *Investigation:* Đưa ra nhiều giả thuyết song song, dùng phản chứng (Counter-evidence) để tìm ra Root Cause thực sự.
  3. *Mitigation:* Đề xuất kế hoạch sửa lỗi chi tiết kèm kịch bản Rollback nhưng **không tự ý can thiệp (không có quyền ghi)** vào hệ thống.
  4. *Prevention:* Gom cụm các lỗi trong quá khứ để tìm quy luật chung và đưa ra các khuyến nghị cải tiến hạ tầng dài hạn.

---

### 1. Bí quyết cốt lõi: Nắm rõ bản đồ hệ thống (Topology Graph)
Trước khi bắt tay vào fix lỗi, AI không lao vào đọc log ngay. Việc điều tra hiệu quả bắt buộc phải xuất phát từ việc hiểu rõ bối cảnh kiến trúc của toàn bộ hệ thống. 

AWS DevOps Agent tự động vẽ ra một bản đồ động (**Topology Graph**). Bản đồ này được tổng hợp và liên tục cập nhật dựa trên: 
* Phân tích hạ tầng từ **AWS CloudFormation / AWS CDK**.
* Khám phá tài nguyên dựa trên tag qua **AWS Resource Explorer**.
* Luồng giao tiếp thực tế của hệ thống khi đang chạy (runtime) từ **CloudWatch Application Signals**, Dynatrace, Datadog...
* Sự liên kết chặt chẽ với các pipeline CI/CD (như GitLab CI/CD, GitHub Actions) để biết chính xác đoạn code nào vừa được thay đổi.

Nếu không có nền tảng này, AI (và cả con người) sẽ chỉ ngụp lặn giữa một biển dữ liệu giám sát. Mọi vận hành của bản đồ này đều được cô lập an toàn trong một phân vùng gọi là **Agent Space** theo từng team hoặc dịch vụ cụ thể.

---

### 2. Vòng đời 4 bước xử lý sự cố của AI Đa đặc vụ
Thay vì làm tất cả trong một bước, AWS DevOps Agent chia quy trình xử lý thành 4 giai đoạn chuyên biệt, hoạt động như một bánh đà vận hành khép kín: 

#### Bước 1: Phân loại (Triage) - Ưu tiên tốc độ
Khi hệ sinh thái có biến, hàng tá cảnh báo từ CloudWatch Alarms, Grafana, PagerDuty hay ServiceNow có thể đổ về cùng lúc. 
* Agent lập tức phân tích và tự động gom nhóm (correlate) các tín hiệu báo lỗi liên quan phát sinh từ cùng một sự kiện lại với nhau thành một sự cố duy nhất. 
* Việc này giúp giảm thiểu tối đa độ nhiễu, giúp anh em dev không bị "ngợp" và tập trung vào vấn đề cốt lõi. 
* Tất nhiên, con người vẫn có toàn quyền kiểm soát: nếu thấy AI gom nhóm sai, bạn hoàn toàn có thể tách chúng ra để điều tra độc lập. 

#### Bước 2: Điều tra (Investigation) - Nghệ thuật tự phản biện
Đây là lúc Agent thể hiện sức mạnh suy luận khác biệt hoàn toàn so với các AI truyền thống. Thay vì chỉ đi theo một hướng cảm tính, Agent tạo ra **nhiều giả thuyết cạnh tranh cùng một lúc**. 

Nó sẽ đào bới dữ liệu để không chỉ tìm bằng chứng ủng hộ, mà còn chủ động tìm bằng chứng phản bác (**Counter-evidence**) các giả thuyết đó:
* *Ví dụ:* Hệ thống checkout của một trang e-commerce bị tăng latency đột biến. Agent đưa ra 3 giả thuyết: do đợt cập nhật code cách đó 20 phút, do cổng thanh toán bên thứ ba phản hồi chậm, hoặc do database bị nghẽn connection pool. 
* Nó kiểm tra đợt cập nhật code và thấy thay đổi đó chỉ là sửa định dạng log (logging verbosity) -> loại bỏ giả thuyết cập nhật code. Nó thấy cổng thanh toán chậm, nhưng sự chậm trễ này xảy ra *sau* khi latency của hệ thống đã tăng -> đây là triệu chứng chứ không phải nguyên nhân. Cuối cùng, nó đối chiếu và phát hiện connection pool đang đạt 94% ngay tại thời điểm xảy ra sự cố và không có dữ liệu nào phản bác -> Chốt Root Cause.

AI sẽ ghi lại toàn bộ nhật ký tư duy này vào một tệp kiểm toán dòng thời gian gọi là **Investigation Journal**.

#### 🛡️ Bước 3: Giảm thiểu (Mitigation) - An toàn là trên hết
Xác định được lỗi rồi, sửa thế nào cho an toàn? Agent sẽ tự động sinh ra một kế hoạch khắc phục cực kỳ chi tiết bao gồm: các bước thực hiện, tiêu chí xác nhận thành công, và quan trọng nhất là kịch bản khôi phục (**rollback**) để đảo ngược tình thế nếu có biến cố mới phát sinh. 

* **Điểm đáng tiền:** Agent **KHÔNG tự ý can thiệp vào hệ thống (restricted write capabilities)**. Quyền hạn của Agent chỉ giới hạn ở việc tạo ticket hoặc case support. Nó chỉ đóng vai trò cố vấn đưa ra đề xuất mã nguồn hoặc câu lệnh sửa lỗi, quyền quyết định nhấn nút thực thi cuối cùng vẫn nằm hoàn toàn trong tay kỹ sư vận hành.

#### Bước 4: Phòng ngừa (Prevention) - Biến thụ động thành chủ động
Hệ thống AI không chỉ giải quyết sự cố bề nổi mà còn nhóm các lỗi trong quá khứ lại để tìm ra quy luật chung dựa trên phân tích mẫu (pattern analysis). 

Nhờ phân tích chéo, nó có thể phát hiện ra rằng hàng loạt các lỗi timeout, lỗi hàng đợi hay API phản hồi chậm thực chất đều có chung một nguyên nhân gốc rễ sâu xa từ việc cấu hình sai database. Từ đó, Agent đề xuất các giải pháp mang tính kiến trúc lâu dài: 
* Tinh chỉnh lại hệ thống giám sát và rào chắn cảnh báo.
* Bổ sung các mẫu code có tính phục hồi cao (retry logic, circuit breakers).
* Thêm các rào chắn kiểm tra (test validation gates) vào luồng CI/CD để ngăn lỗi này vĩnh viễn không lặp lại.

![The Incident Lifecycle](/images/image4.png)

---

### Tổng kết
AWS DevOps Agent đang thay đổi cách chúng ta vận hành hệ thống. Bằng cách ủy thác việc rà soát log, vẽ bản đồ kiến trúc và đối chiếu bằng chứng phản chứng cho AI, các kỹ sư Backend và DevOps có thể thoát khỏi những đêm thức trắng dò lỗi thủ công. 

Mọi ngữ cảnh vận hành vốn trước đây chỉ nằm trong đầu của các kỹ sư kỳ cựu nay đã được hệ thống hóa, lưu vết vĩnh viễn xuyên suốt các đợt thay đổi nhân sự. Bạn sẽ bước vào quá trình fix bug với một tâm thế tự tin hơn, bởi mọi giả thuyết đều đã được kiểm chứng bằng data thực tế, kèm theo một lối thoát hiểm an toàn.

***

**Link bài viết gốc:** [How AWS DevOps Agent uses multi-agent reasoning to find root causes | AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/how-aws-devops-agent-uses-multi-agent-reasoning-to-find-root-causes/)