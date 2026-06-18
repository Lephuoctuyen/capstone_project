# Nghiên cứu và thiết kế hạ tầng trung tâm dữ liệu cho các ứng dụng AI/ML

## 1. Giới thiệu đề tài

Repository này lưu trữ source code, công cụ tạo workload và môi trường mô phỏng phục vụ đồ án tốt nghiệp với đề tài:

**“Nghiên cứu và thiết kế hạ tầng trung tâm dữ liệu cho các ứng dụng AI/ML”**

Đồ án tập trung nghiên cứu hạ tầng mạng trung tâm dữ liệu cho các ứng dụng trí tuệ nhân tạo và học máy phân tán. Trong các hệ thống AI/ML hiện đại, nhiều máy chủ GPU phải trao đổi dữ liệu liên tục trong quá trình huấn luyện. Vì vậy, hạ tầng mạng cần đáp ứng các yêu cầu quan trọng như băng thông cao, độ trễ thấp, hạn chế mất gói và khả năng mở rộng tốt.

Nội dung chính của đồ án gồm nghiên cứu kiến trúc mạng Spine-Leaf, công nghệ truyền dữ liệu tốc độ cao RDMA/RoCEv2 và các cơ chế điều khiển tắc nghẽn trong mạng trung tâm dữ liệu như ECN, PFC, DCQCN, HPCC và TIMELY.

## 2. Thông tin đồ án

* **Sinh viên thực hiện:** Lê Phước Tuyền
* **Mã số sinh viên:** 106210108
* **Lớp:** 21DT2
* **Ngành:** Điện tử Viễn thông
* **Khoa:** Điện tử Viễn thông
* **Trường:** Trường Đại học Bách khoa - Đại học Đà Nẵng
* **Người hướng dẫn:** TS. Trần Thị Minh Hạnh, ThS. Phạm Hoàng Phương
* **Thời gian:** Đà Nẵng, 6/2026

## 3. Mục tiêu nghiên cứu

Mục tiêu của đồ án là tìm hiểu và đánh giá hạ tầng mạng phù hợp cho các ứng dụng AI/ML phân tán trong trung tâm dữ liệu. Cụ thể, đồ án tập trung vào các nội dung sau:

* Phân tích đặc điểm lưu lượng mạng trong các hệ thống AI/ML phân tán.
* Nghiên cứu yêu cầu hạ tầng mạng đối với cụm máy chủ GPU, bao gồm băng thông, độ trễ, mất gói và khả năng mở rộng.
* So sánh kiến trúc mạng truyền thống ba lớp với kiến trúc Spine-Leaf.
* Tìm hiểu công nghệ RDMA và RoCEv2 trong truyền dữ liệu tốc độ cao.
* Phân tích các cơ chế kiểm soát và điều khiển tắc nghẽn gồm ECN, PFC, DCQCN, HPCC và TIMELY.
* Xây dựng môi trường mô phỏng bằng ASTRA-SIM kết hợp NS-3.
* Đánh giá hiệu năng truyền dữ liệu giữa TCP/IP và RoCEv2.
* Đánh giá ảnh hưởng của các cơ chế điều khiển tắc nghẽn thông qua các chỉ số như kích thước hàng đợi, số lượng bản tin PFC và thời gian hoàn thành dòng chảy.

## 4. Phạm vi nghiên cứu

Phạm vi nghiên cứu của đồ án là hạ tầng mạng trung tâm dữ liệu phục vụ huấn luyện AI/ML phân tán. Đồ án không triển khai trên phần cứng thực tế mà thực hiện đánh giá thông qua mô phỏng.

Các đối tượng nghiên cứu chính gồm:

* Kiến trúc mạng Spine-Leaf.
* Giao thức TCP/IP.
* Công nghệ RDMA/RoCEv2.
* Cơ chế ECN.
* Cơ chế PFC.
* Thuật toán điều khiển tắc nghẽn DCQCN.
* Thuật toán điều khiển tắc nghẽn HPCC.
* Thuật toán điều khiển tắc nghẽn TIMELY.
* Công cụ mô phỏng ASTRA-SIM và NS-3.

## 5. Cấu trúc repository

