# Day 22 AI Notes Monetization và GTM

**Trần Vũ Gia Huy · 2A202602705 · Track 1 · 12/10/2026**

Sản phẩm tiếp nối Day 18–20 là **AI Notes — Personal Learning Notes, Option B**. Người học chọn dấu vết, AI tổ chức, người học duyệt từng section và lưu note có nguồn. Bài này đề xuất **Hybrid $8/tháng, 12 qualifying note**, phân phối **PLG** trong 90 ngày đầu.

## File nộp bài

- [Excel monetization model](TranVuGiaHuy_Day22_model.xlsx): giữ 5 tab làm việc cùng 2 tab hướng dẫn/tham chiếu của mẫu.
- [Monetization One-Pager PDF](TranVuGiaHuy_Day22_onepager.pdf): đúng một trang, ba khối Pricing, GTM và Evidence; số liệu có địa chỉ ô Excel.
- [Nhật ký phản biện](critique-log.md): hai prompt §4.7.1 và §4.7.3, phép tính và quyết định accept/reject/partial.
- Evidence Pack: [Eval Plan](evidence/eval-plan.md), [Procurement Q&A](evidence/procurement-qa.md), [Pilot Plan](evidence/pilot-plan.md).

Repo đích đã cấu hình: [Track1_Day22_2A202602705_TranVuGiaHuy](https://github.com/jerrygiahuy/Track1_Day22_2A202602705_TranVuGiaHuy). Các file được chuẩn bị cục bộ; chưa commit, push hoặc nộp Vlearn.

## Các quyết định và số chính

| Chỉ số | Kết quả theo giả định | Ô Excel |
| --- | ---: | --- |
| Job | Note có ≥2 evidence hợp lệ, ≥1 nguồn, duyệt mọi section và server lưu `note_id` duy nhất | `1_Cost_Job!B5` |
| Tỷ lệ hoàn thành R có learner review | 70%, **chưa đo** | `1_Cost_Job!B10` |
| Job thử / hoàn thành mỗi tháng | 1.200 / 840 | `1_Cost_Job!B9:B11` |
| Cost/Job gồm API, infra, retry, QA và support | $0,198686 | `1_Cost_Job!B66` |
| Cost/Job sau phân bổ overhead | $0,298686 | `1_Cost_Job!B67` |
| Giá sàn 3× COGS | $0,596057/note | `2_Pricing!B7` |
| Giá quy đổi khi dùng đủ gói | $8 / 12 = $0,666667/note | `2_Pricing!B19`, `B56:B57` |
| Gross margin trực tiếp | 70,1971% | `2_Pricing!B21` |
| Margin sau overhead cố định $84/tháng | 55,1971% | `2_Pricing!B61` |
| R tối thiểu để GM ≥60% | 52,155% | `2_Pricing!B33` |
| GM dưới 50% khi R thấp hơn | 41,724% | `2_Pricing!B62` |
| R cần đạt nếu muốn margin sau overhead ≥60% | 78,405% | `2_Pricing!B63` |
| CAC tối đa với payback prosumer 3 tháng | $16,8473 | `4_Channel_Fit!B9` |
| CAC PLG mục tiêu, chưa đo | $12 | `4_Channel_Fit!B50` |
| Sales phản chứng | 5,21 deal/AE/ngày; CAC $1.500, cao 89,03× ngân sách | `4_Channel_Fit!B16`, `B22:B23` |

**R không phải autonomous containment.** Đây là tỷ lệ note hoàn thành sau khi người học duyệt, không chứng minh AI tự làm xong hoặc giúp học tốt hơn. Actual autonomous containment chưa đo. Attribution 1/10 và Autonomy 0/10 dẫn đến Hybrid; không bán outcome học tập.

Gói tháng bao gồm lưu nguồn, export và 12 note, không cộng thêm đơn giá note từ note đầu. Giới hạn đề xuất: 22 normal attempts, 2 calls/attempt; tối đa 3 retry toàn job bổ sung, tổng trần 50 calls/account/tháng. Mỗi call giới hạn 2.400 input và 800 output token. Gói thêm mua trước, không tự thu overage. Các giới hạn này chưa triển khai trong prototype.

## Chi phí và ngưỡng rủi ro

LLM Haiku 4.5 dùng giá list $1 input/$5 output mỗi 1M token, kiểm tra 12/10/2026. Không giả định cache hit, không dùng batch cho luồng tương tác. Speech và telephony bằng 0 vì scope chỉ có văn bản. Infra $0,045/normal attempt gồm dự phòng retry I/O, observability/eval và phí thanh toán. Retry kỳ vọng 10% chi phí LLM.

HITL A: người học tự sửa note. Provider vẫn trả QA và hỗ trợ workflow: 10% attempts × (3 phút QA + 3 phút support phân bổ) × $8/giờ. Base có $48 QA và $48 support mỗi tháng. Công này không bao gồm dịch vụ viết lại note cho người học.

Ở cap 22 attempts và 3 retry, GM trực tiếp còn 61,625%; nếu retry infra vượt reserve, còn 59,9375%. Nếu R thật chỉ 35%, GM còn 40,39%. Nếu QA/support gấp đôi, GM còn 53,05%. Đây là rủi ro cần đo trước khi scale.

Giá neo theo giả thuyết tiết kiệm ròng 20 phút/note × $8/giờ × 12 note = $32/tháng; lấy 25% là $8. Chưa đo tiết kiệm thời gian và chưa có WTP. Không giả định thay thế vị trí nhân sự hưởng lương. Tỷ giá quy đổi dùng VCB USD bán 26.070 VND, XML cập nhật 09:20 ngày 12/10/2026.

## Kế hoạch và phần còn cần bằng chứng thật

Pain moment giả thuyết: 22:30 sau lớp online, learner gom highlight và link trong Chrome. Điểm nhúng đề xuất là context menu “Lưu dấu vết” và Chrome side panel; chưa triển khai, chưa có đối tác Vlearn.

Tháng 1 học từ 10 learner, eval 50 ca held-out và pilot ≤120 attempts. Tháng 2–3 mục tiêu 70 paid, CAC ≤$12, R ≥70%, retention 28 ngày ≥40%. Tháng 4+ chỉ mở rộng khi hai cohort liên tiếp đạt gate. Trần Vũ Gia Huy phụ trách toàn bộ. Mục tiêu không phải kết quả đã đạt.

| Tài sản / kiểm tra | Trạng thái hiện tại | Deadline dự kiến |
| --- | --- | --- |
| Eval Results | Có kế hoạch; chưa có kết quả API thật | 26/10/2026 |
| Procurement Q&A | Có bản nháp, nguồn chính sách provider; chưa chứng minh triển khai | 23/10/2026 |
| Pilot Report | Có kế hoạch 27/10–09/11; chưa tuyển/chạy/đo | 12/11/2026 |
| Bài test người lạ | Chưa chạy với người thật; chưa đo số câu hỏi | 15/10/2026 |

Bằng chứng cũ chỉ là 2/2 tester thích Option B và 41/41 dry-run kỹ thuật. Không dùng hai kết quả này làm accuracy, containment, WTP hoặc retention. Ngày bắt đầu pilot phụ thuộc việc triển khai và qua gate, không phải cam kết đã có tích hợp.

## Kiểm tra và điều chỉnh mẫu

Đã đối chiếu chi phí, mẫu số completed jobs, sensitivity, ngưỡng R, cap/retry và channel affordability bằng phép tính độc lập; kiểm tra bố cục các tab và PDF một trang. Các ô chưa có bằng chứng thật ghi rõ trạng thái cùng owner/deadline.

Mẫu có tiêu đề Day 28 trong README và dùng containment tự chủ cho case ticket. Bản làm đổi về Day 22 và đặt nhãn R hoàn thành có người. Bổ sung Prosumer/payback 3 tháng, liên kết giá gói và anchor bằng công thức, bảo vệ trường hợp không có completed job, và thêm stress/support/source blocks. Các công thức sensitivity và tỷ trọng chi phí được giữ quan hệ toán học, làm mới để tránh cache kết quả mẫu. Các dòng benchmark cũ được giữ và đánh dấu chưa xác minh; quyết định dùng bảng nguồn hiện hành ở `6_Benchmarks!A45:E70`.

Các tài liệu nguồn đã kiểm tra được ghi cạnh số liệu trong workbook và Evidence Pack. Thay giả định bằng log/pilot thực tế rồi đọc lại toàn bộ kết quả trước khi dùng để chào bán.
