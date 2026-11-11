# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603329
- Ngày / CVAT local: 11/11/2026, CVAT local
- Công cụ đã dùng: Brush, Polygon, OpenCV Intelligent Scissors

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

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

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: ảnh `000000181542.jpg`, người lái xe máy đội mũ bảo hiểm ở giữa-phải ảnh, phía trước xe bán tải (khoảng x 428–562, y 121–316); vẽ bằng Polygon.
- Class và quy tắc tôi dùng để chọn biên: `person`. Vẽ sát phần nhìn thấy của người (mũ bảo hiểm, thân, chân), không đoán phần bị người và xe phía trước che; dừng mask ở ranh người–xe vì chiếc xe máy là một instance `motorcycle` riêng.
- Nếu dùng gợi ý sau đó: có dùng OpenCV Intelligent Scissors cho một số object sau object đầu tiên; vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: …
- Nếu không dùng gợi ý: không áp dụng (có dùng gợi ý, xem dòng trên).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `easy_semantic`, ảnh `817bca71-00000000.jpg` (dải vỉa hè hai bên đường, khoảng y 456–638) và `81ae7cbb-6bc63a4a.jpg` (vỉa hè bên phải, x 720–1279, y 431–620).
- Lỗi thuộc loại: phủ vùng / thiếu lớp — cả ba ảnh không có pixel `sidewalk` nào.
- Bằng chứng tôi nhìn thấy: trong ZIP đã export, mask của ba ảnh chỉ có `road`, `building`, `vegetation`, `sky`; phần vỉa hè gần như để trống (91–100% chưa tô, một ít bị tô thành `road`). Mỗi ảnh còn 12–19% pixel chưa tô. Khi tự chạy scorer: IoU `sidewalk` = 0.000, coverage 88.7%.
- Quy tắc và hành động sửa: ranh `road`–`sidewalk` xác định theo chức năng và bó vỉa, không theo màu; cần tô `sidewalk` cho dải vỉa hè ở hai ảnh trên và tô nốt vùng `vegetation`/`building` còn trống. Ngày 12/11 đã sửa trong CVAT với hỗ trợ của trợ lý AI Claude (Claude vẽ polygon dựa trên ảnh gốc, không dùng file đáp án): Easy thêm `sidewalk` ở hai ảnh trên và tô thêm `vegetation`/`building` cho vùng trống ở cả ba ảnh; Medium đổi xe bán tải ở `000000181542.jpg` từ `car` sang `truck` và tách nhóm người ở `000000458325.jpg` thành 8 mask `person` riêng; Hard thêm `vegetation`, `building`, `sidewalk` cho vùng trống và vẽ lại 3 `traffic light` ở `000000350023.jpg`. Tôi sẽ tự rà lại các polygon này.
- Sau sửa đã Save và export lại chưa? Đã Save trong CVAT; chưa export lại — ZIP trong `submissions/` vẫn là bản export ngày 11/11, trước khi sửa.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): tự chạy `scoring/score.py` sau khi nhận ground truth ba tier — `easy_semantic` mIoU 0.581 (8.1/20; road 0.975, sidewalk 0.000, building 0.604, vegetation 0.472, sky 0.854); `medium_instance` 0.513 (8.0/32; TP 48, FP 15, FN 23); `hard_panoptic` PQ 0.391 (12.7/30); tổng 28.8/82. Đây là điểm trước khi sửa, chưa có điểm sau sửa. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 — Medium `000000181542.jpg`, xe bán tải ở giữa ảnh (x 273–470, y 114–244) | `car` hay `truck` | Xe có thùng hở phía sau; tôi đã gán `car`. Sau khi tự chấm, reference ghi vật này là `truck`. | Đã gán `car`; cần đổi thành `truck`. Hỏi coach: xe bán tải luôn tính là `truck`? |
| 2 — Medium `000000458325.jpg`, nhóm người nhỏ ở xa (x 203–297, y 176–242) | Một mask chung cho cả nhóm, hay mỗi người một mask | Quy tắc: hai vật cùng lớp sát nhau vẫn là hai instance. Tôi vẽ một mask gộp cả nhóm; ảnh này tôi nộp 2 `person`, reference có 12. | Cần tách từng người thấy rõ. Hỏi coach: người quá nhỏ (dưới ~10 px) có bắt buộc vẽ không? |
| 3 — Hard `000000350023.jpg`, đèn giao thông (quanh x 345–414, y 353–364) | Chỉ tô bóng đèn đang sáng, hay tô cả hộp đèn | Tôi tô 2 vùng rất nhỏ (18–29 px); reference có 3 đèn, mỗi đèn 73–186 px, nên không mask nào khớp (PQ `traffic light` = 0). | Nên tô cả hộp đèn nhìn thấy và thêm đèn thứ ba. Hỏi coach: cột đèn có thuộc `traffic light` không? |
