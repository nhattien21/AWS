---
title: "Blog 2"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# TỔNG HỢP: CÁC PHƯƠNG PHÁP BẢO MẬT CHUỖI CUNG ỨNG PHẦN MỀM THEO CHUẨN AWS WELL-ARCHITECTED

Bài viết giải quyết bài toán rủi ro ngày càng tăng từ các cuộc tấn công vào chuỗi cung ứng phần mềm (như mã độc chèn vào các thư viện mã nguồn mở). Để đối phó, AWS đề xuất áp dụng các nguyên tắc từ Trụ cột Bảo mật (Security Pillar) của AWS Well-Architected Framework.

Dưới đây là 5 nhóm phương pháp trọng tâm:

### 1. Quản lý Danh tính và Truy cập (Identity & Access Management)
Kẻ tấn công thường nhắm vào môi trường phát triển (máy dev) hoặc CI/CD để đánh cắp thông tin xác thực.
* **Loại bỏ thông tin xác thực dài hạn (Long-term credentials):** Tuyệt đối không lưu trữ cứng (hardcode) các key như AWS IAM Access Keys trên máy tính cá nhân hoặc trong mã nguồn.
* **Sử dụng thông tin xác thực ngắn hạn (Short-lived credentials):** Môi trường CI/CD (như GitHub Actions, GitLab) nên sử dụng OIDC (OpenID Connect) để nhận thông tin xác thực IAM dùng một lần (temporary credentials).
* **Áp dụng đặc quyền tối thiểu (Least Privilege):** Các luồng pipeline CI/CD chỉ được cấp quyền vừa đủ để thực hiện tác vụ của nó (ví dụ: chỉ được phép push image lên một ECR repository cụ thể, không có quyền xóa).

### 2. Bảo vệ Tính toàn vẹn của Dữ liệu (Data Protection & Integrity)
Bạn cần đảm bảo mã nguồn và các file thực thi (artifacts) không bị kẻ gian chỉnh sửa trên đường vận chuyển.
* **Quản lý Dependency tập trung:** Sử dụng AWS CodeArtifact làm proxy để lưu trữ và quản lý các package nội bộ/bên ngoài. Điều này giúp ngăn chặn các cuộc tấn công typosquatting (đặt tên thư viện độc hại gần giống thư viện thật) bằng cách chỉ cho phép tải về từ các nguồn đã được phê duyệt.
* **Ký xác thực Artifact (Artifact Signing):** Sử dụng AWS Signer để tạo chữ ký số cho các container image hoặc đoạn code. Hệ thống ở môi trường Production (ví dụ: Amazon EKS) phải được cấu hình để kiểm tra tính hợp lệ của chữ ký trước khi cho phép triển khai (deploy).

### 3. Quản lý Lỗ hổng (Vulnerability Management)
Các công cụ quét tĩnh (CVE) truyền thống không đủ khả năng phát hiện các mã độc Zero-day chưa từng được biết đến.
* **Quét tự động và liên tục trong CI/CD:** Tích hợp Amazon Inspector vào quy trình build. Công cụ này phân tích hành vi để phát hiện các mã độc tiềm ẩn (sleeper packages) trước cả khi chúng bị công bố rộng rãi.
* **Xây dựng SBOM (Software Bill of Materials):** Hãy luôn duy trì bản kiểm kê phần mềm (SBOM). Nó giúp bạn biết chính xác ứng dụng đang sử dụng những thư viện nào, từ đó phản ứng cực kỳ nhanh chóng khi một lỗ hổng mới (như Log4j) được công bố.

### 4. Bảo vệ Cơ sở hạ tầng (Infrastructure Protection)
* **Cô lập môi trường CI/CD:** Hệ thống build/deploy phải được chạy trong một môi trường biệt lập (isolated), giới hạn quyền truy cập ra internet bên ngoài để ngăn chặn việc mã độc tự động tải thêm payload độc hại (call-home) trong quá trình build.
* **Áp dụng phòng thủ nhiều lớp (Defense in Depth):** Yêu cầu MFA (Xác thực đa yếu tố) và quy trình phê duyệt nhiều người (multi-approval) cho các thay đổi quan trọng trên môi trường Production.

