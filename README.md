# Kết Nối IoT2050 Với PLC S7 Series

Bài viết hướng dẫn cách kết nối bộ IOT Gateway **Siemens IoT2050** với PLC **Siemens S7-1200** thông qua giao thức **S7 Connector**. 

![image](https://github.com/user-attachments/assets/8c0e4e1f-a46d-4040-8f82-260dc972bcca)


## Mục Lục

- [Yêu Cầu](#yêu-cầu)
- [Cấu Hình IoT2050](#cấu-hình-iot2050)
- [Cấu Hình S7-1200](#cấu-hình-s7-1200)
- [Cấu hình Kết Nối](#cấu-hình-kết-nối)
- [Testing](#testing)
- [Hỗ Trợ](#hỗ-trợ)

## Yêu Cầu

Trước khi bắt đầu, đảm bảo bạn đã chuẩn bị đầy đủ các yêu cầu sau:

- IoT Gateway **Siemens IoT2050** 
- PLC **Siemens S7-1200** : S7-300/400/1200,S7/1500 
- **TIA Portal** : phần mềm lập trình cho PLC
- Kiến thức cơ bản **Node-RED** hoặc **MQTT** cho truyền thông IoT 

## Cấu Hình IoT2050

1. **Cài Đặt OS dựa trên Image có sẵn:**
    https://support.industry.siemens.com/cs/document/109741799/downloads-for-simatic-iot20x0?dti=0&lc=en-VN
![image](https://github.com/user-attachments/assets/196336ea-ceb6-4996-b5cd-a1c718a607a3)
2. **Thiết lập kết nối Internet:**
   ![image](https://github.com/user-attachments/assets/ac3a0116-8523-453f-8bf7-cbf60892f1ce)

   - Test kết nối Internet:
     ```bash
     ping 8.8.8.8
     ```
   Putty : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
3. **SSH Access:**
   ![image](https://github.com/user-attachments/assets/4740c7a1-2148-46d5-8cb7-03eaec6d97c4)

## Cấu Hình S7-1200

   1. **PUT/GET config:**
   Với giao thức truyền thông S7, ví dụ như để truyền dữ liệu qua PROFINET và Ethernet công nghiệp của các CPU S7 Series. Các lệnh sau đây có sẵn cho giao thức truyền thông S7:
PUT để gửi dữ liệu
GET để nhận dữ liệu
   - Từ giao diện TIA Portal, vào mục Properties --> Protection & Security --> Connection Mechanism của CPU S7-1200/1500, chọn Permiss access with Put/Get communication from remote Partner : 
     ![image](https://github.com/user-attachments/assets/d5ec8003-9f66-4e13-a00c-6ef8b7948ec7)

   - Việc này cho phép các thiết bị khác có thể truy xuất data từ CPU S7-1200/1500 , ở bài viết này IoT2050 đóng vai trò là client và CPU S7-1200/1500 đóng vai trò là server
<a name="data-type"></a>
## 2. **Data type:** 
   Data maping có thể xem ở trang thư viện S7 connector trên website của Node-red : https://flows.nodered.org/node/node-red-contrib-s7
   Đối với các giá trị được sắp xếp theo Data Block trong Program Block, chúng ta cần phải thiết lập để xem được địa chỉ tuyệt đối ( Absolute Address) của giá trị

   3. **Absolute Address:**
   - Chuột phải vào DB muốn truy cập để lấy địa chỉ tuyệt đối, bỏ chọn Optimize Block Access :
      ![image](https://github.com/user-attachments/assets/0cb7b3b4-0545-4dc4-902a-43a4d1e0c263)
   - Sau khi Compile Project, địa chỉ tuyệt đối sẽ xuất hiện, format được nhắc đến ở mục [mục 2 - Data Type](#data-type):
     ![image](https://github.com/user-attachments/assets/d11d0cc9-f43b-4c8f-b6e1-81e25ca32886)



## Cấu Hình Kết Nối

1. **Node-Red:**
   - Ở Command Line Putty, gõ Node-red để truy cập vào giao diện Node-red 
   -  Gõ lệnh sau để có thể tải thư viện S7 connector : 
     ```bash
     npm install node-red-contrib-s7 
     ```
   - Từ giao diện node-red kéo thả node “s7 in” vào trang project để thiết lập kết nối với PLC S7-1200, điền các trường thông tin như IP của PLC, rack, slot, tên biến : 
     ![image](https://github.com/user-attachments/assets/c145b4b9-4ece-4bee-ab18-c58d5915b692)

     ![image](https://github.com/user-attachments/assets/0b6bccd1-f0d1-49c6-86ea-ad0c75b9387d)







    **Lưu ý** : với **S7-1200/1500**, rack 0 và slot 1
                     **S7-300/400** , rack 0 và slot 2

3. **Read Data:**
     Để hiển thị data, chúng ta cần phải kéo thả thêm node "msg.payload" , các giá trị sẽ được hiển thị ở tab Debug :
     ![image](https://github.com/user-attachments/assets/323822a2-48c0-4eb2-9a89-dd73c08189b1)

## Chạy Chương Trình

1. Khởi động PLC S7-1200 và đảm bảo nó hoạt động đúng với cấu hình đã cài đặt.
2. Khởi động IoT2050 và chạy Node-RED hoặc chương trình Python để lấy dữ liệu từ PLC.

## Hỗ Trợ

Nếu bạn gặp bất kỳ vấn đề nào trong quá trình thực hiện, hãy liên hệ qua email hoặc tạo một [issue](https://github.com/your_repo/issues) trên trang GitHub của dự án.


