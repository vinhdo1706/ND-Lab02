# ND-Lab02 — Advanced OSPF Redundant WAN

## Giới thiệu

Bài thực hành môn Thiết kế mạng được triển khai bằng Cisco Packet Tracer 9.0.1. Mạng gồm **4 router Cisco 2811 (R1–R4), 4 switch Cisco 2960 (SW1–SW4), 10 PC (PC0–PC9), 4 LAN và 5 liên kết WAN**. Định tuyến động sử dụng **OSPF process 1, Area 0**. Các subnet LAN và WAN đều sử dụng mặt nạ `/24` theo đề bài.

## Sơ đồ logic và địa chỉ mạng

| Phân đoạn | Subnet | Kết nối |
|---|---|---|
| LAN1 | `192.168.1.0/24` | R1 — SW1 — PC0, PC1, PC2 |
| LAN2 | `192.168.2.0/24` | R3 — SW2 — PC3, PC4, PC5 |
| LAN3 | `192.168.3.0/24` | R4 — SW3 — PC6, PC7 |
| LAN4 | `192.168.4.0/24` | R4 — SW4 — PC8, PC9 |
| WAN1 | `10.0.1.0/24` | R1 — R2 |
| WAN2 | `10.0.2.0/24` | R1 — R4 |
| WAN3 | `10.0.3.0/24` | R2 — R3 |
| WAN4 | `10.0.4.0/24` | R4 — R3 |
| WAN5 | `10.0.5.0/24` | R1 — R3 |

Các cổng kết nối thực tế và phía cấp clock DCE được liệt kê trong `PORT_MAPPING.csv`. Địa chỉ IP, subnet mask và default gateway của các máy tính được liệt kê trong `PC_ADDRESSING.csv`.

## Cấu hình và kiểm tra

Router được cấu hình địa chỉ IPv4 trên các interface LAN/WAN, OSPF Area 0 và `passive-interface` ở các LAN thích hợp. Các liên kết Serial được cấp clock tại phía DCE. Switch sử dụng IPv4 quản lý trên `interface Vlan1`.

Các lệnh kiểm tra chính trên router:

```text
show ip interface brief
show ip ospf neighbor
show ip route ospf
show ip route 192.168.2.0
show controllers serial 0/0/0
```

Kiểm thử bao gồm ping, traceroute hai chiều giữa các LAN; trạng thái láng giềng OSPF; bảng định tuyến; mô phỏng ARP, ICMP, OSPF; và thử ngắt/khôi phục liên kết WAN5 giữa R1–R3. Bảng `T01_T14_RESULTS.csv` ghi nhận kết quả của 14 phép kiểm tra trong đề bài.

Trong kịch bản ngắt WAN5, quan sát trên R1 cho thấy metric đến `192.168.2.0/24` thay đổi từ **65** (qua WAN5) thành **129** (qua các đường dự phòng), rồi trở lại **65** khi WAN5 được khôi phục. Tuyến PC0–PC9 có thể tiếp tục dùng WAN2 trực tiếp và không nhất thiết thay đổi khi WAN5 bị ngắt.

## Danh mục tệp

| Tệp | Mục đích |
|---|---|
| `20110112_DoThanhVinh_ND-Lab02.pkt` | Mô hình Packet Tracer |
| `R1.txt`–`R4.txt` | Cấu hình CLI router |
| `SW1.txt`–`SW4.txt` | Cấu hình CLI switch |
| `PC_ADDRESSING.csv` | Bảng địa chỉ thiết bị đầu cuối |
| `PORT_MAPPING.csv` | Ánh xạ cổng LAN/WAN và đầu DCE |
| `T01_T14_RESULTS.csv` | Kết quả 14 phép kiểm tra |
| `README.md` | Mô tả mô hình và thành phần dự án |
