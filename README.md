# Kết Nối IoT2050 Với S7-1200

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

- Bộ điều khiển **Siemens IoT2050** 
- PLC **Siemens S7-1200** : S7/1200,S7/1500 Series 
- **TIA Portal** : phần mềm lập trình cho PLC
- Kiến thức cơ bản **Node-RED** hoặc **MQTT** cho truyền thông IoT 
- **OPC UA Server** hoặc giao thức **Profinet** cho việc trao đổi dữ liệu
- **Python** nếu sử dụng lập trình với MQTT

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
   Với giao thức truyền thông S7, ví dụ như để truyền dữ liệu qua PROFINET và Ethernet công nghiệp của các CPU S7-1500 và S7-1200. Các lệnh sau đây có sẵn cho giao thức truyền thông S7:
PUT để gửi dữ liệu
GET để nhận dữ liệu
   - Từ giao diện TIA Portal, vào mục Properties --> Protection & Security --> Connection Mechanism của CPU S7-1200/1500, chọn Permiss access with Put/Get communication from remote Partner : 
     ![image](https://github.com/user-attachments/assets/d5ec8003-9f66-4e13-a00c-6ef8b7948ec7)

   - Việc này cho phép các thiết bị khác có thể truy xuất data từ CPU S7-1200/1500 , ở bài viết này IoT2050 đóng vai trò là client và CPU S7-1200/1500 đóng vai trò là server
2. **Data type:** (#2-datatype)
   Data maping có thể xem ở trang thư viện S7 connector trên website của Node-red : https://flows.nodered.org/node/node-red-contrib-s7
   Đối với các giá trị được sắp xếp theo Data Block trong Program Block, chúng ta cần phải thiết lập để xem được địa chỉ tuyệt đối ( Absolute Address) của giá trị

3. **Absolute Address:**
   - Chuột phải vào DB muốn truy cập để lấy địa chỉ tuyệt đối, bỏ chọn Optimize Block Access :
      ![image](https://github.com/user-attachments/assets/0cb7b3b4-0545-4dc4-902a-43a4d1e0c263)
   - Sau khi Compile Project, địa chỉ tuyệt đối sẽ xuất hiện, format được nhắc đến ở mục ##datatype :
     ![image](https://github.com/user-attachments/assets/d11d0cc9-f43b-4c8f-b6e1-81e25ca32886)



## Lập Trình Kết Nối

1. **Kết Nối Qua OPC UA:**
   - Sử dụng Python với thư viện `opcua`:
     ```bash
     pip install opcua
     ```
   - Mẫu mã Python kết nối với OPC UA Server:
     ```python
     from opcua import Client

     client = Client("opc.tcp://<PLC_IP>:4840")
     client.connect()
     root = client.get_root_node()
     print("Các node OPC UA:", root)
     ```

2. **Kết Nối Qua MQTT:**
   - Cấu hình Node-RED để nhận/gửi dữ liệu qua MQTT từ IoT2050.
   - Kết nối MQTT với PLC thông qua IoT Gateway trên IoT2050.

## Chạy Chương Trình

1. Khởi động PLC S7-1200 và đảm bảo nó hoạt động đúng với cấu hình đã cài đặt.
2. Khởi động IoT2050 và chạy Node-RED hoặc chương trình Python để lấy dữ liệu từ PLC.

## Hỗ Trợ

Nếu bạn gặp bất kỳ vấn đề nào trong quá trình thực hiện, hãy liên hệ qua email hoặc tạo một [issue](https://github.com/your_repo/issues) trên trang GitHub của dự án.


