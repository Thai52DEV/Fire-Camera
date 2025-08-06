# Hiện thực thiết kế
## 1. Mô tả tổng quan hệ thống
<img width="975" height="578" alt="image" src="https://github.com/user-attachments/assets/43460c2c-576e-493c-8d8a-e1d4cb9ceb9d" />
<img width="975" height="325" alt="image" src="https://github.com/user-attachments/assets/a5b86b4a-9a24-41ad-93e8-83c30344abfa" />

## 2. Gateway - Node

- Sử dụng chế độ AP mode của Gateway để gửi thông tin wifi đến các Node trong mạng lưới:
  + Đơn giản: Giao diện nhập thông tin wifi đơn giản cho người dùng hơn là mã code.
  + Tự động: Không cần setup thủ công cho từng Node.
  + Di động: Hệ thống có thể kết nối đến nhiều wifi khác nhau thay vì những wifi được thiết lập sẵn trong code.
<img width="1005" height="468" alt="image" src="" />
<p align="center">
  <img src="https://github.com/user-attachments/assets/91d8dd5a-9606-4a31-b6a9-0a5f70a3fc78" alt="Hình minh họa" width="1005" height="468"/>
</p>
<p align="center"><em>*Giao diện nhập wifi*</em></p>

**Giao diện nhập wifi**

- Ứng dụng giao thức mạng websocket hai chiều cho bài toán
  + Websocket là giao thức mạng có thể tạo kết nối liên tục chỉ trong lần đầu tiền kết nối (handshake), không cần kết nối nhiều lại nhiều lần, giảm delay.
  + Bên cạnh đó, giao thức websocket cho phép server chủ động gửi dữ liệu đến client mà không cần đợi client gửi request.

## 3. Servo

- Hệ thống được tích hợp 2 mô-đun servo quay 360 độ, cho phép điều chỉnh góc nhìn của camera một cách linh hoạt theo nhu cầu giám sát.
- Servo có tốc độ quay 0.1s/60 độ, quay 30 độ với mỗi tín hiệu từ người dùng một cách nhanh chóng
- Việc bổ sung khả năng xoay giúp camera bao quát khu vực rộng hơn, giảm điểm mù trong quan sát và tăng hiệu quả giám sát trong thời gian thực.
➔ Điều này đặc biệt quan trọng trong các môi trường có nguy cơ cháy nổ hoặc yêu cầu theo dõi chính xác vị trí mục tiêu.

## 4. AI nhận diện lửa

- Mô hình trí tuệ nhân tạo (YOLOv11) được xây dựng và huấn luyện để nhận diện đám cháy thông qua phân tích hình ảnh truyền về từ camera.
- Mô hình này được tích hợp trực tiếp trên server để thực hiện suy luận thời gian thực, giúp hệ thống có khả năng cảnh báo sớm khi phát hiện tín hiệu của lửa.
- Nhờ vậy, người dùng có thể phản ứng kịp thời và giảm thiểu thiệt hại tiềm ẩn.

## 5. Giao diện

- Hệ thống cung cấp một giao diện web thân thiện, cho phép người dùng truy cập và quan sát hình ảnh trực tiếp từ camera ở bất kỳ đâu có kết nối internet. 
- Ngoài ra, giao diện còn hỗ trợ điều khiển từ xa đối với hướng quay của camera, nâng cao khả năng tương tác và điều hành. 
- Thiết kế giao diện đảm bảo dễ sử dụng, trực quan và tối ưu cho cả máy tính và thiết bị di động.

## 6. Server

- Máy chủ đóng vai trò là trung tâm tiếp nhận, xử lý và triển khai mô hình trí tuệ nhân tạo, thực hiện quá trình suy luận nhằm nhận diện đám cháy và đưa ra cảnh báo.
- Đồng thời, server xử lý các yêu cầu điều khiển từ người dùng gửi về và phân phối chúng tới các thành phần như camera và servo. 
- Kiến trúc tập trung này giúp hệ thống hoạt động ổn định và có thể mở rộng linh hoạt.

# III. Kiểm tra, đánh giá

## 1. Ưu điểm:

- Servo quay được nhiều hướng, phản hồi nhanh, ổn định và chính xác
- Mô hình AI có thể nhận diện với độ ổn định
- Giao diện trực quan, thân thiện với người dùng, dễ sử dụng

## 2. Nhược điểm:

- Hình ảnh từ camera kém chất lượng, độ phân giải thấp, một khung hình có kích thước 240 x 240
- Còn nhận diện nhầm lẫn, độ confident chia ngưỡng cao nhưng vẫn nhận dạng nhầm lẫn với vật thể màu sáng
- Thiết bị chưa thể hoạt động trong thời gian dài, dễ trở nên nóng sau 30 phút đến 1 giờ

# Đề xuất cải tiến
-	Nâng cấp phần cứng, ngoại vi của hệ thống, cải tiện lại thiết bị gateway cần có khả năng chuyển tiếp dữ liệu tốt hơn
-	Áp dụng RTOS, đa luồng để hỗ trợ xử lý real time
-	Tích hợp cơ chế lưu trữ cục bộ và kết nối lại tự động khi bị mất kết nối
-	Thêm cơ chế cảnh báo tới người dùng (qua email hoặc tin nhắn…)
-	Thêm cơ chế xem lại video, truy vết, tìm nguyên nhân gây cháy nổ
-	Huấn luyện mô hình AI với ngữ cảnh cụ thể (phòng bếp, phòng ngủ…)

# V. Tài liệu tham khảo

[1] ESP32-CAM – Truyền video không dây  
https://randomnerdtutorials.com/esp32-cam-video-streaming-web-server-camera-homeassistant/  
https://www.instructables.com/ESP32-CAM-WEB-Server-and-Getting-Started-Guide/

[2] Servo điều khiển góc quay camera  
https://www.instructables.com/DIY-Pan-Tilt-Control-Using-Servos-for-ESP32-Cam-Wi/

[3] AI nhận diện lửa – Link Data set:  
https://github.com/OlafenwaMoses/FireNET/releases/download/v1.0/fire-dataset.zip

[4] Kết nối nhiều ESP32-CAM đến một ESP32 làm Access Point  
https://randomnerdtutorials.com/esp32-access-point-ap-web-server/

[5] Giao thức WebSocket  
https://randomnerdtutorials.com/esp32-websocket-server-arduino/

[6] Slide lí thuyết môn Thiết kế hệ thống nhúng không dây – Trường ĐH Công Nghệ Thông Tin – ĐHQG TPHCM

