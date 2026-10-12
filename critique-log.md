# Nhật ký phản biện Day 22

Ngày: **12/10/2026**, múi giờ Asia/Ho_Chi_Minh. Người thực hiện: AI agent trong phiên làm lab, theo hai prompt §4.7.1 và §4.7.3 của tài liệu được cung cấp. Bản ghi này hệ thống hóa các lượt phản biện và quyết định đã thực hiện; **không phải API eval, phỏng vấn khách trả tiền, hoặc bài test người lạ thật**. Người học cần đọc lại và tự bảo vệ các quyết định trước khi nộp.

## 1. §4.7.1 — Cost/Job Stress Test

### English prompt executed

```text
Act as a ruthless CFO and a skeptical infrastructure engineer.
Review the Cost/Job model for an AI product below.
Do NOT rewrite my numbers. Perform a stress test:

1. MISSING COST CATEGORIES
List every cost category I have omitted or underestimated for THIS
specific product type. Focus especially on retry and timeout costs;
human-in-the-loop cost and WHO bears it; observability, logging and
eval running costs; egress, storage and vector DB growth over time.
For each, estimate a realistic value at my stated volume.

2. DENOMINATOR CHECK
Am I dividing by jobs ATTEMPTED or jobs COMPLETED? Recalculate
Cost/Job using completed jobs only. State the difference as a percentage.

3. TOKEN MATH AUDIT
Recompute my LLM cost per job from the token counts and list prices.
Flag arithmetic errors. Explain savings from prompt caching and batch
pricing, and whether batch is viable for my latency requirement.

4. PRICE VOLATILITY
Which prices are promotional or likely to change within 6 months?
Recompute Cost/Job at list price.

5. BREAKEVEN SENSITIVITY
Solve for the minimum success/containment rate required for 60% gross
margin at my price. Show the algebra. Show Cost/Job and gross margin
at success rates of 50%, 60%, 70%, 80% and 90%.

6. THE ONE NUMBER THAT KILLS ME
Identify the single input that, if wrong by 2x, breaks the model.
Be direct and highly critical. Use my numbers and show the arithmetic.

FINAL ASSUMPTIONS
Product: AI Notes Option B. The learner selects at least 2 evidence
items and 1 source; AI organizes a draft; the learner reviews every
section; the server saves a unique note_id. One completed job is one
such reviewed, saved note. No autonomous learning-outcome claim.
1,200 normal attempts/month; assisted completion R=0.70; no API eval
or willingness-to-pay evidence; only 2/2 prototype preferences.
Haiku 4.5 list price: $1/input and $5/output per 1M tokens.
2 normal calls/attempt; 2,400 fresh input and 800 output tokens/call;
cached tokens=0; expected technical whole-job retry cost=10% of LLM.
Infra=$0.045/normal attempt: hosting/DB/storage $0.020, logs $0.010,
payment/refund reserve $0.015. Base infra is assumed to include the
retry-related infra reserve; stress that assumption explicitly.
No speech/telephony: text-only product.
HITL A: learners correct their own notes. Provider QA/first-line
support pool: 10% of attempts x 6 minutes x $8/hour, split into
3 minutes QA and 3 minutes support. This allocation is unmeasured.
Overhead=$84/month: 10.5 hours of R&D/admin at $8/hour.
Package=$8/month for 12 saved notes; effective price=$8/12 per note;
22 normal attempts and at most 3 technical whole-job retries/month;
2 calls per job execution, hence at most 50 calls/account/month.
No automatic overage; an extra identical bundle requires opt-in.
At full note allowance: 70 paid accounts, 840 notes, revenue=$560.
Unvalidated value anchor: 20 net minutes saved/note x $8/hour x 12
notes=$32/month. A 25% capture cap implies $8/month. No headcount
replacement; wage-replacement anchor=0.
```

### Kết quả phản biện bằng tiếng Việt

**Token và mẫu số.** LLM/attempt = `2 × (2.400×1 + 800×5)/1.000.000 = $0,01280`. Retry = `$0,00128`. QA và hỗ trợ = `10% × 6/60 × $8 = $0,08000`. Tổng biến phí = `0,01280 + 0,00128 + 0,04500 + 0,08000 = $0,13908/attempt`. Cộng overhead phân bổ `$84/1.200 = $0,07` thành `$0,20908/attempt`.

Completed notes = `1.200×70% = 840`. **R là assisted completion có người học review, không phải containment tự động.** Chia cho attempts thay vì completed notes làm chi phí bị đánh giá thấp; sửa mẫu số làm Cost/Job tăng **42,86%**.

