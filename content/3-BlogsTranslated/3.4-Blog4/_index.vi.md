---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# [SECURITY/Web3] Building secure, verifiable blockchain key management on AWS Nitro Enclaves at Turnkey

Chào anh em! Các vụ lộ private key liên tục diễn ra khiến bài toán "Quản lý khóa (Key Management)" trở thành nỗi đau đầu lớn cho dev Web3/DeFi. Mình vừa đọc bài blog kỹ thuật từ AWS Web3 Blog về cách Turnkey giải quyết triệt để vấn đề này nhờ kết hợp mật mã học và hạ tầng phần cứng AWS Nitro Enclaves. Dưới đây là các điểm cốt lõi: 

1. **Thách thức trong kiến trúc quản lý khóa hiện tại**
   Quá trình ký giao dịch (transaction signing) thông thường đòi hỏi sự cân nhắc giữa bảo mật và hiệu suất vận hành:
   * Tự xây dựng hạ tầng: Yêu cầu chi phí lớn và đối mặt với rủi ro cao về mặt tuân thủ (compliance).
   * Ủy thác cho bên thứ ba (Custodians): Làm giảm quyền kiểm soát trực tiếp và thiếu tính minh bạch về mặt vận hành.
   * Hạ tầng phần mềm thông thường: Khóa thô (raw keys) có nguy cơ bị khai thác thông qua kết xuất bộ nhớ (memory dumps) hoặc tệp tin nhật ký (log files) khi hệ sinh thái bị xâm nhập.

2. **Cơ chế cô lập của AWS Nitro Enclaves**
   Turnkey triển khai mô hình mã hóa tích hợp enclave (Enclave-Native Key Management), chuyển toàn bộ các tác vụ nhạy cảm bao gồm khởi tạo khóa, ký số và thực thi chính sách vào môi trường AWS Nitro Enclaves:
   * Cô lập phần cứng: Môi trường enclave không có lưu trữ vĩnh viễn, không hỗ trợ truy cập tương tác (no SSH) và không kết nối Internet.
   * Giao tiếp an toàn: Dữ liệu được truyền qua kênh ảo nội bộ VSOCK. Khóa cấu hình chỉ được giải mã trong RAM tại thời điểm ký giao dịch và được xóa ngay lập tức. Quản trị viên hệ thống của Turnkey hoặc AWS đều không thể tiếp cận khóa thô.

3. **Quy trình khởi tạo và lưu trữ dữ liệu mật mã**
   Hệ thống sử dụng mô hình ví cây phân cấp (Hierarchical Deterministic Wallet - HD Wallet) để quản lý cấu trúc khóa phái sinh:
   * Khởi tạo dữ liệu gốc (Seed): Sử dụng bộ sinh số ngẫu nhiên an toàn được cung cấp bởi phần cứng Nitro Security Module (NSM).
   * Mã hóa đối xứng: Chuỗi seed gốc được mã hóa thông qua khóa Quorum Key trước khi lưu trữ vào cơ sở dữ liệu.
   * Ký giao dịch: Bản mã được nạp vào enclave, giải mã tạm thời trong RAM để thực hiện lệnh ký mật mã, sau đó xóa bỏ. Khóa thô không bao giờ được ghi xuống đĩa lưu trữ (disk).

