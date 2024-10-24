## IAM

## Networking & VPC, Subnet

## EC2

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

## Cloudwatch

## KMS

## Route53

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