| Chỉ số cuối | Giá trị | Ô workbook |
|---|---:|---|
| COGS/completed note, chưa overhead | $0,198686 | `1_Cost_Job!B66` |
| Cost/completed note, gồm overhead | $0,298686 | `1_Cost_Job!B67` |
| Giá sàn 3×COGS | $0,596057 | `2_Pricing!B7` |
| Giá hiệu dụng $8/12 | $0,666667 | `2_Pricing!B19` |
| Gross margin theo COGS | 70,20% | `2_Pricing!B21` |
| Margin sau overhead | 55,20% | `2_Pricing!B61` |

Chi phí tháng là **$166,896** biến phí, **$250,896** gồm overhead; doanh thu **$560**. Giá sàn 3×COGS thấp hơn giá bán. Nếu dùng chi phí gồm overhead, sàn là **$0,896057/note**, vượt trần giá trị giả định **$0,666667**. Không được trình bày GM70,20% như lợi nhuận sau mọi chi phí.

**Ngưỡng gãy.** Với `P=8/12`, `GM=1−(0,13908/R)/P`. Muốn GM≥60%: `0,13908/R ≤ (8/12)×0,40`, nên **R≥52,155%** (`2_Pricing!B33`). Nếu gồm overhead thì **R≥78,405%** (`B63`). Ngưỡng GM50% là **R≥41,724%** (`B62`). R70% chưa được đo nên chưa thể kết luận sản phẩm thật đã qua các ngưỡng này.

| R có review | COGS/note | Cost/note gồm overhead | GM COGS | Sau overhead |
|---:|---:|---:|---:|---:|
| 50% | $0,278160 | $0,418160 | 58,28% | 37,28% |
| 60% | $0,231800 | $0,348467 | 65,23% | 47,73% |
| 70% | $0,198686 | $0,298686 | 70,20% | 55,20% |
| 80% | $0,173850 | $0,261350 | 73,92% | 60,80% |
| 90% | $0,154533 | $0,232311 | 76,82% | 65,15% |

Bảng sensitivity cố định 1.200 normal attempts/tháng và chuẩn hóa doanh thu theo giá hiệu dụng/note. Doanh thu subscription thực tế còn phụ thuộc số người trả tiền và mức dùng gói.

**Stress quota.** Dùng hết 22 normal attempts và 3 whole-job retries tạo tối đa `2×(22+3)=50` LLM calls. COGS/account = `25×0,0128 + 22×0,045 + 22×0,08 = $3,07`; GM = **61,625%**. Với 70 accounts, COGS **$214,90**, gồm overhead **$298,90**, margin sau overhead **46,625%**. Khoảng đệm GM60% chỉ 1,625 điểm phần trăm. Nếu mỗi retry còn phát sinh thêm `$0,045` infra chưa nằm trong reserve, COGS thành **$3,205/account**, GM **59,9375%**. Cap giúp giới hạn thiệt hại, chưa chứng minh reserve đủ.

**Khoản chưa được chứng minh.** QA và hỗ trợ đã được bổ sung nhưng 10%×6 phút vẫn là phân bổ gộp; support riêng tương đương `$48/tháng`, khoảng 5,14 phút/account ở 70 người. Cần đo ticket thật, timeout/retry, hóa đơn infra, storage/egress tăng theo thời gian và chi phí eval hồi quy. Ví dụ lập ngân sách 100 test attempts/tháng theo cùng biến phí là `$13,908`, không tạo doanh thu; không coi con số này là hóa đơn đã phát sinh. Thời gian người học review thuộc chi phí của khách; 20 phút tiết kiệm phải là số **ròng sau review/sửa**, chưa được đo.

**Cache, batch và giá.** Cached tokens=0 nên tiết kiệm caching base=0. Batch giảm một nửa LLM và retry liên quan, tiết kiệm **$8,448/tháng**, COGS/note còn **$0,188629**, GM **71,71%**. Dùng batch cho eval/offline; không mặc định áp cho luồng tương tác cần phản hồi ngay. Giá Haiku sử dụng list `$1/$5` theo [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing), ngày kiểm tra của bài 12/10/2026; không áp giá khuyến mại nên kịch bản trở về list không đổi base case. Giá tương lai vẫn có thể thay đổi.

