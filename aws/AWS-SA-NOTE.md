## IAM
### IAM Security Tools Audit:
- IAM Credentials Report (account-level): cho phép xem tất cả status của các user trong cùng account
- IAM Access Advisor (user-level): Show chi tiết các permission được gán cho user, được dùng để check policy

### Cognito
- Cognito User Pools hỗ trợ user bên ngoài đăng ký, đăng nhập qua username/pass hoặc qua 3rd party
- Amazon Cognito Identity Pools cho phép user bên trong AWS sử dụng các service của AWS

## Networking & VPC, Subnet
- NACL là stateless, nghĩa là cho phép inbound thì cũng phải cho phép luôn outbound, còn Security Group là statefull, cho phép inbound thì tự động cho outbound luôn
- NACL có deny và allow rule, thứ tự duyệt từ trên xuống dưới, rule nào có số nhỏ hơn sẽ được ưu tiên hơn. SG thì chỉ cho phép allow rule, và sẽ là union của các rule này
- SG rule có thể reference bằng IP hoặc SG khác

## EC2
- Spot instance có thể bị thu hồi hàng giờ vì thay đổi spot price, có thể dùng Spot Block để ngăn interrupt từ 1 tới 6h
- Instance store là storage gắn liền với hardware của EC2 nên chạy nhanh nhưng chỉ lưu dữ liệu tạm thời và dễ dàng bị mất. EBS và EFS phải attach qua mạng nên chậm hơn
- User data được chạy lần boot đầu tiên với quyền root
- EBS gắn liền với EC2, EFS chỉ tương thích với Linux, không dùng cho Windows. EFS dùng SG để điều khiển truy cập, được encrypt bởi KMS
- Elastic Network Interface (ENI) chỉ đơn giản là card mạng bt, ko tối ưu cho HPC, Elastic Network Adapter (ENA) thì xịn hơn ENI, Elastic Fabric Adapter xịn nhất trong họ

### Auto Scaling
- Target policy: giữ mức CPU trung bình cố định là 50%
- Simple scaling, step scaling: Nếu CPU trung bình là 30% thì tăng thêm 1 instance, nếu là 70% thì tăng thêm 3 instance
- Cơ chế scale in là xóa instance ở AZ có nhiều instance hơn để đảm bảo tính balance và high avaibility

### Launch Template/ Launch Configuration
- Launch Template là version mới hơn, hỗ trợ các spot instance và on demand
- 

## S3
- S3 mặc định cung cấp strong consistency cho tất cả các thao tác ghi và đọc (read after write), nghĩa là nếu ghi xong rồi thì sẽ luôn thấy kết quả mới
- S3 support 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket.
- Các version khác nhau của cùng 1 object có thể có các retention khác nhau. Mặc định retention period được chỉ định là khoảng thời gian (bao nhiêu ngày), còn khi chỉ định cụ thể phải dùng Retain Until Date
- S3 object mặc định được own bới người upload chứ không phải người tạo bucket nên mặc định người tạo bucket sẽ không có quyền với object được upload bởi người khác

## RDS
- Amazon RDS không cho phép custom OS, muốn custom phải dùng Amazon RDS Custom
- Read replicate là asynchronous, multi AZ là synchronous trong 1 AZ, Cross-AZ, or Cross-Region
- RDS cho phép enable storage autoscaling để tự động tăng dung lượng
- Read replicate cần thay đổi endpoint để trỏ tới
- 

## Database

## SNS & SQS
### SQS
- SQS có 2 loại: standard và fifo
  - Queue có visible time, khi một message được consume bởi 1 consumer, nó sẽ invisible với tất cả consumer khác cho tới khi hết visible time mà không bị consumer kia xóa khỏi queue
  - Standard không giới hạn băng thông, nhưng xử lý không có thử tự: at least 1
  - Fifo xử lý có thứ tự, đảm bảo exact 1 message
  - Không thể convert Standard về fifo mà phải xóa đi tạo mới, fifo queue phải có suffix .fifo
### SNS
### Kenisis
- Data Stream cho real time, Firehose cho lưu trữ tới 1 đích khác (không sp DynamoDB), Analytics chỉ có thể nhận từ DS hoặc Firehose, sau đó thực hiện phân tích
##### SNS cơ chế là push, tức là consumer sẵn sàng, có là xử lý luôn, còn SQS cơ chế pull, nghĩa là data cứ được lưu mãi trong queue cho tới khi hết hạn hoặc có consumer chủ động kéo về

## Lambda
- Hiện tại support 1000 concurrent lambda/ account/ region. Muốn nhiều hơn cần liên hệ support để tăng

## Cloudwatch
### Cloudwatch alarm
- Có thể trigger các hành động liên quan tới EC2: start, stop, recover,...
- Phân biệt Cloudwatch, CloudTrail, Config
  - Think resource performance monitoring, events, and alerts; think Amazon CloudWatch.
  - Think account-specific activity and audit; think AWS CloudTrail.
  - Think resource-specific history, audit, and compliance; think AWS Config.

