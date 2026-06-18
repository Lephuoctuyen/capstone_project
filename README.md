# Capstone Project: Mô phỏng và đánh giá cơ chế điều khiển tắc nghẽn trong mạng Data Center cho ứng dụng AI/ML phân tán

## 1. Giới thiệu

Repository này được xây dựng phục vụ đồ án tốt nghiệp với định hướng nghiên cứu, mô phỏng và đánh giá các cơ chế điều khiển tắc nghẽn trong hạ tầng mạng trung tâm dữ liệu, đặc biệt đối với các ứng dụng AI/ML phân tán.

Trong các hệ thống huấn luyện mô hình học sâu hiện đại, nhiều GPU hoặc server cần trao đổi dữ liệu với cường độ lớn trong thời gian ngắn, điển hình là quá trình đồng bộ tham số hoặc gradient thông qua các thuật toán collective communication như All-Reduce. Đặc điểm lưu lượng này có thể tạo ra các đợt burst traffic, làm tăng độ dài hàng đợi tại switch, gây tắc nghẽn, tăng độ trễ, kích hoạt PFC hoặc làm giảm hiệu năng huấn luyện tổng thể.

Đồ án tập trung vào việc xây dựng môi trường mô phỏng dựa trên ASTRA-sim và NS-3 để phân tích hành vi mạng trong các kịch bản AI/ML distributed training, từ đó đánh giá các cơ chế giảm tắc nghẽn như ECN, PFC, DCQCN và các hướng cải tiến liên quan đến điều chỉnh ngưỡng ECN động.

## 2. Mục tiêu của đồ án

Các mục tiêu chính của đồ án gồm:

* Nghiên cứu đặc điểm lưu lượng mạng trong hệ thống AI/ML phân tán.
* Tìm hiểu kiến trúc mạng Data Center dạng Spine-Leaf.
* Phân tích vai trò của RDMA/RoCEv2 trong truyền dữ liệu hiệu năng cao.
* Mô phỏng quá trình trao đổi dữ liệu giữa nhiều node/GPU bằng ASTRA-sim.
* Tích hợp backend mạng NS-3 để đánh giá hành vi mạng ở mức gói tin.
* Khảo sát các cơ chế điều khiển tắc nghẽn như ECN, PFC, DCQCN.
* Đánh giá các chỉ số như queue length, PFC event, flow completion time và thời gian truyền thông.
* Đề xuất hướng cải tiến cơ chế đánh dấu ECN động dựa trên trạng thái hàng đợi tại switch.

## 3. Phạm vi nghiên cứu

Đồ án tập trung vào môi trường mô phỏng, không triển khai trực tiếp trên phần cứng switch thực tế. Phạm vi chính bao gồm:

* Mô hình mạng Data Center sử dụng kiến trúc Spine-Leaf.
* Mô phỏng workload AI/ML phân tán bằng ASTRA-sim.
* Sử dụng NS-3 làm network backend để mô phỏng truyền gói, hàng đợi, buffer và cơ chế điều khiển tắc nghẽn.
* Đánh giá ảnh hưởng của lưu lượng burst đến hàng đợi switch và hiệu năng truyền thông.
* Phân tích cơ chế ECN/PFC/DCQCN trong môi trường lossless Ethernet/RDMA.

## 4. Cấu trúc thư mục

```text
capstone_project/
├── core_source_network_ns3/
│   └── astra-network-ns3/
│       └── Source code backend mạng NS-3 dùng cho mô phỏng network-level
│
├── create_traffic_workload_AI/
│   └── collectiveapi/
│       └── Công cụ và API hỗ trợ tạo workload collective communication cho AI/ML
│
├── simulation/
│   └── astra-sim/
│       └── Bộ mô phỏng ASTRA-sim phục vụ mô phỏng hệ thống AI/ML phân tán
│
└── README.md
```

## 5. Mô tả các thành phần chính

### 5.1. `core_source_network_ns3/astra-network-ns3`

Thư mục này chứa phần source code backend mạng dựa trên NS-3. Đây là thành phần dùng để mô phỏng chi tiết hành vi mạng ở mức packet-level, bao gồm:

* Topology mạng Data Center.
* Link giữa server, leaf switch và spine switch.
* Hàng đợi tại switch.
* Buffer và cơ chế quản lý bộ nhớ switch.
* ECN marking.
* PFC pause/resume.
* Cơ chế điều khiển tắc nghẽn như DCQCN.
* Thu thập các chỉ số mô phỏng như queue length, PFC event và flow completion time.

Đây là phần quan trọng để đánh giá tác động của tắc nghẽn mạng đối với workload AI/ML.

### 5.2. `create_traffic_workload_AI/collectiveapi`

Thư mục này phục vụ quá trình tạo workload giao tiếp cho các ứng dụng AI/ML phân tán. Trong hệ thống huấn luyện phân tán, các node thường trao đổi dữ liệu thông qua các thuật toán collective communication như:

* All-Reduce
* Broadcast
* Reduce
* All-Gather
* Reduce-Scatter

Workload được tạo ra từ phần này có thể được sử dụng làm đầu vào cho ASTRA-sim nhằm mô phỏng quá trình truyền thông giữa các node/GPU.

### 5.3. `simulation/astra-sim`

Thư mục này chứa ASTRA-sim, bộ mô phỏng hệ thống AI/ML phân tán. ASTRA-sim cho phép mô phỏng các thành phần ở mức hệ thống như:

* Workload AI/ML.
* Collective communication.
* Thời gian tính toán của GPU.
* Thời gian truyền thông giữa các node.
* Ảnh hưởng của mạng đến hiệu năng huấn luyện.

Khi kết hợp với `astra-network-ns3`, ASTRA-sim có thể đánh giá chi tiết tác động của network congestion đến quá trình huấn luyện AI/ML phân tán.

## 6. Kiến trúc mô phỏng tổng quát

Luồng mô phỏng tổng quát của đồ án có thể mô tả như sau:

```text
AI/ML Workload
      ↓
Collective Communication
      ↓
ASTRA-sim
      ↓
NS-3 Network Backend
      ↓
Spine-Leaf Data Center Network
      ↓
ECN / PFC / DCQCN Evaluation
      ↓
Kết quả mô phỏng và phân tích
```

Trong đó:

* ASTRA-sim mô phỏng quá trình huấn luyện và trao đổi dữ liệu giữa các node.
* NS-3 mô phỏng mạng truyền thông bên dưới.
* Switch trong mô phỏng xử lý hàng đợi, buffer, ECN marking và PFC.
* Kết quả đầu ra được dùng để đánh giá mức độ tắc nghẽn và hiệu quả của cơ chế điều khiển tắc nghẽn.

## 7. Các cơ chế mạng được nghiên cứu

### 7.1. ECN - Explicit Congestion Notification

ECN là cơ chế cho phép switch đánh dấu gói tin khi phát hiện dấu hiệu tắc nghẽn thay vì drop gói ngay lập tức. Khi phía nhận phát hiện gói có dấu ECN, nó có thể gửi phản hồi về phía gửi để giảm tốc độ truyền.

Trong đồ án, ECN được xem là tín hiệu cảnh báo sớm giúp hạn chế queue tăng quá cao.

### 7.2. PFC - Priority Flow Control

PFC là cơ chế pause theo từng priority, thường được sử dụng trong môi trường lossless Ethernet để hạn chế mất gói đối với lưu lượng RDMA/RoCEv2.

Tuy nhiên, nếu PFC bị kích hoạt quá nhiều, hệ thống có thể gặp các vấn đề như tăng độ trễ, lan truyền nghẽn hoặc ảnh hưởng đến hiệu năng tổng thể.

### 7.3. DCQCN - Data Center Quantized Congestion Notification

DCQCN là cơ chế điều khiển tắc nghẽn thường được sử dụng cho RoCEv2 trong Data Center. Cơ chế này kết hợp:

* ECN marking tại switch.
* CNP phản hồi từ phía nhận.
* Điều chỉnh tốc độ gửi tại RNIC của sender.

DCQCN giúp giảm tốc độ truyền trước khi hàng đợi tăng quá cao, từ đó hạn chế PFC và cải thiện hiệu năng truyền thông.

### 7.4. Hướng cải tiến ECN động

Bên cạnh cơ chế ECN tĩnh, đồ án định hướng nghiên cứu điều chỉnh ngưỡng ECN động dựa trên trạng thái hàng đợi tại switch. Ý tưởng chính là:

* Khi hàng đợi có xu hướng tăng cao, giảm ngưỡng ECN để đánh dấu sớm hơn.
* Khi hàng đợi thấp và mạng ổn định, nới ngưỡng ECN để tránh đánh dấu không cần thiết.
* Sử dụng thông tin queue hiện tại hoặc giá trị trung bình EWMA để phản ánh xu hướng tắc nghẽn.
* Hạn chế kích hoạt PFC bằng cách phản ứng sớm trước khi queue vượt ngưỡng nguy hiểm.

## 8. Các chỉ số đánh giá

Đồ án sử dụng các chỉ số chính sau để phân tích hiệu năng:

| Chỉ số                   | Ý nghĩa                                       |
| ------------------------ | --------------------------------------------- |
| Queue Length             | Độ dài hàng đợi tại switch theo thời gian     |
| PFC Count                | Số lần switch gửi tín hiệu pause              |
| PFC Duration             | Thời gian pause do PFC gây ra                 |
| Flow Completion Time     | Thời gian hoàn thành truyền dữ liệu của flow  |
| Communication Time       | Thời gian truyền thông giữa các node          |
| GPU Cycle / Compute Time | Thời gian tính toán hoặc chu kỳ xử lý của GPU |
| Link Utilization         | Mức sử dụng băng thông trên các liên kết      |
| Packet Drop              | Số lượng gói bị drop nếu có                   |