**Input nguy hiểm nhất.** Nếu dự báo R70% cao gấp đôi thực tế R35%, COGS/note thành **$0,397371**, GM **40,39%**; sau overhead **10,39%**. Nếu toàn bộ pool QA+hỗ trợ `$0,08` tăng gấp đôi, GM còn **53,05%**. Nếu infra gấp đôi, GM còn **60,55%**: khoảng đệm rất hẹp.

### Quyết định sau phản biện

| Phản biện | Quyết định và thay đổi cuối |
|---|---|
| Gói cũ $6 và cap30 dễ làm economics xấu ở full quota | **Accept:** thử gói **$8**, **cap22**; giá vẫn là giả thuyết WTP. |
| Thiếu hỗ trợ khách hàng | **Accept:** bổ sung 3 phút first-line support bên cạnh 3 phút QA trong pool10%; ghi rõ chưa đo. |
| Hard cap2calls mâu thuẫn retry10% | **Accept:** 2 calls/job execution, tối đa **3 whole-job retries/account/tháng** và **50 calls** tổng. |
| R70% chứng minh autonomy/containment | **Reject:** gọi assisted completion; chưa có API eval. |
| GM70,20% là margin sau overhead | **Reject:** tách margin sau overhead55,20%. |
| Full-quota cap bảo đảm GM60% trong mọi điều kiện | **Partial:** đạt61,625% theo reserve hiện tại; giữ downside59,9375% nếu retry infra phát sinh thêm. |
| Giá8USD đã được khách chấp nhận | **Reject:** chỉ phù hợp trần giá trị giả định; phải thử thanh toán thật. |

## 2. §4.7.3 — Channel Reality Check

### English prompt executed

```text
I am choosing ONE go-to-market channel for the next 90 days.

My numbers:
- ARPU: $8/month; COGS gross margin: 70.197142857%.
- Segment: individual learners/prosumers, an explicit extension of
  the lab's SMB/mid-market/enterprise table.
- Payback allowance: 3 months; computed CAC ceiling: $16.847314286.
- Operating CAC target: $12; $840 total acquisition budget for
  70 paid accounts, comprising $420 cash and 52.5 founder hours
  valued at $8/hour. These are plans, not observed results.
- Chosen channel: PLG only for 90 days.
- Pain moment hypothesis: 22:30, after an online class, the learner
  organizes evidence and source links in Chrome.
- Proposed integration: Chrome side panel/context menu; not deployed.
- ACV=$96; AE quota assumption=$120,000/year; 240 working days;
  fully-loaded AE cost assumption=$24,000/year.
- Rep-driven CPO assumption=$300, comprising 5 hours x $8 plus
  $260 outreach; win rate=20%. Do not double-count this labor.
- Template ICONIQ CPO $6,300 is unverified; stress reference only,
  not an established current or local-market benchmark.
- Product, price, quotas, R semantics and evidence gaps are as
  specified in the Cost/Job Stress Test above.

Task:
1. Compute deals one AE must close per year and working day at my
ACV, assuming realistic quota and fully-loaded cost for my market.
State assumptions and whether sales-led is arithmetically possible.
2. Compare my CAC budget with rep-driven cost-per-opportunity
benchmarks; state the gap as a multiple and flag uncertainty.
3. Stress test the pain moment: can it name an integration surface?
If vague, identify the missing detail.
4. If Partner-Led, explain the required partner incentives and the
fastest two-week falsification. Otherwise mark this not applicable.
5. Give the strongest argument AGAINST my chosen channel.
Be blunt. I would rather be wrong now than in month 3.
```

### Kết quả phản biện bằng tiếng Việt

**Affordability.** `CAC ceiling = $8 × 70,1971% × 3 = $16,8473` (`4_Channel_Fit!B9`). Mục tiêu12USD thấp hơn ceiling; payback base **2,14 tháng**, nếu dùng margin sau overhead là **2,72 tháng**. Budget840USD/70paid=12USD nhưng chưa có khách trả tiền để tính CAC thực. Full hard-quota GM61,625% cho ceiling3tháng **$14,79**, payback CAC12 là **2,43 tháng**. Dùng margin sau overhead tại full quota thì ceiling chỉ **$11,19**; không che rủi ro này.

**Sales-Led không phù hợp theo giả định.** ACV=`8×12=$96`; quota120.000USD cần **1.250 deals/năm**, **5,21 deals/ngày** trên240 ngày (`4_Channel_Fit!B16`). AE fully-loaded24.000USD/năm là giả định, không phải benchmark local đã đo: riêng AE cost là **$19,20/deal** ở quota100%, **$25,60** ở quota75%, trước chi phí khác. CPO300USD/win20% tạo **CAC1.500USD** (`B22`), bằng **89,03× CAC ceiling16,8473USD** (`B23`) và **125× target12USD**. CPO tối đa tương ứng target chỉ **$2,40**. Không so CPO trực tiếp với CAC, và không cộng AE labor lần nữa nếu đã nằm trong CPO.

