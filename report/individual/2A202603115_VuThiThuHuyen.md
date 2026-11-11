# Báo cáo thực hành Day 3 – Image Segmentation

- Mã học viên theo lớp: **2A202603115**
- Họ và tên: **Vu Thi Thu Huyen**
- Ngày thực hiện: **12/11/2026**
- Công cụ đã dùng: **CVAT Online, Segment Anything 3 (SAM 3) và thao tác kiểm tra, chỉnh sửa mask thủ công.**

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Tôi đã hoàn thành và export đủ 9 file ZIP từ CVAT. Sau khi export, tôi sử dụng `unzip -t` để kiểm tra tính toàn vẹn của các file. Cả 9 file đều không phát hiện lỗi trong dữ liệu nén.

## 2. Một quyết định trước khi dùng gợi ý

- **Ảnh và object Medium đầu tiên tự vẽ:** Ảnh `000000181542.jpg`, đối tượng motorcycle. Đây là đối tượng đầu tiên tôi tự vẽ trước khi sử dụng gợi ý AI.
- **Class và quy tắc chọn biên:** Tôi xác định đối tượng là motorcycle, chỉ lấy phần thân xe và các bộ phận nhìn thấy được của xe; không gộp người điều khiển, mặt đường hoặc vật thể xung quanh vào mask.
- **Công cụ gợi ý được sử dụng:** Sau khi tự vẽ đối tượng đầu tiên, tôi sử dụng Segment Anything 3 (SAM 3) trong CVAT Online để hỗ trợ tạo mask cho các đối tượng tiếp theo.
- **Cách kiểm tra kết quả:** Tôi không chấp nhận tự động tất cả mask được đề xuất. Tôi kiểm tra class, đường biên và phạm vi đối tượng, sau đó giữ lại hoặc chỉnh sửa thủ công khi kết quả chưa phù hợp.

Quyết định của tôi là ưu tiên mask phản ánh đúng đối tượng trong ảnh thay vì chỉ dựa vào kết quả do AI đề xuất.

## 3. Lỗi tìm thấy và đã sửa

Trong quá trình kiểm tra annotation và mask, tôi phát hiện hai trường hợp cần điều chỉnh:

**Trường hợp 1 – CP4 (cp4_curb):**
- Lỗi: Vùng sidewalk (vỉa hè) bị bỏ sót trong lần export trước.
- Hành động sửa: Kiểm tra lại ảnh gốc, bổ sung vùng sidewalk theo ranh giới quan sát được, sau đó Save và export lại task.
- Kết quả: File `cp4_curb.zip` mới đã được export và kiểm tra tính toàn vẹn.

**Trường hợp 2 – CP6 (cp6_coverage):**
- Lỗi: Vùng vegetation (thảm thực vật) bị bỏ sót trong lần export trước.
- Hành động sửa: Rà soát lại ảnh, bổ sung vùng vegetation nhìn thấy được và chỉnh sửa mask trước khi Save, export lại.
- Kết quả: File `cp6_coverage.zip` mới đã được export và kiểm tra tính toàn vẹn.

Tôi đã sử dụng các bản export sau khi chỉnh sửa để chuẩn bị nộp bài.

**Kết quả tự đánh giá:** Chưa có điểm tự đánh giá được xác nhận từ GitHub Actions hoặc script chấm điểm.

## 4. Ba trường hợp chưa chắc chắn hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Building | Một vùng có thể thuộc building hoặc vegetation khi cây che khuất một phần công trình. | Quan sát đặc điểm bề mặt, cấu trúc công trình và phần ranh giới còn nhìn thấy được. | Chỉ gán building cho phần thực sự thuộc công trình; tách riêng vegetation theo ranh giới quan sát được. |
| Occlusion | Đối tượng bị che khuất có thể chỉ được annotation phần nhìn thấy hoặc bị nhầm với đối tượng ở phía trước. | Dựa vào hình dạng và đặc điểm còn quan sát được; không tự suy đoán vùng bị che khuất. | Giữ riêng từng instance và chỉ annotation phần có thể xác định được theo quy tắc bài lab. |
| Thin | Đối tượng mảnh có thể bị bỏ sót hoặc mask quá rộng, làm mất hình dạng thực tế. | Quan sát chiều rộng, hình dạng và đường biên của đối tượng trong ảnh gốc. | Cố gắng giữ đúng hình dạng mảnh, tránh gộp với nền hoặc đối tượng bên cạnh. |