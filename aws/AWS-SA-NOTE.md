## IAM
### IAM Security Tools Audit:
- IAM Credentials Report (account-level): cho phép xem tất cả status của các user trong cùng account
- IAM Access Advisor (user-level): Show chi tiết các permission được gán cho user, được dùng để check policy

## Networking & VPC, Subnet
- NACL là stateless, nghĩa là cho phép inbound thì cũng phải cho phép luôn outbound, còn Security Group là statefull, cho phép inbound thì tự động cho outbound luôn
- NACL có deny và allow rule, thứ tự duyệt từ trên xuống dưới, rule nào có số nhỏ hơn sẽ được ưu tiên hơn. SG thì chỉ cho phép allow rule, và sẽ là union của các rule này
- SG rule có thể reference bằng IP hoặc SG khác

## EC2
- Instance store là storage gắn liền với hardware của EC2 nên chạy nhanh nhưng chỉ lưu dữ liệu tạm thời và dễ dàng bị mất. EBS và EFS phải attach qua mạng nên chậm hơn
- User data được chạy lần boot đầu tiên với quyền root
- EBS gắn liền với EC2
- EFS chỉ tương thích với Linux, không dùng cho Windows
- Elastic Network Interface (ENI) chỉ đơn giản là card mạng bt, ko tối ưu cho HPC, Elastic Network Adapter (ENA) thì xịn hơn ENI, Elastic Fabric Adapter xịn nhất trong họ

## S3
- S3 mặc định cung cấp strong consistency cho tất cả các thao tác ghi và đọc (read after write), nghĩa là nếu ghi xong rồi thì sẽ luôn thấy kết quả mới

## RDS

## Database

## SNS & SQS
### SQS
- SQS có 2 loại: standard và fifo
  - Queue có visible time, khi một message được consume bởi 1 consumer, nó sẽ invisible với tất cả consumer khác cho tới khi hết visible time mà không bị consumer kia xóa khỏi queue
  - Standard không giới hạn băng thông, nhưng xử lý không có thử tự: at least 1
  - Fifo xử lý có thứ tự, đảm bảo exact 1 message
  - Không thể convert Standard về fifo mà phải xóa đi tạo mới, fifo queue phải có suffix .fifo
### SNS
##### SNS cơ chế là push, tức là consumer sẵn sàng, có là xử lý luôn, còn SQS cơ chế pull, nghĩa là data cứ được lưu mãi trong queue cho tới khi hết hạn hoặc có consumer chủ động kéo về

## Cloudwatch
### Cloudwatch alarm
- Có thể trigger các hành động liên quan tới EC2: start, stop, recover,...
- Phân biệt Cloudwatch, CloudTrail, Config
  - Think resource performance monitoring, events, and alerts; think Amazon CloudWatch.
  - Think account-specific activity and audit; think AWS CloudTrail.
  - Think resource-specific history, audit, and compliance; think AWS Config.

## KMS

## Route53

## CloudFront
- CloudFront cho performance tốt hơn Global Accelerator nhưng chỉ phù hợp cho http/https. Global Accelerator phù hợp cho udp/tcp

## AWS Config, Parameter Store, Secret Manager
### Parameter Store
- Parameter Store lưu cấu hình dạng key-value
- Lưu cả plaintext và secret
- Tích hợp trực tiếp với EC2, Lambda, Elastic Beanstalk
- Cho phép quản lý phiên bản

### AWS Secrets Manager
- Chuyên cho các thông tin bảo mật
- Tích hợp với RDS, Redshift, DocumentDB
- Dùng KMS để mã hóa thông tin

### AWS Config
- Theo dõi thay đổi cấu hình tài nguyên AWS
- Đánh giá tài nguyên có tuân thủ các yêu cầu bảo mật không
- Tích hợp CloudTrail để theo dõi sự thay đổi cấu hình người dùng, dịch vụ
- Cho phép tạo custom rule để kiểm tra cấu hình có tuân thủ không để gửi noti nếu vi phạm

## Các service khác  
### GuardDuty
  - Giám sát liên tục và bảo vệ tài khoản AWS, workloads, S3 data
  - Dựa vào các dữ liệu từ AWS CloudTrail Events, Amazon VPC Flow Logs, and DNS Logs
  
### Macie 
- Bảo vệ dữ liệu lưu trong s3 bằng cách sử dụng AI để tìm kiếm và bảo vệ các dữ liệu nhạy cảm như PII

### EventBridge 
- Là service quản lý event một cách hiệu quả, nhận event từ nhiều nguồn và đưa vào bus, có thể dựa vào rule để đưa tới các đích như sqs, sns, Kinesis Data Firehose, lambda
- SQS chỉ là 1 hàng đợi task, sns là dịch vụ thông báo, nằm ở giữa producer và consumer, còn eventbridge giống như 1 ông nằm ngay sau producer giúp lọc các event vào các bus tương ứng, sqs và sns sẽ nhận event từ các bus này

### AWS Trusted Advisor 
- Là một dịch vụ AWS hỗ trợ giám sát và tối ưu hóa môi trường AWS của bạn bằng cách cung cấp các khuyến nghị về bảo mật, hiệu suất, chi phí, khả năng phục hồi và giới hạn dịch vụ. Trusted Advisor giúp bạn tối ưu hóa cơ sở hạ tầng AWS, đảm bảo rằng các tài nguyên đang hoạt động hiệu quả, an toàn, và không tiêu tốn nhiều chi phí không cần thiết.
- Nó chỉ đưa ra khuyến nghị chứ không thực hiện việc thay đổi nào

### Amazon FSx

### CloudFormation
- CloudFormation StackSets nâng cao hơn của stack, cho phép deploy cùng 1 template tới nhiều account trong nhiều regions khác nhau, đảm bảo tính đồng nhất

### Storage Gateway
- Gồm 3 loại: File Gateway (support giao thức SMB và NFS), Volumn Gateway và Tape Gateway
- Dữ liệu sẽ được lưu xuống S3

### Transfer Family
- Chỉ cho phép truyền file tới S3 và EFS

### DataSync
- Cho phép truyền file tới nhiều service: S3, EFS, FSx for Windows File Server, CloudWatch, CloudTrail
