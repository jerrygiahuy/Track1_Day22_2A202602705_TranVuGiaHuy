# Procurement Q&A — AI Notes

**Chủ sở hữu:** Trần Vũ Gia Huy · **Mã học viên:** 2A202602705  
**Bản lập kế hoạch:** 12/10/2026 · **Hạn hoàn thiện bằng chứng cho pilot:** 23/10/2026

**Trạng thái:** câu trả lời và chính sách dự kiến cho sản phẩm cá nhân/pilot nhỏ. Chưa có triển khai API production, kết quả eval, hợp đồng khách hàng, chứng nhận bảo mật hoặc kiểm chứng toàn bộ luồng dữ liệu. Đây chưa phải gói cam kết procurement cho triển khai doanh nghiệp.

## 1. AI này có hallucinate không? Nếu nó nói sai thì sao?

**Trả lời:** Có khả năng sai, kể cả khi output có citation. AI Notes hỗ trợ learner tổ chức dấu vết đã chọn; không cam kết ghi chú tự động đúng hoàn toàn. Learner chọn ≥2 dấu vết hợp lệ và ≥1 nguồn, xem nguồn và duyệt tất cả section bắt buộc trước khi lưu. Không tính API output là job hoàn thành. Note chỉ được ghi nhận khi server lưu thành công, trả `note_id`, đáp ứng điều kiện chất lượng và đã chống trùng.

**Kiểm soát dự kiến:** chỉ cho phép citation về nguồn trong phạm vi đã chọn; kiểm schema/ID trước review; hiển thị uncertainty và xung đột; yêu cầu bổ sung khi thiếu căn cứ; có đường sửa, tự viết và quay lại bài. AI không được tự lưu, xuất bản hoặc quyết định thay learner. Dừng pilot khi gặp citation bịa, lộ dữ liệu hoặc lỗi nghiêm trọng làm đảo nghĩa; giữ error record đã ẩn dữ liệu, sửa nguyên nhân và chạy lại kiểm thử trước mở lại.

**Nếu note sai:** learner có thể sửa/xóa và báo lỗi. Trần Vũ Gia Huy là đầu mối tiếp nhận, kiểm tra nguồn, sửa lỗi sản phẩm và thông báo cho người bị ảnh hưởng trong pilot. Cơ chế dự kiến trả lại credit cho qualifying note có lỗi hệ thống xác nhận; điều kiện xử lý phải được viết rõ trước thu tiền. Chưa có SLA bồi thường, bảo hiểm, accuracy guarantee hay cam kết phản hồi 24/7.

| Bằng chứng cần có | Hạn | Chủ sở hữu |
| --- | --- | --- |
| 50-case offline eval: grounded correctness, citation, schema, refusal, injection; gồm failure examples và n/N | 26/10/2026 | Trần Vũ Gia Huy |
| Video/log QA chứng minh không lưu khi review thiếu section, citation ngoài scope hoặc schema lỗi; test idempotency | 23/10/2026 | Trần Vũ Gia Huy |
| Hướng dẫn báo lỗi, pause/restart pilot, sửa/xóa note và quy tắc trả credit được hiển thị trước mua | 23/10/2026 | Trần Vũ Gia Huy |

HITL là **Biến thể A**: learner tự sửa/duyệt, thời gian learner không nằm trong COGS của nhà cung cấp. Nhà cung cấp dự toán **10% attempt × 6 phút × $8/giờ**, gồm **3 phút QA + 3 phút phân bổ hỗ trợ workflow tuyến đầu**. Hỗ trợ giúp learner dùng flow, không viết/sửa nội dung note thay learner. Ở quy mô giả định 70 tài khoản trả tiền trong workbook, ngân sách tháng là **$48 QA + $48 support**; support tương đương khoảng **5,14 phút/tài khoản**, không phải cam kết thời gian đáp ứng. Tỷ lệ/thời gian đều là giả định, phải đo pilot và tính thêm chi phí nếu phát sinh. QA 10% không thay thế learner review 100% section bắt buộc và không phải bảo đảm mỗi note được nhân viên kiểm.

## 2. Dữ liệu của tôi có bị dùng để train model không?

**Trả lời:** Hiện chưa thể đưa ra bảo đảm đã được kiểm chứng cho hệ thống AI Notes. Thiết kế dự kiến dùng Anthropic API thương mại, chỉ gửi các dấu vết và đoạn nguồn learner đã chọn cùng ngữ cảnh tối thiểu. Trước khi dùng dữ liệu pilot thật, Trần Vũ Gia Huy phải kiểm tra chính sách/hợp đồng **đúng sản phẩm API, tài khoản và cấu hình thực tế**, gồm điều kiện training/opt-in, retention, ngoại lệ xử lý abuse và subprocessors. Không suy chính sách của ứng dụng chat cá nhân sang API thương mại.

**Nguồn nhà cung cấp đã tra cứu ngày 12/10/2026:** tài liệu công khai nói mặc định không dùng input/output thương mại để train; feedback hoặc cho phép chia sẻ có thể tạo ngoại lệ. Đây là chính sách nhà cung cấp, chưa chứng minh cấu hình AI Notes. [Anthropic — Is my data used for model training?](https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training)