```text
capstone_project/
├── core_source_network_ns3/
│   └── astra-network-ns3/
│       └── Source code backend mạng NS-3 phục vụ mô phỏng hạ tầng mạng
│
├── create_traffic_workload_AI/
│   └── collectiveapi/
│       └── Công cụ hỗ trợ tạo workload giao tiếp tập thể cho AI/ML
│
├── simulation/
│   └── astra-sim/
│       └── Bộ mô phỏng ASTRA-SIM phục vụ mô phỏng hệ thống AI/ML phân tán
│
└── README.md
```

## 6. Mô tả các thành phần chính

### 6.1. `core_source_network_ns3/astra-network-ns3`

Thư mục này chứa phần source code backend mạng dựa trên NS-3. Đây là thành phần dùng để mô phỏng hạ tầng mạng ở mức network-level, bao gồm topology, switch, link, hàng đợi, buffer và các cơ chế điều khiển tắc nghẽn.

Trong đồ án, NS-3 được sử dụng để mô phỏng mạng Spine-Leaf và đánh giá các chỉ số như:

* Kích thước hàng đợi tại switch.
* Số lượng bản tin PFC.
* Thời gian hoàn thành dòng chảy.
* Ảnh hưởng của tắc nghẽn đến quá trình truyền dữ liệu.

### 6.2. `create_traffic_workload_AI/collectiveapi`

Thư mục này phục vụ quá trình tạo workload giao tiếp cho các ứng dụng AI/ML phân tán. Trong huấn luyện AI/ML phân tán, các máy chủ GPU thường phải trao đổi dữ liệu thông qua các thao tác giao tiếp tập thể như:

* AllReduce
* AllGather
* ReduceScatter
* All-to-All

Các workload này được sử dụng làm đầu vào cho ASTRA-SIM để mô phỏng quá trình trao đổi dữ liệu giữa các node trong hệ thống AI/ML.

### 6.3. `simulation/astra-sim`

Thư mục này chứa ASTRA-SIM, công cụ mô phỏng hệ thống AI/ML phân tán. Trong đồ án, ASTRA-SIM được dùng để tạo và mô phỏng workload AI/ML, sau đó kết hợp với backend mạng NS-3 để đánh giá ảnh hưởng của hạ tầng mạng đến hiệu năng truyền dữ liệu.

ASTRA-SIM giúp mô phỏng các thành phần như:

* Workload AI/ML.
* Collective communication.
* Quá trình truyền dữ liệu giữa các node.
* Sự kết hợp giữa thời gian tính toán và thời gian truyền thông.

## 7. Kiến trúc mạng mô phỏng

Topology mạng trong đồ án được xây dựng theo kiến trúc Spine-Leaf. Đây là kiến trúc phù hợp với lưu lượng đông - tây giữa các máy chủ trong trung tâm dữ liệu hiện đại.

Mô hình mô phỏng gồm:

| Thành phần                | Giá trị              |
| ------------------------- | -------------------- |
| Tổng số node              | 144                  |
| Máy chủ/GPU node          | 128 node, ID 0–127   |
| Switch leaf               | 8 switch, ID 128–135 |
| Switch spine              | 8 switch, ID 136–143 |
| Tổng số liên kết          | 192                  |
| Liên kết server–leaf      | 128 link             |
| Liên kết leaf–spine       | 64 link              |
| Số server trên mỗi leaf   | 16                   |
| Số spine kết nối mỗi leaf | 8                    |

Đồ án xây dựng hai trường hợp mạng:

### 7.1. Mô hình có tắc nghẽn

Trong mô hình có tắc nghẽn:

* Liên kết server–leaf: 200 Gbps, độ trễ 0,005 ms.
* Liên kết leaf–spine: 200 Gbps, độ trễ 0,0125 ms.
* Tổng downlink mỗi leaf: 16 × 200 = 3200 Gbps.
* Tổng uplink mỗi leaf: 8 × 200 = 1600 Gbps.
* Tỷ lệ oversubscription: 2:1.

Với tỷ lệ oversubscription 2:1, khi nhiều server truyền dữ liệu đồng thời, leaf switch có thể trở thành điểm nghẽn.

### 7.2. Mô hình không tắc nghẽn theo lý thuyết

Trong mô hình không tắc nghẽn:

* Liên kết server–leaf: 200 Gbps, độ trễ 0,005 ms.
* Liên kết leaf–spine: 400 Gbps, độ trễ 0,0125 ms.
* Tổng downlink mỗi leaf: 16 × 200 = 3200 Gbps.
* Tổng uplink mỗi leaf: 8 × 400 = 3200 Gbps.
* Tỷ lệ oversubscription: 1:1.

Với tỷ lệ oversubscription 1:1, tổng băng thông uplink bằng tổng băng thông downlink, do đó mô hình có khả năng hạn chế nghẽn tại leaf tốt hơn.

## 8. Các công nghệ và cơ chế được nghiên cứu

### 8.1. RDMA

RDMA là công nghệ cho phép truyền dữ liệu trực tiếp giữa vùng nhớ của hai máy chủ thông qua card mạng, hạn chế sự tham gia của CPU và giảm số lần sao chép dữ liệu. Công nghệ này phù hợp với các hệ thống cần truyền dữ liệu lớn, độ trễ thấp và thông lượng cao như cụm AI/ML phân tán.

### 8.2. RoCEv2

RoCEv2 là công nghệ triển khai RDMA trên nền Ethernet. RoCEv2 cho phép tận dụng hạ tầng Ethernet phổ biến trong trung tâm dữ liệu nhưng vẫn hỗ trợ truyền dữ liệu hiệu năng cao.

Tuy nhiên, RoCEv2 nhạy cảm với mất gói. Vì vậy, mạng sử dụng RoCEv2 cần được kết hợp với các cơ chế như PFC, ECN và thuật toán điều khiển tắc nghẽn để duy trì hiệu năng ổn định.

### 8.3. ECN

ECN là cơ chế cho phép switch đánh dấu gói tin khi phát hiện dấu hiệu tắc nghẽn thay vì loại bỏ gói tin. Tín hiệu ECN giúp hệ thống phát hiện nghẽn sớm và hỗ trợ phía gửi điều chỉnh tốc độ truyền.

### 8.4. PFC

PFC là cơ chế điều khiển luồng theo mức ưu tiên. Khi hàng đợi tại switch tăng cao và có nguy cơ tràn buffer, switch có thể gửi bản tin PFC để yêu cầu phía gửi tạm dừng truyền dữ liệu đối với một mức ưu tiên nhất định.

PFC giúp hạn chế mất gói trong mạng RoCEv2, nhưng nếu bị kích hoạt quá nhiều có thể làm tăng độ trễ và ảnh hưởng đến hiệu năng toàn mạng.

### 8.5. DCQCN

DCQCN là cơ chế điều khiển tắc nghẽn phù hợp với mạng RoCEv2. Cơ chế này sử dụng ECN để phát hiện tắc nghẽn, CNP để phản hồi thông tin nghẽn và điều chỉnh tốc độ truyền tại phía gửi.

Trong đồ án, DCQCN được tập trung đánh giá vì có tính thực tế khi triển khai trong mạng RoCEv2 và có khả năng kiểm soát tắc nghẽn tốt.

### 8.6. HPCC

HPCC là cơ chế điều khiển tắc nghẽn có độ chính xác cao, sử dụng thông tin In-band Network Telemetry để thu thập trạng thái mạng trực tiếp. HPCC có khả năng phản ứng nhanh và đạt hiệu năng tốt, tuy nhiên yêu cầu thiết bị mạng hỗ trợ telemetry nên phức tạp hơn khi triển khai.

### 8.7. TIMELY

TIMELY là cơ chế điều khiển tắc nghẽn dựa trên độ trễ khứ hồi RTT. Cơ chế này đơn giản hơn so với HPCC, nhưng có thể phản ứng chậm hơn trong các workload AI/ML có lưu lượng burst và thay đổi nhanh.

## 9. Kịch bản mô phỏng

Đồ án thực hiện mô phỏng bằng cách kết hợp ASTRA-SIM và NS-3.

Trong đó:

* ASTRA-SIM dùng để mô phỏng workload AI/ML.
* NS-3 dùng để mô phỏng hạ tầng mạng Spine-Leaf.
* Workload sử dụng gồm tác vụ AllReduce và mô hình khuyến nghị học sâu DLRM.
* Các kịch bản tập trung so sánh TCP/IP với RoCEv2 và đánh giá các cơ chế điều khiển tắc nghẽn DCQCN, HPCC và TIMELY.