4. **Kiến trúc phân tách trạng thái và luồng dữ liệu hệ thống**
   Để dễ hiểu, kiến trúc của Turnkey được chia làm hai phần tách biệt: Phân vùng quản lý bên ngoài (không an toàn tuyệt đối) và Phân vùng tính toán bên trong (an toàn tuyệt đối).
   * Bên ngoài (Hạ tầng AWS Cloud): Khi người dùng gửi yêu cầu qua API Gateway, máy chủ EC2 (Coordinator) sẽ tiếp nhận và xử lý. Các dữ liệu trạng thái và bản khóa gốc đã mã hóa được lưu ở Aurora Database. Các tác vụ nền khác như hàng đợi bất đồng bộ (Async Queue), Redis, Updater, Heartbeat và Notifier chỉ làm nhiệm vụ đồng bộ hệ thống và gửi thông báo (Webhook Targets). Phân vùng này hoàn toàn không biết khóa thô là gì.
   * Bên trong (AWS Nitro Enclave): Máy chủ EC2 chuyển các lệnh nhạy cảm xuống Enclave qua kênh nội bộ gRPC/VSOCK. Trong môi trường cô lập này, luồng xử lý diễn ra khép kín qua 5 bước:
     * TLS Fetcher: Tạo kết nối mạng bảo mật từ bên trong.
     * Parser: Bóc tách dữ liệu giao dịch.
     * Policy Engine: Kiểm tra giao dịch có vi phạm các quy tắc (hạn mức, danh sách chặn...) của người dùng hay không.
     * Notarizer: Ký chứng nhận giao dịch hợp lệ sau khi vượt qua bộ máy chính sách.
     * Signer: Lấy bản mã hóa khóa từ database, giải mã tạm thời trong RAM để ký số giao dịch rồi lập tức xóa sạch dấu vết.

5. **Cơ chế xác thực từ xa bằng toán học**
   Turnkey chuyển đổi mô hình từ tin tưởng tuyệt đối (trust-based) sang mô hình có thể kiểm chứng (verifiable) dựa trên:
   * Remote Attestation: AWS cung cấp tài liệu chứng thực mật mã được ký bởi phần cứng nhằm xác minh mã thực thi trong enclave không bị thay đổi hoặc cài cắm mã độc.
   * Reproducible Builds: Hệ thống vận hành trên QuorumOS - một hệ điều hành tối giản. Các bên độc lập có thể tự biên dịch lại mã nguồn từ đầu để đối chiếu tính toàn vẹn của hệ thống.

**Ứng dụng thực tế**
* Embedded Wallets: Tích hợp ví không lưu ký (non-custodial) vào ứng dụng phi tập trung với tiêu chuẩn an toàn cấp doanh nghiệp.
* AI Agent Transactions: Hỗ trợ các tác nhân AI thực thi giao dịch tự động trên chuỗi (on-chain) theo các chính sách thiết lập sẵn mà không làm lộ khóa cấu hình.

**Tổng kết**
Giải pháp của Turnkey tận dụng AWS Nitro Enclaves để thiết lập một quy trình xử lý khóa khép kín trong RAM và tự động giải phóng bộ nhớ sau khi sử dụng. Sự tách biệt hoàn toàn giữa lưu trữ trạng thái (State) và môi trường thực thi phần cứng cô lập (Execution) giúp bảo vệ tài sản số ngay cả khi hạ tầng máy chủ máy ảo bị xâm nhập. Đồng thời, nhờ cơ chế chứng thực từ xa và khả năng tái lập mã nguồn, hệ thống cho phép người dùng kiểm chứng tính toàn vẹn và minh bạch của toàn bộ quy trình mật mã.
![Turnkey Infrastructure Diagram](/AWS/images/3-Blogs/Blog-1/blog1.png)

## Kết luận
Tổng kết Giải pháp của Turnkey tận dụng AWS Nitro Enclaves để thiết lập một quy trình xử lý khóa khép kín trong RAM và tự động giải phóng bộ nhớ sau khi sử dụng. Sự tách biệt hoàn toàn giữa lưu trữ trạng thái (State) và môi trường thực thi phần cứng cô lập (Execution) giúp bảo vệ tài sản số ngay cả khi hạ tầng máy chủ máy ảo bị xâm nhập. Đồng thời, nhờ cơ chế chứng thực từ xa và khả năng tái lập mã nguồn, hệ thống cho phép người dùng kiểm chứng tính toàn vẹn và minh bạch của toàn bộ quy trình mật mã.

Link bài viết: [Building secure, verifiable blockchain key management on AWS Nitro Enclaves at Turnkey | AWS Web3 Blog ](https://aws.amazon.com/vi/blogs/web3/building-secure-verifiable-blockchain-key-management-on-aws-nitro-enclaves-at-turnkey/)