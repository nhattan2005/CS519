# Multimodal Vision Mamba for Pathological Complete Response Prediction in Breast Cancer MRI

> **Đồ án cuối kỳ môn học CS519.Q11**
>
> **Nghiên cứu ứng dụng Vision Mamba đa phương thức trong dự đoán đáp ứng bệnh lý hoàn toàn (pCR)**

[![Mamba](https://img.shields.io/badge/Architecture-Vision%20Mamba-green)](https://github.com/state-spaces/mamba)
[![Dataset](https://img.shields.io/badge/Dataset-MAMA--MIA-red)](https://scientificdata.nature.com/)

## 👥 Thông tin nhóm (Team Info)

| STT | Họ và tên | MSSV|
|:---:|:---|:---:|
| 1 | **Phan Nhật Tân** | 23521405 |
| 2 | **Nguyễn Thái Sơn** | 23521356 |

- **Lớp:** CS519.Q11
- **Demo Video:** [YouTube Link](https://youtu.be/u8JvkWEmVOY)

---

## 📖 Tóm tắt (Abstract)

Ung thư vú là nguyên nhân tử vong hàng đầu ở phụ nữ toàn cầu. Việc dự đoán chính xác **Phản ứng hoàn toàn về mặt bệnh lý (pCR)** sau hóa trị tân bổ trợ (NAC) đóng vai trò then chốt để tối ưu hóa phác đồ điều trị.

Các mô hình Deep Learning hiện tại gặp hạn chế:
- **CNN (U-Net, ResNet):** Vùng tiếp nhận cục bộ, khó nắm bắt ngữ cảnh toàn cục.
- **Vision Transformer (ViT):** Chi phí tính toán quá lớn khi xử lý chuỗi token dài từ ảnh 3D MRI.

👉 **Giải pháp:** Đồ án này đề xuất **MedMamba-Fusion**, ứng dụng kiến trúc **State Space Models (Mamba)** với độ phức tạp tuyến tính $O(N)$, kết hợp đa phương thức (ảnh 3D DCE-MRI + dữ liệu lâm sàng) trên bộ dữ liệu chuẩn hóa quy mô lớn **MAMA-MIA**.

---

## 📊 Dữ liệu (Dataset)

Nghiên cứu sử dụng bộ dữ liệu **MAMA-MIA (2025)** - tập dữ liệu DCE-MRI ung thư vú đa trung tâm lớn nhất hiện nay.

* **Quy mô:** 1.506 ca bệnh từ 4 trung tâm lớn (DUKE, ISPY1, NACT, ISPY2).
* **Đặc điểm:** Bao gồm ảnh DCE-MRI 3D, 21 biến số lâm sàng (Tuổi, HR, HER2, chủng tộc...) và **nhãn phân đoạn chuyên gia (expert segmentations)**

---

## 🚀 Phương pháp (Methodology)

### 1. Tiền xử lý (Preprocessing)
* **Hình ảnh (Image):**
    * Tạo *Subtraction Map* ($I_{input} = I_{post} - I_{pre}$) để làm nổi bật khối u.
    * Cắt vùng quan tâm (ROI) 3D dựa trên nhãn chuyên gia + mở rộng biên (margin).
    * Chuẩn hóa Z-score để giảm sai biệt giữa các máy quét.
* **Lâm sàng (Clinical):**
    * Mã hóa One-hot cho biến phân loại.
    * Chuẩn hóa [0, 1] cho biến liên tục.

### 2. Kiến trúc MedMamba-Fusion
Hệ thống vận hành theo quy trình hai giai đoạn (Two-stage pipeline):

1.  **Giai đoạn 1 (Detection):** Sử dụng mạng 3D U-Net nhẹ để tự động phát hiện và cắt ROI.
2.  **Giai đoạn 2 (Classification):**
    * **Nhánh Hình ảnh (3D Vision Mamba):** Trích xuất đặc trưng không gian từ ảnh 3D với độ phức tạp tuyến tính.
    * **Nhánh Lâm sàng (Lightweight MLP):** Trích xuất đặc trưng từ dữ liệu bảng.
    * **Fusion Module:** Sử dụng cơ chế **Gated Fusion** để tự động trọng số hóa mức độ quan trọng giữa hình ảnh và lâm sàng trước khi đưa ra dự đoán cuối cùng.

---
