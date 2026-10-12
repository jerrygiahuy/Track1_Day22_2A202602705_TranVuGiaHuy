# Pilot Plan — AI Notes, Option B

**Chủ sở hữu:** Trần Vũ Gia Huy · **Mã học viên:** 2A202602705  
**Ngày lập:** 12/10/2026 · **Pilot dự kiến:** 27/10–09/11/2026 · **Báo cáo:** 12/11/2026  
**Trạng thái:** chưa tuyển đủ người, chưa chạy pilot, chưa đo API quality, WTP, CAC hoặc completion thực tế.

## 1. Câu hỏi cần học

Learner có hoàn thành ghi chú đã duyệt với chi phí và công sức chấp nhận được không? Họ có tìm/dùng lại note, và có thực sự muốn trả **$8/tháng cho 12 qualifying note đã lưu** không? Đây là giả thuyết pricing cho phần mềm hỗ trợ cá nhân, không bán outcome học tập hay ghi chú tự động đúng tuyệt đối.

Kế thừa [Day18](https://github.com/jerrygiahuy/Track1_Day18_2A202602705_TranVuGiaHuy/blob/main/group-feedback-synthesis.md) và [Day20](https://github.com/jerrygiahuy/Track1_Day20_2A202602705_TranVuGiaHuy/blob/main/metrics-pack.md): hai tester chọn B là preference; 41/41 dry-run là QA kỹ thuật. Cả hai không thay thế pilot, accuracy, willingness to pay hoặc chứng minh retention.

## 2. Phạm vi, tuyển người và điều kiện bắt đầu

Trần Vũ Gia Huy dự kiến tuyển **10 learner** đang học/research một chủ đề và có dấu vết học tập thực, qua một flow PLG tự phục vụ. Mời thử trong cộng đồng học tập thuộc cùng kênh PLG; chưa giả định một đối tác hay thỏa thuận phân phối đã tồn tại. Mỗi người thử tối đa **12 attempt**, tổng tối đa **120 attempt** trong hai tuần. Đây là giới hạn thí nghiệm, nhỏ hơn cap gói bán dự kiến; người rút lui hoặc ít dùng được báo đúng, không điền attempt giả để đủ mẫu.

Chỉ dùng nội dung learner có quyền chia sẻ, đã bỏ PII và thông tin nhạy cảm. Không yêu cầu đăng nhập nền tảng khác hoặc cấp quyền toàn bộ kho tài liệu. Chưa gửi lời mời hay thông tin cho người ngoài trong khuôn khổ bài lab.

| Điều kiện go/no-go trước pilot | Hạn | Chủ sở hữu |
| --- | --- | --- |
| 3 câu procurement có bằng chứng: hallucination/review, data/training, export/đóng cửa | 23/10/2026 | Trần Vũ Gia Huy |
| Backend có save thật, review gate, note ownership, idempotency, export/xóa và logging tối thiểu; QA trên fixture | 23/10/2026 | Trần Vũ Gia Huy |
| 50-case held-out eval đạt gate; model/prompt/schema đóng băng và có failure log | 26/10/2026 | Trần Vũ Gia Huy |
| Consent, quota thông báo rõ, đường báo lỗi/thoát/tự viết; kiểm tra hợp đồng và cấu hình provider | 26/10/2026 | Trần Vũ Gia Huy |

Nếu không đạt gate, hoãn pilot dữ liệu thật và tiếp tục kiểm thử bằng fixture. Ngày bắt đầu là kế hoạch phụ thuộc gate, không phải tuyên bố đã triển khai.

## 3. Pain moment và trải nghiệm cụ thể

**Pain moment giả thuyết:** khoảng **22:30**, sau buổi học, learner đang gom **highlight và liên kết nguồn trong Chrome** để tổ chức lại thành ghi chú cá nhân. Bề mặt đề xuất là mục **“Tổ chức ghi chú” trong Chrome context menu**, mở **Chrome side panel** để learner chọn dấu vết và xem nguồn ngay tại nội dung đang đọc. Extension/context menu/side panel chưa được triển khai; pilot dự kiến mô phỏng bề mặt này với nội dung được phép dùng. Không có tích hợp production với Vlearn.

Flow: learner chọn ≥2 evidence và ≥1 nguồn → AI tổ chức/đề nghị làm rõ nếu cần → learner duyệt/sửa mọi section bắt buộc → chủ động lưu → backend trả `note_id` → mở lại nguồn hoặc export. Không tự tạo/lưu note thay learner. Nếu API lỗi hoặc nguồn không đủ, giữ dấu vết và cho tự viết/thoát.

Gói thương mại đề xuất: **$8/tháng, 12 qualifying note đã lưu; cap 22 normal attempt, tối đa 2 LLM call/attempt, 2.400 input và 800 output token/call**. Retry kỹ thuật có quota riêng **tối đa 3 whole-job retry/tài khoản/tháng**; tính cả normal call và retry, trần là **50 LLM call/tài khoản/tháng**. Retry được log gắn với attempt gốc, có chi phí riêng trong workbook, không sinh thêm completed job hay credit đã lưu; tỷ lệ kỳ vọng 10% whole job là giả định. Hết quota thì dừng, cho xem quota hoặc mua trước một bundle cùng điều kiện; **không tự tính overage**. Giá/quota này là giả thuyết, chưa có khách trả tiền. Pilot giới hạn 12 normal attempt/người để học có kiểm soát, không mô tả đây là quota thương mại đã được kiểm chứng.

## 4. Đo completed job và chi phí

| Chỉ số | Hợp đồng đo |
| --- | --- |
| Attempt hợp lệ | Backend nhận job với ≥2 evidence hợp lệ và ≥1 source; gồm attempt API lỗi, schema lỗi hoặc bị bỏ dở sau khi bắt đầu |
| Completed job | Note đáp ứng điều kiện chất lượng, learner duyệt mọi section bắt buộc, bấm lưu, server lưu thành công và trả `note_id`; đếm note riêng biệt, loại save/event trùng và chỉnh phiên bản cùng note |
| R với user review | Completed job / attempt hợp lệ; báo n/N toàn cohort và theo learner; **70% là giả định tài chính**, không gọi autonomous containment |
| Cost/Job — COGS | (API + Infra + QA/support nhà cung cấp + Retry) / completed job; không chia cho tổng attempt |
| Chi phí sau overhead | (COGS + fixed overhead R&D/admin) / completed job; báo riêng để đủ 5 nhóm chi phí và không trộn gross margin với margin sau overhead |
| HITL | Biến thể A: learner tự sửa/duyệt. Đo riêng phút review của learner; vận hành nhà cung cấp dự toán 10% attempt × 6 phút × $8/giờ, gồm 3 phút QA + 3 phút pooled first-line support; hỗ trợ workflow, không viết note thay learner |
| Quality | Lỗi sau save, citation sai, phủ định/đánh đổi bị mất, major rework >50% trong 24 giờ; không đánh đồng “đã bấm review” với correctness |
| Activation | Qualifying note đầu tiên trong 7 ngày từ session học đủ điều kiện đầu tiên |
| Useful retrieval | Note cũ mở từ search/library rồi edit, copy hoặc mở nguồn trong 10 phút; loại mở ngay sau save |
| WTP | Số người được xem offer, số nói muốn mua, số thật sự chủ động trả tiền và hoàn tất giao dịch; báo riêng từng loại |
| CAC | Chi phí PLG thực tế (công cụ + thời gian founder theo đơn giá công bố) / khách mới thực sự trả tiền; nếu chưa có paid user, ghi chưa tính được CAC, không ghi $0 |

Founder QA khoảng **12/120 attempt** nếu pilot đạt đủ mẫu. Với 3 phút QA/ca và 3 phút phân bổ support/ca, $8/giờ, dự toán pilot là **$4,80 QA + $4,80 support = $9,60**; chưa là chi phí thực tế. Ở quy mô tháng giả định 70 tài khoản trả tiền trong workbook, ngân sách là **$48 QA + $48 support**; support tương đương **5,14 phút/tài khoản/tháng**, không phải SLA. Support là pool hỗ trợ workflow, không bảo đảm mỗi note được nhân viên hỗ trợ hoặc viết lại. Nếu có lỗi cần review hoặc hỗ trợ thêm, ghi đủ phút tăng thêm vào COGS; không giữ cố định 10% chỉ để đẹp mô hình. Chi phí tạo bộ eval, tuyển và phỏng vấn một lần được báo riêng với vận hành để không tính trùng.

Mốc tài chính để đối chiếu, toàn bộ còn là giả định: giá quy đổi khi dùng đủ gói **$8/12 = $0,666667/note**; base **1.200 normal attempt × R70% = 840 completed job/tháng**, COGS **$166,896/tháng**, **Cost/Job $0,198686**, **gross margin 70,20%**. Fixed overhead **$84/tháng = 10,5 giờ R&D/admin × $8/giờ** nâng chi phí sau overhead lên **$0,298686/job**, cho **margin sau overhead 55,20%**. Overhead $84 là khoản cố định, không mặc định bằng 15% doanh thu ở quy mô khác.

Stress test cap dự toán **$3,07 COGS/tài khoản**, GM **61,625%**, phụ thuộc ngân sách infra **$0,045/normal attempt** đã gồm reserve I/O cho retry. Nếu cần trả thêm infra cho cả 3 retry ở đơn giá $0,045, COGS tăng $0,135 và GM cap giảm còn **59,9375%**. Pilot phải đo retry I/O thực tế; không coi reserve này là chi phí đã kiểm chứng hoặc hứa mọi tài khoản luôn đạt GM60%.

Pilot hai tuần không đủ chứng minh 30-day useful retrieval hoặc retention ngày 8–21 cho mọi learner. Báo cáo ngày 12/11 phải nêu cohort chưa trưởng thành; tiếp tục theo dõi retrieval 30 ngày và retention phù hợp trong tháng 2 của kế hoạch 90 ngày. Không suy lời nói “hữu ích” thành return event.

## 5. Lịch thực hiện và quyết định

| Ngày | Hoạt động và đầu ra định lượng | Chủ sở hữu |
| --- | --- | --- |
| 12–19/10/2026 | Chốt 50 fixture held-out, schema/event contract và script phỏng vấn pricing | Trần Vũ Gia Huy |
| 20–23/10/2026 | Hoàn thiện 3 câu procurement cùng bằng chứng; kiểm backend/export/xóa và consent | Trần Vũ Gia Huy |
| 24–26/10/2026 | Chạy 50 case offline, báo n/N và lỗi; quyết định có đủ gate mở pilot không | Trần Vũ Gia Huy |
| 27/10–09/11/2026 | 10 learner dự kiến, ≤120 normal attempt; log token/cost/review/save/retrieval/retry; QA mẫu 10% và pooled support | Trần Vũ Gia Huy |
| 10–12/11/2026 | Tổng hợp funnel, R, Cost/Job, GM, lỗi và WTP; report có dữ liệu ẩn danh và giới hạn | Trần Vũ Gia Huy |

Đây là nhánh học của kế hoạch 90 ngày **12/10/2026–09/01/2027**; chưa phải scale tháng đầu. Ngày 12/11 so R thực tế với giả định 70% và ngưỡng GM60% trong workbook, đọc cùng quality/rework và effort của learner. Không có paid user thì chưa kết luận giá đã validated hoặc CAC tốt. Phải có bằng chứng giao dịch thật và cohort trưởng thành trước quyết định mở rộng.

**Dừng sớm:** citation bịa nghiêm trọng, lộ dữ liệu, bỏ được review gate hoặc tính quota sai. Trần Vũ Gia Huy khóa nhánh lỗi, thông báo người bị ảnh hưởng, bảo toàn bản note của learner, xác minh sửa và chạy lại kiểm thử trước mở lại. Không dùng phản hồi thuận lợi để bù một lỗi nghiêm trọng.

## 6. Mẫu báo cáo cần điền sau pilot

| Mục | Giá trị hiện tại |
| --- | --- |
| Người mời / consent / activated / người rút lui | Chưa có dữ liệu |
| Attempt hợp lệ / completed job riêng biệt / R | Chưa có dữ liệu |
| API / Infra / QA-support / Retry / fixed Overhead; Cost/Job COGS và chi phí sau overhead | Chưa có dữ liệu |
| Token/call, whole-job retry rate, QA/support phút thực tế, learner review phút thực tế | Chưa có dữ liệu |
| Grounded/citation errors; major rework; sự cố phải dừng | Chưa có dữ liệu |
| Số người nhận offer / nói muốn mua / thực sự trả tiền | Chưa có dữ liệu |
| Chi phí acquisition thực tế / paid users / CAC | Chưa có dữ liệu |
| Retrieval, retention cohort đủ ngày và cohort chưa đủ ngày | Chưa có dữ liệu |
| Quyết định tiếp tục / sửa / dừng và lý do | Chờ báo cáo ngày 12/11/2026 |