### 5. Phát hiện và Phản hồi sự cố (Detection & Incident Response)
Giả định rằng hệ thống có thể bị xâm nhập, bạn cần có khả năng phát hiện càng sớm càng tốt.
* **Giám sát API chuyên sâu:** Luôn bật AWS CloudTrail để ghi log mọi hành động. Thiết lập cảnh báo cho các hành vi bất thường, ví dụ: lệnh push image (`ecr:PutImage`) xuất phát từ một địa chỉ IP lạ thay vì từ máy chủ CI/CD hợp lệ.
* **Sử dụng AI/ML để phát hiện mối đe dọa:** Kích hoạt Amazon GuardDuty để theo dõi, phát hiện các luồng mạng bất thường hoặc các hành vi đào coin (cryptojacking) có thể xảy ra nếu chuỗi cung ứng bị tấn công.

> **Tóm tắt cốt lõi:** Bảo mật chuỗi cung ứng không chỉ là quét mã nguồn, mà là một chiến lược phòng thủ toàn diện (Defense in Depth): Từ việc cấp quyền tối thiểu cho CI/CD, quản lý nguồn gốc thư viện, ký xác thực sản phẩm đầu ra, cho đến việc giám sát liên tục mọi hành vi trên môi trường AWS.

---

# [Security] Bảo mật chuỗi cung ứng phần mềm theo chuẩn AWS Well-Architected 🛡️☁️

Xin chào mọi người trong AWS Study Group VN! 👋

Thời gian gần đây, các cuộc tấn công vào chuỗi cung ứng phần mềm (software supply chain attacks) thông qua npm Registry như vụ Shai-Hulud, tea.xyz hay axios đang ngày càng phổ biến. Các kẻ tấn công thường nhắm vào 2 điểm yếu: đánh cắp tài khoản của người maintainer để chèn mã độc, và môi trường CI/CD của người dùng vô tình thực thi các package này.

![Mô phỏng luồng tấn công chuỗi cung ứng phần mềm](/images/image1.png)
*Chú thích: Hình 1 - Mô phỏng luồng tấn công chuỗi cung ứng phần mềm: Kẻ tấn công chiếm quyền hệ thống của maintainer -> publish phiên bản npm chứa mã độc -> người dùng (nhà phát triển/hệ thống CI/CD) cài đặt -> thông tin xác thực (credentials) bị thu thập và gửi về cho kẻ tấn công.*

Vậy làm sao để bảo vệ hệ thống trước những rủi ro này? Dựa trên framework AWS Well-Architected (Security Pillar), hôm nay nhóm mình xin tóm tắt 5 phương pháp hay nhất (Best Practices) từ AWS Security Blog để giúp anh em tăng cường phòng thủ:

### 1. Loại bỏ Long-term Credentials và Áp dụng Least Privilege
Mã độc thường quét môi trường CI/CD và máy dev để tìm kiếm secret (như npm token, AWS IAM access keys).
* **Với Developer:** Hãy dùng lệnh `aws login` để lấy thông tin xác thực ngắn hạn (short-lived credentials) thay vì lưu key cứng trên máy.
* **Với CI/CD:** Sử dụng OIDC (OpenID Connect) với GitHub Actions/GitLab CI để cấp credential tạm thời cho mỗi luồng job. Nếu bắt buộc dùng third-party không hỗ trợ, hãy lưu vào AWS Secrets Manager và tự động hóa việc rotate (xoay vòng) key.

### 2. Phòng thủ nhiều lớp (Defense in Depth) & Ký xác thực (Artifact Signing)
Một tài khoản bị lộ không nên là "dấu chấm hết".
* Bắt buộc bật MFA và yêu cầu nhiều người phê duyệt (multi-approval) cho các lần deploy lên môi trường Production.
* Sử dụng AWS Signer để tạo chữ ký xác thực mật mã học cho các artifact. Tính năng Amazon ECR managed signing có thể tự động ký các container image khi được push lên ECR. Sau đó, các admission controller trên EKS (như Kyverno) sẽ kiểm tra tính hợp lệ của chữ ký trước khi cho phép deploy.