API có retention tiêu chuẩn đến 30 ngày, kèm ngoại lệ và điều kiện riêng; không được hiểu thành zero retention. Cần đối chiếu hợp đồng thực tế trước dữ liệu thật. [Anthropic — Data retention](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data), [Commercial Terms](https://www.anthropic.com/legal/commercial-terms).

**Chính sách dự kiến của AI Notes:** không dùng note hoặc nguồn riêng của learner để train/fine-tune model của mình; không bật chia sẻ dữ liệu hoặc opt-in training. Pilot chỉ nhận nội dung được quyền dùng, đã loại PII, không nhận dữ liệu nhạy cảm/doanh nghiệp chưa được phép. Phải thông báo việc gửi nội dung đã chọn đến provider trước khi learner đồng ý tham gia. Tránh ghi raw prompt/output vào analytics; chỉ lưu ID, trạng thái, token/cost và lỗi đã ẩn nội dung. QA có xem nội dung phải được learner biết và chấp thuận trong pilot.

Những quy tắc trên là thiết kế cần triển khai và kiểm chứng, chưa phải tuyên bố về cấu hình hiện tại. Chưa xác nhận zero retention, lưu trữ riêng tại Việt Nam, mã hóa theo một chuẩn cụ thể, SOC 2 hoặc khả năng đáp ứng chính sách nội bộ của một doanh nghiệp.

| Bằng chứng cần có trước dữ liệu pilot thật | Hạn | Chủ sở hữu |
| --- | --- | --- |
| Bản chụp/ghi nhận ngày kiểm tra điều khoản Anthropic API thương mại; account settings về data-sharing; tài liệu retention và ngoại lệ áp dụng | 23/10/2026 | Trần Vũ Gia Huy |
| Sơ đồ data flow: browser → backend → provider → lưu note/log; danh sách dữ liệu, nơi lưu và quyền truy cập | 23/10/2026 | Trần Vũ Gia Huy |
| Chính sách lưu/xóa cho note, raw data, metadata và backup; test xóa xuyên toàn luồng; thông báo consent pilot | 23/10/2026 | Trần Vũ Gia Huy |
| DPA/điều khoản bảo mật đúng nhu cầu khách, nếu khách yêu cầu trước triển khai doanh nghiệp | Trước ký hợp đồng/live data doanh nghiệp; chưa có hợp đồng | Trần Vũ Gia Huy |

Nếu điều khoản/cấu hình chưa được xác nhận ngày 23/10, chỉ dùng fixture tổng hợp để kiểm thử và hoãn pilot dữ liệu thật. Không gửi dữ liệu để “thử rồi kiểm tra chính sách sau”.

## 3. Nếu startup đóng cửa thì dữ liệu của tôi ở đâu?

**Trả lời:** Thiết kế dự kiến cho phép learner tải note của mình dưới dạng Markdown/JSON, gồm nội dung đã duyệt, nguồn, evidence references, `note_id`, phiên bản và thời điểm lưu. Bản tải xuống nằm trên thiết bị của learner; liên kết nguồn ngoài vẫn phụ thuộc chủ nguồn. Khả năng export này **chưa được xây dựng/kiểm chứng** cho backend production và phải xong trước pilot có lưu dữ liệu thật.

Kế hoạch dừng dịch vụ: cung cấp thông báo và khoảng thời gian tải dữ liệu trước khi ngừng nếu điều kiện vận hành cho phép; mốc dự kiến **30 ngày** phải được ghi trong điều khoản, không hứa bảo đảm khi đóng cửa đột ngột. Khuyến khích export ngay khi note quan trọng được lưu. Sau cửa sổ đã công bố, xóa dữ liệu theo chính sách retention/backup đã được kiểm chứng. Không hứa cung cấp dịch vụ vĩnh viễn, chuyển dữ liệu cho bên thứ ba hay giữ URL cloud hoạt động sau đóng cửa.

| Bằng chứng cần có | Hạn | Chủ sở hữu |
| --- | --- | --- |
| File export mẫu và test mở độc lập: nội dung, Unicode tiếng Việt, citation, note/version ID đúng | 23/10/2026 | Trần Vũ Gia Huy |
| Test export toàn bộ tài khoản và đối chiếu số note, không lẫn note của tài khoản khác | 23/10/2026 | Trần Vũ Gia Huy |
| Runbook dừng dịch vụ, thông báo/export window, địa chỉ đầu mối và quy trình xóa cả backup | 23/10/2026 | Trần Vũ Gia Huy |

## 4. Kết luận readiness

Hiện có prototype và định nghĩa metric từ Day18/20; chưa có các bằng chứng backend/data-flow cần thiết. Procurement Q&A hoàn thiện bằng chứng dự kiến **23/10/2026**, eval **26/10/2026**. Pilot chỉ bắt đầu **27/10/2026** nếu cả hai gate đạt; nếu không, cập nhật lịch và lý do. Trần Vũ Gia Huy chịu trách nhiệm toàn bộ các mục; chưa có tổ chức nào duyệt procurement hoặc đồng ý mua.