## CloudTrail
- Tích hợp với Cloudwatch
- Không tích hợp trực tiếp với Kenisis

## EventBridge 
- Là service quản lý event một cách hiệu quả, nhận event từ nhiều nguồn và đưa vào bus, có thể dựa vào rule để đưa tới các đích như sqs, sns, Kinesis Data Firehose, lambda
- SQS chỉ là 1 hàng đợi task, sns là dịch vụ thông báo, nằm ở giữa producer và consumer, còn eventbridge giống như 1 ông nằm ngay sau producer giúp lọc các event vào các bus tương ứng, sqs và sns sẽ nhận event từ các bus này
  
## KMS
- Khi xóa KMS, chỉ bị xóa tạm thời, ở trong trạng thái PENDING và có thể cancel trong vòng 30 ngày để lấy lại
- Được tích hợp với CloudTrail để audit
- Không thể convert từ single region key sang multi region key
- Không thể share KMS key giữa các region

## Route53
- Geolocation: Chỉ định client ở location nào sẽ được trỏ tới IP nào
- 

## CloudFront
- CloudFront cho performance tốt hơn Global Accelerator nhưng chỉ phù hợp cho http/https. Global Accelerator phù hợp cho udp/tcp
- CloudFront có tính năng lọc request theo geo location

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

## WAF/Shield/
- WAF hỗ trợ chặn request theo vị trí địa lý, hỗ trợ IP match để block IP

## Các service khác  
### GuardDuty
  - Giám sát liên tục và bảo vệ tài khoản AWS, workloads, S3 data
  - Dựa vào các dữ liệu từ AWS CloudTrail Events, Amazon VPC Flow Logs, and DNS Logs
  
### Macie 
- Bảo vệ dữ liệu lưu trong s3 bằng cách sử dụng AI để tìm kiếm và bảo vệ các dữ liệu nhạy cảm như PII

### Inspector 
- Thực hiện kiểm tra security cho service

### AWS Trusted Advisor 
- Là một dịch vụ AWS hỗ trợ giám sát và tối ưu hóa môi trường AWS của bạn bằng cách cung cấp các khuyến nghị về bảo mật, hiệu suất, chi phí, khả năng phục hồi và giới hạn dịch vụ. Trusted Advisor giúp bạn tối ưu hóa cơ sở hạ tầng AWS, đảm bảo rằng các tài nguyên đang hoạt động hiệu quả, an toàn, và không tiêu tốn nhiều chi phí không cần thiết.
- Nó chỉ đưa ra khuyến nghị chứ không thực hiện việc thay đổi nào

### Amazon FSx
- Amazon FSx for Lustre: cho phép thực hiện các thao tác HPC (high-performance computing), được tích hợp với S3
- Amazon FSx for Windows File Server: thích hợp với Windows, cho phép truy cập thông qua SMB, build trên Windows Server nên support Microsoft Active Directory (AD), không coi S3 object là file, cho phép Microsoft’s Distributed File System (DFS)
- AWS Directory Service for Microsoft Active Directory (AWS Managed Microsoft AD) không cho phép Microsoft’s Distributed File System (DFS)

### Storage Gateway
- Gồm 3 loại: File Gateway (support giao thức SMB và NFS), Volumn Gateway và Tape Gateway
- Dữ liệu sẽ được lưu xuống S3
- File gateway hỗ trợ SMB or NFS
- Volume Gateway hỗ trợ iSCSI
- Tape Gateway hỗ trợ 

### Transfer Family
- Chỉ cho phép truyền file tới S3 và EFS

### DataSync
- Cho phép truyền file tới nhiều service: S3, EFS, FSx for Windows File Server, CloudWatch, CloudTrail, giữa onprem và AWS hoặc giữa các service AWS
- Hỗ trợ chuyển data tới rất nhiều service

### Direct Connect
- Mất 1 tháng để thực hiện kết nối vật lý từ on prem lên AWS, đảm bảo đường truyền tốc độ nhanh vì dùng mạng AWS thay vì internet
- Kết nối là riêng tư vì không qua mạng internet nhưng không được encrypt nên không bảo mật, cần phải tích hợp với cơ chế bảo mật khác như VPN

### Transit Gateway
- Tạo 1 kết nối chung giữa nhiều môi trường on prem và AWS thay vì nhiều kết nối site to site
- Chỉ là 1 cách kết nối mạng, không tối ưu cho performance

### Snow Family
- Snowcone support 8TB
- Snowball sp 80TB, hỗ trợ cluster
- Snowmobile sp 10PB

### CloudFormation
- CloudFormation StackSets nâng cao hơn của stack, cho phép deploy cùng 1 template tới nhiều account trong nhiều regions khác nhau, đảm bảo tính đồng nhất

## AWS Organization
- Để chuyển 1 account từ org A sang B, cần xóa acc khỏi A, gửi lời mời từ B và chấp nhận lời mời

## Chi phí
- Có chi phí chuyển data giữa các region
- S3 upload dùng SATA thì chỉ tính phần được tăng tốc, upload thường không mất phí
- S3 download mất phí