Các chỉ số này giúp đánh giá cơ chế điều khiển tắc nghẽn có làm giảm queue, giảm PFC và cải thiện thời gian truyền thông hay không.

## 9. Môi trường thực hiện

Môi trường khuyến nghị:

* Hệ điều hành: Ubuntu hoặc Ubuntu trên WSL
* Công cụ quản lý mã nguồn: Git
* Ngôn ngữ chính: C++, Python, C, Shell
* Mô phỏng mạng: NS-3
* Mô phỏng hệ thống AI/ML: ASTRA-sim
* Trình biên dịch: GCC/G++
* Công cụ build: CMake, Make

## 10. Hướng dẫn clone repository

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

## 11. Hướng dẫn sử dụng tổng quát

### Bước 1: Chuẩn bị workload AI/ML

Di chuyển vào thư mục tạo workload:

```bash
cd create_traffic_workload_AI/collectiveapi
```

Tại đây, người dùng có thể tạo hoặc chỉnh sửa các workload collective communication phục vụ mô phỏng AI/ML phân tán.

### Bước 2: Chuẩn bị ASTRA-sim

Di chuyển vào thư mục ASTRA-sim:

```bash
cd ../../simulation/astra-sim
```

Thành phần này được dùng để mô phỏng quá trình huấn luyện phân tán và liên kết với backend mạng.

### Bước 3: Chuẩn bị backend mạng NS-3

Di chuyển vào thư mục network backend:

```bash
cd ../../core_source_network_ns3/astra-network-ns3
```

Thành phần này mô phỏng mạng Data Center, bao gồm topology, switch, queue, buffer và cơ chế điều khiển tắc nghẽn.

### Bước 4: Chạy mô phỏng

Tùy theo cấu hình cụ thể của từng kịch bản, người dùng cần thiết lập:

* File topology mạng.
* File workload AI/ML.
* Tham số băng thông link.
* Tham số độ trễ link.
* Cơ chế điều khiển tắc nghẽn.
* Thời gian mô phỏng.
* Các file output cần thu thập.

Sau khi cấu hình xong, tiến hành build và chạy mô phỏng theo hướng dẫn tương ứng trong từng thư mục thành phần.

## 12. Kết quả đầu ra mong muốn

Sau khi chạy mô phỏng, các kết quả có thể bao gồm:

```text
queue_length_result.txt
pfc_result.txt
fct_result.txt
simulation_trace.tr
communication_time_result.txt
```

Các file này được dùng để vẽ biểu đồ và phân tích hiệu năng mạng trong từng kịch bản.

## 13. Ý nghĩa thực tiễn của đồ án

Đồ án có ý nghĩa trong bối cảnh các Data Center hiện đại ngày càng phải phục vụ các workload AI/ML có lưu lượng lớn, thời gian truyền ngắn và yêu cầu hiệu năng cao. Việc mô phỏng và đánh giá các cơ chế điều khiển tắc nghẽn giúp:

* Hiểu rõ nguyên nhân gây nghẽn trong mạng Data Center.
* Đánh giá vai trò của ECN, PFC và DCQCN.
* Phân tích ảnh hưởng của buffer và queue đến hiệu năng truyền thông.
* Hỗ trợ thiết kế chính sách điều khiển tắc nghẽn phù hợp hơn cho workload AI/ML.
* Làm cơ sở cho các hướng nghiên cứu tiếp theo về Dynamic ECN, Dynamic Load Balancing hoặc tối ưu hóa mạng RDMA/RoCEv2.

## 14. Định hướng phát triển tiếp theo

Một số hướng có thể tiếp tục phát triển:

* Hoàn thiện cơ chế điều chỉnh ngưỡng ECN động dựa trên EWMA queue.
* So sánh chi tiết giữa DCQCN, DCTCP, TIMELY và HPCC.
* Tích hợp cơ chế Dynamic Load Balancing dựa trên độ dài hàng đợi.
* Tối ưu tham số PFC để hạn chế pause storm và head-of-line blocking.
* Mở rộng topology Spine-Leaf với số lượng node lớn hơn.
* Tạo thêm workload AI/ML đa dạng hơn, ví dụ nhiều kích thước message và nhiều kiểu collective khác nhau.
* Tự động hóa quá trình chạy mô phỏng và vẽ biểu đồ kết quả.

## 15. Tác giả

Sinh viên thực hiện: **Lê Phước Tuyền**

Đề tài: **Nghiên cứu cơ chế điều khiển tắc nghẽn và thiết kế hạ tầng mạng Data Center cho các ứng dụng AI/ML phân tán**

Mục đích repository: Lưu trữ source code, workload, môi trường mô phỏng và các thành phần phục vụ đồ án tốt nghiệp.

## 16. Ghi chú

Repository này được xây dựng với mục đích học thuật và nghiên cứu. Các kết quả mô phỏng phụ thuộc vào cấu hình topology, workload, tham số mạng và cơ chế điều khiển tắc nghẽn được sử dụng trong từng kịch bản.