Luồng mô phỏng tổng quát:

```text
AI/ML workload
        ↓
ASTRA-SIM
        ↓
NS-3 network backend
        ↓
Spine-Leaf topology
        ↓
TCP/IP hoặc RoCEv2
        ↓
ECN / PFC / DCQCN / HPCC / TIMELY
        ↓
Kết quả mô phỏng
```

## 10. Các chỉ số đánh giá

Các chỉ số chính được sử dụng để đánh giá hiệu năng trong đồ án gồm:

| Chỉ số                        | Ý nghĩa                                                                  |
| ----------------------------- | ------------------------------------------------------------------------ |
| Thông lượng                   | Đánh giá khả năng truyền dữ liệu của mạng                                |
| Độ trễ                        | Đánh giá thời gian truyền dữ liệu giữa các node                          |
| Kích thước hàng đợi           | Phản ánh mức độ tắc nghẽn tại switch                                     |
| Số lượng bản tin PFC          | Cho biết mức độ thường xuyên switch phải tạm dừng luồng để tránh mất gói |
| Flow Completion Time          | Thời gian hoàn thành dòng chảy                                           |
| Thời gian hoàn thành workload | Đánh giá ảnh hưởng của mạng đến toàn bộ quá trình truyền thông AI/ML     |

## 11. Kết quả đánh giá chính

Kết quả mô phỏng trong đồ án cho thấy:

* RoCEv2 phù hợp hơn TCP/IP trong môi trường truyền dữ liệu tốc độ cao cho AI/ML phân tán.
* DCQCN có khả năng kiểm soát tắc nghẽn tốt và thực tế khi triển khai trong mạng RoCEv2.
* HPCC đạt hiệu năng tốt nhờ sử dụng telemetry để thu thập trạng thái mạng trực tiếp, nhưng yêu cầu thiết bị mạng hỗ trợ phức tạp hơn.
* TIMELY có cơ chế đơn giản hơn vì dựa trên RTT, nhưng có thể phản ứng chậm với lưu lượng thay đổi nhanh.
* Trong workload DLRM, HPCC đạt thời gian hoàn thành trung bình tốt nhất, DCQCN có hiệu năng gần với HPCC, còn TIMELY có thời gian hoàn thành lớn hơn.
* Việc sử dụng topology Spine-Leaf với tỷ lệ oversubscription phù hợp giúp hạn chế nghẽn tại leaf switch và cải thiện hiệu năng truyền dữ liệu.

## 12. Ý nghĩa thực tiễn

Đồ án có ý nghĩa trong việc nghiên cứu và đánh giá hạ tầng mạng cho các trung tâm dữ liệu phục vụ AI/ML phân tán. Khi số lượng GPU tăng lên, lưu lượng trao đổi giữa các máy chủ ngày càng lớn, đặc biệt trong các thao tác đồng bộ dữ liệu như AllReduce.

Việc lựa chọn kiến trúc Spine-Leaf, sử dụng RoCEv2 và triển khai các cơ chế điều khiển tắc nghẽn phù hợp giúp:

* Giảm nguy cơ tắc nghẽn trong mạng.
* Hạn chế mất gói đối với lưu lượng RDMA.
* Giảm thời gian chờ giữa các GPU.
* Cải thiện hiệu quả truyền dữ liệu trong huấn luyện AI/ML phân tán.
* Làm cơ sở cho việc thiết kế cụm máy chủ GPU trong trung tâm dữ liệu hiện đại.

## 13. Hướng dẫn clone repository

```bash
git clone https://github.com/Lephuoctuyen/capstone_project.git
cd capstone_project
```

Kiểm tra cấu trúc thư mục:

```bash
ls
```

Kết quả mong đợi:

```text
core_source_network_ns3
create_traffic_workload_AI
simulation
README.md
```

## 14. Ghi chú

Repository này được xây dựng phục vụ mục đích học thuật, nghiên cứu và mô phỏng trong khuôn khổ đồ án tốt nghiệp. Các kết quả mô phỏng phụ thuộc vào cấu hình topology, workload, tham số liên kết, cơ chế điều khiển tắc nghẽn và môi trường thực nghiệm được sử dụng.
