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
     ![image](https://github.com/user-attachments/assets/6173aca5-58b7-4afb-870a-bd05823498f3)
   Putty : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
3. **SSH Access:**
   ![image](https://github.com/user-attachments/assets/4740c7a1-2148-46d5-8cb7-03eaec6d97c4)

## Cấu Hình S7-1200

1. **PUT/GET config:**

Bạn có thể sử dụng giao thức truyền thông S7, ví dụ như để truyền dữ liệu qua giao diện PROFINET tích hợp và giao diện Ethernet công nghiệp của các CPU S7-1500 và S7-1200. Các lệnh sau đây có sẵn cho giao thức truyền thông S7:

PUT để gửi dữ liệu
GET để nhận dữ liệu
   - Truy cập TIA Portal và tạo project mới.
   - Thêm PLC **S7-1200** vào project.
   - .
   - Định nghĩa các tag mà bạn muốn chia sẻ qua OPC UA.

2. **Get Absolute Address:**
   - Nếu bạn sử dụng Profinet, hãy cấu hình kết nối mạng Profinet giữa S7-1200 và IoT2050.
   - Sử dụng TIA Portal để gán địa chỉ IP và định cấu hình các I/O.

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


