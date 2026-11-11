# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603160
- Ngày / CVAT local: 11/11/2026 / Docker localhost:8080 (CVAT 2.74.1)
- Công cụ đã dùng: CVAT Brush, Polygon, Python CVAT Auto-Labeler (YOLOv8-seg)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Toàn bộ 9/9 task đã được Save đầy đủ trên CVAT và xuất khẩu thành công các file ZIP đúng định dạng quy định (`Segmentation mask 1.1` cho Semantic và `COCO 1.0` cho Instance/Panoptic) đặt tại thư mục `submissions/`.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe máy lớn (`motorcycle`) ở tiền cảnh góc dưới bên phải ảnh (tọa độ khoảng x=404 đến 640, y=295 đến 502).
- Class và quy tắc tôi dùng để chọn biên: Class `motorcycle`. Quy tắc biên: Chỉ gán nhãn cho các pixel thuộc về thân xe, bánh xe, lốp, ống xả và gương chiếu hậu thực sự nhìn thấy được. Dừng mask dứt khoát tại ranh giới tiếp xúc với chân người lái (`person`) ngồi phía trên, mép dưới/phải ảnh và ranh giới mặt đường nhựa; tuyệt đối không vẽ lấn sang quần áo của người lái hay bóng đổ của xe xuống mặt đường.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Khi chạy thử Auto-Labeler (YOLOv8-seg), model tạo đề xuất bao trùm cả phần ống quần/chân người lái vào thân xe máy và phình viền lốp lấn ra bóng đổ mặt đường, đồng thời bỏ sót cuống gương chiếu hậu mảnh phía trên. Hành động: Tôi giữ lại phần thân xe chính từ gợi ý, dùng CVAT Brush chế độ Erase thu nhỏ đầu cọ để gọt bỏ phần chân người lái (chuyển sang tạo mask `person` riêng biệt), xóa vùng lem bóng đổ, và dùng Polygon vẽ bù phần gương chiếu hậu mảnh. Lý do: Giữ đúng nguyên tắc phân tách thực thể độc lập (instance separation) và không gộp người vào phương tiện.
- Nếu không dùng gợi ý: (Đã dùng kết hợp gợi ý YOLOv8-seg và hiệu chỉnh thủ công bằng Brush/Polygon như trên).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, ảnh `000000017627.jpg`, khu vực giữa hai ô tô đỗ sát nhau ở làn giữa (khoảng x=174 đến 375, y=236 đến 303).
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Lỗi gộp-tách (Merge / Separation — hai đối tượng cùng class bị dính chung thành một mask).
- Bằng chứng tôi nhìn thấy: Đề xuất tự động từ model gộp liền hai chiếc ô tô con đỗ kề sát nhau thành một mask duy nhất mang nhãn `car`. Khi phóng to 300%, nhìn thấy rõ khe hở hẹp giữa cản sau của xe phía trước và đầu xe phía sau, cùng viền phản quang đèn xe.
- Quy tắc và hành động sửa: Quy tắc instance segmentation yêu cầu "hai vật cùng class sát nhau vẫn là hai instance riêng biệt". Tôi dùng Brush cắt tách dải nối giữa hai xe tại khe hở, phân chia thành 2 object `car` riêng biệt với ID độc lập, sau đó căn chỉnh lại viền theo đúng phần thân xe nhìn thấy của từng xe.
- Sau sửa đã Save và export lại chưa? Đã Save đầy đủ trong CVAT và export lại file `cp2_slice.zip` (định dạng COCO 1.0) ghi đè vào `submissions/cp2_slice.zip`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Chưa có điểm (môi trường máy local chưa cài `pycocotools` và chờ gói ground truth chính thức từ release repo lớp; script `inspect_submissions.py` chạy nội bộ đã báo `[OK]` cho toàn bộ 9 ZIP, riêng `cp2_slice.zip` đã xác nhận cấu trúc hợp lệ với 11 annotations riêng biệt). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Task `cp4_curb`, ảnh `7d83710e-4697c3b2.jpg`, đoạn mép bó vỉa hạ thấp phẳng ngang mặt đường (drop curb) ở góc phải | Thuộc về `road` (vì phẳng ngang mặt đường xe có thể lăn bánh, màu bê tông xỉn giống nhựa đường) hay `sidewalk` (thuộc vỉa hè cho người đi bộ). | Quy tắc guideline: "Ranh road–sidewalk xác định theo chức năng và bó vỉa, không chỉ theo màu sắc". Đoạn dốc này là lối hạ hè dành cho người đi bộ/xe lăn tiếp cận lòng đường. | Quyết định gán toàn bộ phần hè hạ dốc vào `sidewalk`, lấy rãnh tiếp giáp mặt nhựa đường làm biên; nhờ coach xác nhận chuẩn quy ước khi bó vỉa bị mài mòn phẳng hoàn toàn. |
| 2. Task `cp1_holes`, ảnh `000000144300.jpg`, khu vực nan hoa bánh xe và khe hở khung sườn xe máy (`motorcycle`) ở tiền cảnh | Khoét rỗng từng lỗ nhỏ li ti giữa các nan hoa để lộ nền phía sau (lỗ background) hay phủ kín bánh xe thành một khối mask liên tục. | Tiêu chí `cp1_holes`: "Phân biệt chi tiết bên trong vật và lỗ nền thật, không khoét tùy tiện làm vỡ mask"; các khe nan hoa quá mảnh nếu khoét sẽ làm nát mask và lỗi topology. | Quyết định khoét các khoảng trống lớn xuyên qua khung sườn nhìn rõ mặt đường, nhưng phủ kín vùng nan hoa bánh xe; nhờ coach hướng dẫn ngưỡng diện tích lỗ tối thiểu cần khoét trên CVAT. |
| 3. Task `cp5_occlusion`, ảnh `000000336232.jpg`, chiếc ô tô (`car`) ở hậu cảnh bị cột biển báo che cắt ngang thân thành 2 phần đầu và đuôi rời nhau | Tách thành 2 object `car` riêng vì 2 mảng pixel không dính liền, hay giữ chung 1 ID instance cho cả hai phần nhìn thấy. | Quy tắc task: "Một vật bị che vẫn là một instance; không tách ID vì bị che và không tự vẽ đoán xuyên qua phần bị che". | Quyết định gán cả hai mảng nhìn thấy vào cùng một instance `car` (1 ID, không tạo object mới), để trống phần cột che; xin coach xác nhận định dạng RLE của COCO khi biểu diễn multi-part instance. |
