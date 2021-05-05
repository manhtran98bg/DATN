# Dự án giám sát máy công trình

## Công nghệ sử dụng

### Môi trường phát triển

- ReactJS (create-react-app)
- ReactNative (React Native CLI)
- Apollo React Client (GraphQL)
- NodeJs
- GraphQL
- TypeGraphQL
- @Mikro/orm
- PostgresSQL
- Redis
- Typescript

### Môi trường test & triển khai

- Docker Container

## Cài đặt

> Để chạy được dự án, cần cấu hình, cài đặt
>
> 1. PostgresSQL Database
> 2. NodeJS
> 3. Redis

## Danh sách MQTT Topic cho Device

> Ví dụ
> Brand = mandevices
> Device ID = deviceID_0001

| Topic                               | Ý nghĩa                                     | Ví dụ                                                        |
| ----------------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| `mandevices`/`deviceID_0001`/$name  | Tên thiết bị                                | Quan trắc môi trường                                         |
| `mandevices`/`deviceID_0001`/$state | Trạng thái thiết bị                         | `init`, `ready`, `disconnected`, `sleeping`, `lost`, `alert` |
| `mandevices`/`deviceID_0001`/$homie | Phiên bản Homie                             | 4.0.0                                                        |
| `mandevices`/`deviceID_0001`/$nodes | Danh sách nodes, ngăn cách nhau bởi dấu `,` | environment,location                                         |

## Danh sách MQTT Topic cho Node

> Ví dụ
> Brand = mandevices
> Device ID = deviceID_0001
> Node ID = environment

| Topic                                                  | Ý nghĩa                                     | Ví dụ                |
| ------------------------------------------------------ | ------------------------------------------- | -------------------- |
| `mandevices`/`deviceID_0001`/`environment`/$name       | Tên Node                                    | Thông số môi trường  |
| `mandevices`/`deviceID_0001`/`environment`/$type       | Loại Node                                   |                      |
| `mandevices`/`deviceID_0001`/`environment`/$properties | Danh sách `Property`, ngăn cách bởi dấu `,` | temperature,humidity |

Các Node dự án sử dụng

| Node        | Ý nghĩa                                                          |
| ----------- | ---------------------------------------------------------------- |
| environment | Nhóm các Property liên quan tới môi trường vận hành của thiết bi |
| vehicle     | Nhóm các Property liên quan tới thông số xe mà thiết bị theo dõi |
| device      | Nhóm các Property liên quan tới thiết bị                         |

## Danh sách MQTT Topic cho Property

> Ví dụ
> Brand = mandevices
> Device ID = deviceID_0001
> Node ID = environment
> Property ID = temperature

| Topic                                                              | Ý nghĩa      | Ví dụ                                            |
| ------------------------------------------------------------------ | ------------ | ------------------------------------------------ |
| `mandevices`/`deviceID_0001`/`environment`/`temperature`/$name     | Tên Property | Nhiệt độ                                         |
| `mandevices`/`deviceID_0001`/`environment`/`temperature`/$datatype | Kiểu dữ liệu | `integer`, `float`, `string`, `boolean`, `color` |
| `mandevices`/`deviceID_0001`/`environment`/`temperature`           | Giá trị      | 27.8                                             |

Các Property dự án sử dụng

| Node                     | Property     | Data type   | Ý nghĩa                           |
| ------------------------ | ------------ | ----------- | --------------------------------- |
| environment              | temperature  | Float       | Nhiệt độ môi trường               |
| environment              | humidity     | Float       | Độ ẩm môi trường                  |
| vehicle                  | temperature  | Float       | Nhiệt độ thiết bị                 |
| vehicle                  | rate_voltage | Float       | Điện áp làm việc của thiết bị     |
| vehicle                  | rpm          | Float       | Tốc độ vòng tua                   |
| session_xxx (session id) | start        | DateTime    | Thời gian bắt đầu phiên làm việc  |
| session_xxx (session id) | end          | DateTime    | Thời gian kết thúc phiên làm việc |
| driver                   | id           | Integer     | ID của người lái xe               |
| device                   | location     | GeoWithTime | Tọa độ của thiết bị kèm thời gian |

> `session id` sẽ được gửi từ Server xuống, sau khi quản lý lập kế hoạch làm việc cho xe & tài xế, phiên làm việc sẽ được khởi tạo và gửi thông tin xuống cho thiết bị.
> Ứng với thông tin `session` (id, thông tin tài xế, ...) thiết bị sẽ xử lý thông tin theo yêu cầu nghiệp vụ

## Kiểu dữ liệu Payload hỗ trợ

- Payload phải được gửi dưới dạng chuỗi được mã hóa UTF-8
- Dữ liệu Payload phải thuộc 1 trong các kiểu dữ liệu sau

1. String
1. Integer
1. Float
1. Percent
1. Boolean
1. Enum
1. Color
1. DateTime
   - Theo chuẩn ISO 8601
1. GeoWithTime
   - Dữ liệu dạng JSON gồm các Key sau
     | Key |Data Type|Ý nghĩa|
     |-----------|---------|-------|
     |long |Float |Kinh độ|
     |lat |Float |Vĩ độ|
     |time |DateTime |Thời điểm gửi tọa độ|