![Luồng CI/CD bảo mật với AWS CodePipeline và AWS Signer](/images/image2.png)
*Chú thích: Hình 2 - Luồng CI/CD bảo mật với AWS CodePipeline và AWS Signer: Nhà phát triển push code -> ECR tự động kích hoạt AWS Signer khi nhận image mới -> Chữ ký (Signature) được lưu trữ cùng image -> Môi trường vận hành (EKS/ECS) kéo và xác thực chữ ký trước khi triển khai.*

### 3. Quản lý Dependency tập trung
* Thay vì để developer tự pull trực tiếp từ bên ngoài, hãy dùng AWS CodeArtifact để quản lý các package nội bộ. Bạn có thể định nghĩa danh sách các upstream an toàn, chặn hoàn toàn các cuộc tấn công typosquatting (kẻ xấu tạo package có tên gần giống package thật).
* **Kiểm tra npm provenance attestation:** Một tính năng giúp xác minh package bạn tải về thực sự được build từ đúng mã nguồn và luồng CI/CD của tác giả, chống giả mạo artifact.

### 4. Quét (Scanning) tự động và liên tục
Các công cụ quét lỗ hổng truyền thống (chỉ dựa vào mã CVE) sẽ bó tay trước các mã độc Zero-day chưa từng được báo cáo.
* Tích hợp Amazon Inspector vào thẳng luồng CI/CD. Khác với quét CVE thông thường, Inspector sử dụng phân tích hành vi ở quy mô lớn để phát hiện các "sleeper packages" (mã độc ngủ đông) hoặc các package thu thập thông tin bất thường ngay cả trước khi chúng bị gán mã độc công khai (MAL-ID).
* Luôn chuẩn bị sẵn SBOMs (Software Bills of Materials) để biết chính xác ứng dụng của bạn đang xài những dependency nào, giúp cô lập thiệt hại cực nhanh khi có sự cố.

![Amazon Inspector và quy trình phản hồi tự động](/images/image3.png)
*Chú thích: Hình 3 - Amazon Inspector và quy trình phản hồi tự động: Inspector quét ECR, CodeArtifact, Lambda và phân tích hành vi để phát hiện các lỗ hổng Nghiêm trọng (Malicious Package, Credential Harvesting). Khi phát hiện, một sự kiện (finding event) được gửi đến pipeline phản hồi tự động (EventBridge -> Security Hub -> SNS -> Lambda Remediation).*

### 5. Tăng cường Logging & Monitoring
* Phải luôn bật AWS CloudTrail để audit toàn bộ API Call. Chú ý các hành động bất thường như: `sts:AssumeRole` từ dải IP lạ, hoặc `ecr:PutImage` được push thẳng từ máy dev mà bỏ qua CI/CD.
* Kết hợp Amazon GuardDuty và EventBridge để phát hiện kịp thời và kích hoạt các phản hồi tự động nếu có rủi ro xảy ra.

### Kiến trúc tổng thể:
![Kiến trúc tổng thể bảo mật chuỗi cung ứng phần mềm](/images/image3.png)
*Chú thích: Hình 4 - Kiến trúc tổng thể bảo mật chuỗi cung ứng phần mềm 5 giai đoạn: Ngăn chặn rò rỉ credential (IAM, Secrets Manager) -> Kiểm soát Dependency (CodeArtifact, CodeBuild) -> Xác thực ký & Quét (Inspector, Signer, EKS/ECS) -> Giám sát (GuardDuty, CloudTrail) -> Phản hồi tự động (SNS, Lambda).*

---

## Kết luận
Tóm lại: Bảo mật chuỗi cung ứng không chỉ là viết code an toàn, mà là xây dựng một kiến trúc nhiều lớp (defense in depth), giới hạn quyền và luôn có sự kiểm soát đối với mọi artifact đưa vào hệ thống.