Template CPO6.300USD chưa xác minh chỉ là stress reference: với win20%, CAC31.500USD bằng2.625× target. Không coi đó là bằng chứng ICONIQ2026 hoặc benchmark Việt Nam đã kiểm tra.

**Pain moment và điểm nhúng.** Giờ22:30 + việc gom evidence sau lớp + Chrome đủ để thiết kế side panel/context menu. Chưa quan sát thói quen này ngoài thực tế, chưa đo nhu cầu cài extension, chưa triển khai tích hợp. Partner-Led không áp dụng vì chỉ chọnPLG.

**Lập luận mạnh nhất chốngPLG.** ARPU thấp khiến sales khó nuôi nhưng không tạo traffic, khả năng tự dùng, retention hay tự trả tiền. 2/2 preference không chứng minh WTP. Cần thử tự tạo note, dùng lại sau7ngày, thời gian tiết kiệm ròng và thanh toán8USD. Cohort nhỏ trong2tuần có thể đặt learning gate15người thử, ≥5dùng lại, ≥3trả tiền không giảm giá; đây là gate vận hành đề xuất, không phải kết quả đã đạt hoặc chứng minh thống kê.

### Quyết định sau phản biện

| Phản biện | Quyết định và lý do |
|---|---|
| Duy trì nhiều kênh90ngày | **Reject:** chốt **PLG duy nhất**. |
| PLG đã được chứng minh | **Reject:** chỉ là kênh thử nghiệm, cần đo activation, repeat và paid conversion. |
| Tuyển AE ở ARPU8USD | **Reject:** CAC và throughput không phù hợp theo assumptions. |
| Bỏ qua thời gian founder trong CAC | **Accept sửa:** giữ420USD cash +52,5giờ×8USD trong budget840USD. |
| Dùng SMB12tháng payback cho prosumer | **Reject:** dùng3tháng và ghi đây là phần mở rộng có chủ ý. |
| CPO6.300USD là benchmark đã xác minh | **Reject:** chỉ stress reference chưa xác minh. |
| Tích hợp Chrome phù hợp pain moment | **Partial:** điểm nhúng cụ thể, nhưng pain moment và adoption chưa đo. |

## 3. Checklist nhất quán one-pager — self-review

Đây là **tự kiểm tra số liệu bằng công thức và final metrics**, không phải người lạ đã đọc one-pager. Các số được đối chiếu với `/private/tmp/day22-lab/metrics.json`; ô workbook ghi trong các bảng trên. Cần kiểm tra hình thức của bản PDF cuối riêng.

- [x] Job=note đã được người học review và server lưu unique note_id; không dùng API request làm job.
- [x] `1.200×70%=840`; R là assisted completion chưa đo, không phải autonomy.
- [x] `70×$8=$560`; gói12notes, giá hiệu dụng `$8/12=$0,666667`.
- [x] COGS0,198686USD; giá sàn0,596057USD; GM70,20%; gồm overhead55,20% cùng cơ sở tính.
- [x] Ngưỡng GM60%=R52,155%; gồm overhead78,405%; không khẳng định đã vượt ngưỡng bằng eval.
- [x] Quota22attempts +3whole-job retries = tối đa50calls; stress GM61,625%, downside59,9375% có ghi điều kiện infra.
- [x] CAC ceiling16,8473USD; target12USD; sales CAC1.500USD; gap89,03× ceiling/125×target.
- [x] ChỉPLG90ngày; Chrome side panel/context menu là đề xuất chưa triển khai.
- [x] Trần giá trị32USD×25%=8USD là giả định; không biến2/2 preference thành WTP.
- [x] Không ghi đã có API eval, paid buyers, cohort thử nghiệm, SOC2 hoặc bài test người lạ.
- [ ] Người thật chưa biết sản phẩm đọc PDF trong2phút và hỏi lại≤3câu: **chưa thực hiện**.

**Giới hạn bằng chứng:** số tính nhất quán không chứng minh người học tiết kiệm20phút, completion70%, support đủ, retention đủ dài hoặc khách sẵn sàng trả8USD. Bài làm giữ rõ những khoảng trống này để kiểm chứng trong pilot.
