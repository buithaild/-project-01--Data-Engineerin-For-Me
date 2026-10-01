Version 2:
1. Business Problems
BP-01 — Growing dependency on data
ShopVerse đang tăng trưởng nhanh và các business teams ngày càng phụ thuộc vào data cho reporting, decision-making, operations và financial reconciliation.
Current data-access processes không scale tốt theo mức tăng của consumers và data volume.
BP-02 — Analytics phụ thuộc operational systems
Một số analytics hiện query operational data trực tiếp hoặc phụ thuộc vào manual exports/scripts.
Điều này tạo operational risk và tăng dependency vào engineering.
Customer-facing workloads như checkout, ordering và payment phải luôn được ưu tiên hơn analytics workloads.
BP-03 — Fragmented analytics
Business data nằm trong nhiều source systems:
Commerce
Payment
Inventory
Web/mobile tracking

Không có một analytical view thống nhất xuyên các business domains.
BP-04 — Metric inconsistency
Các team có thể tính cùng một business concept theo những cách khác nhau.
Đặc biệt các khái niệm như:
order value
paid value
recognized revenue
refund
net revenue

cần được định nghĩa và reconcile rõ ràng.
BP-05 — Historical analytics khó khăn
Business cần YoY analysis, campaign comparison, customer history và financial investigation nhưng current processes không cung cấp historical analytics đáng tin cậy và dễ sử dụng.
BP-06 — Poor reproducibility/auditability
CSV exports và independent scripts khiến report khó:
reproduce
trace
audit
correct

khi có vấn đề.
BP-07 — Data quality problems
Operational/tracking data có thể chứa:
- duplicates;
- missing identifiers;
- late updates;
- malformed records;
- schema changes.
Business cần biết khi analytical results không còn đáng tin cậy.
2. Data Consumers & Use Cases
Management
Cần:
- daily revenue/orders;
- WoW/YoY comparison;
- campaign performance.
Critical daily executive report phải available trước 08:00.
Finance
Cần:
- paid orders;
- successful/failed payments;
- refunds;
- recognized revenue;
- net revenue;
- reconciliation.
Daily Finance report target 09:00.
Correctness được ưu tiên hơn low latency.
Incomplete financial data không được silently presented như complete data.
Sales / Category
Cần:
- top-selling products;
- revenue by category/product;
- campaign performance;
- product/category trends.
General freshness target khoảng ≤1 hour.
Operations
Cần:
- delayed orders;
- order status;
- fulfillment duration;
- operational trends.
Target freshness:
≤30 minutes during normal operation.
Marketing
Cần:
- acquisition/channel analysis;
- customer behavior;
- funnel analysis;
- campaign monitoring;
- repeat customer analysis.
Một số use cases yêu cầu low latency, nhưng exact realtime SLA chưa finalized.
Data Analysts
Cần self-service analytical access cho:
- exploratory SQL analysis;
- historical analysis;
- cross-domain analysis;
- ad-hoc business questions.
Không nên cần engineering tạo manual export cho phần lớn common analyses.
3. Known Requirements
BR-01 — Operational isolation
Analytics/data workloads không được materially degrade customer-facing transactional systems.
BR-02 — Executive freshness
Previous-day executive report phải available trước 08:00 local business time.
BR-03 — Finance freshness
Previous-day Finance report target available trước 09:00.
Correctness quan trọng hơn latency vài phút.
BR-04 — Operational freshness
Operations analytics target ≤30 phút trong normal operation.
BR-05 — Sales freshness
Sales/category analytics thông thường target ≤1 giờ.
BR-06 — Historical analytics
Business phải có khả năng thực hiện multi-year historical analysis.
BR-07 — Financial retention
Orders/payments/refunds phải retained tối thiểu 7 năm.
BR-08 — Behavioral retention
Behavioral events ban đầu retained 13 tháng.
BR-09 — Product/catalog history
Product/catalog history retained tối thiểu 3 năm.
BR-10 — Late corrections
Valid late-arriving business changes phải có khả năng được phản ánh vào historical analytics.
BR-11 — Financial corrections
Financial reports phải có khả năng correction/reconciliation khi payment, refund hoặc valid business correction xuất hiện muộn.
BR-12 — Historical transaction semantics
Historical transaction analysis phải có khả năng phản ánh business attributes và transaction values tại thời điểm transaction xảy ra.
BR-13 — Financial auditability
Business-critical financial corrections phải traceable.
Finance cần xác định được:
what changed
when
why
affected period

BR-14 — Data completeness visibility
Business-critical report không được silently trình bày incomplete data như complete data.
BR-15 — PII restriction
Raw customer PII không được accessible mặc định cho general analytics users.
BR-16 — Sensitive payment exclusion
Raw card credentials, payment secrets, authentication secrets và tokens không thuộc analytics scope.
BR-17 — Access auditability
Access vào sensitive analytics data phải có khả năng audit.
BR-18 — Privacy
System phải hỗ trợ customer privacy/deletion/anonymization requirements mà không phá required financial transaction history.
Exact policy/SLA: TBD.
BR-19 — Growth
Architecture sau này phải xét business forecast khoảng 2–3x trong 2 năm.
5–10x được xem là scalability stress scenario, không phải committed business forecast.
Lưu ý: BR-19 hiện vẫn là business capacity expectation. Sau này chúng ta sẽ chuyển nó thành engineering capacity requirement.
BR-20 — Peak seasonality
System requirements phải xét traffic/data-generation spikes trong major campaigns.
Peak có thể tập trung mạnh trong 15–60 phút đầu campaign.
BR-21 — Source data imperfections
Analytics process phải account cho thực tế source data có thể:
arrive late
contain duplicates
contain malformed records
miss identifiers
change schema

Câu này vẫn hơi gần engineering language. Sau này chúng ta sẽ refine khi chuyển phase.
BR-22 — Self-service analytics
Analysts phải có khả năng tự thực hiện phần lớn common/ad-hoc analytical queries mà không phụ thuộc engineering tạo manual exports.
4. Open Questions / Ambiguities
Không cần giải quyết tất cả trước architecture V0, nhưng phải track chúng.
OQ-01: Marketing low-latency use cases cụ thể là gì và business action nào phụ thuộc vào chúng?
OQ-02: Marketing freshness target chính xác là bao nhiêu?
OQ-03: Marketing chấp nhận trade-off correctness/completeness vs freshness đến mức nào?
OQ-04: Acceptable tracking-event loss/duplicate thresholds là bao nhiêu?
OQ-05: Identity semantics giữa anonymous user → logged-in customer cần như thế nào?
OQ-06: Exact customer deletion/anonymization policy và SLA là gì?
OQ-07: Exact access-control matrix cho PII là gì?
OQ-08: Sensitive-data access logs phải retained bao lâu?
OQ-09: Formal checkout/order/payment SLOs hiện tại là gì?
OQ-10: Exact monthly Finance closing/correction process là gì?
OQ-11: Tracking late-arrival correction window nên bao lâu?
OQ-12: Formal tracking schema compatibility contract là gì?
OQ-13: Inventory historical requirements là snapshot, complete change history hay chỉ current state?
OQ-14: Actual source data sizes tính theo bytes/day là bao nhiêu?
OQ-15: Peak events/sec và source change rate thực tế là bao nhiêu?
OQ-16: Ad-hoc analytical query concurrency dự kiến bao nhiêu users/queries?
OQ-17: Dashboard/query performance expectation là bao nhiêu giây/phút?
OQ-18: Detailed Finance definitions cho tax, voucher, discount và special refund cases là gì?
5. Assumptions
Phần này phải tách khỏi requirement.
A-01
ShopVerse hiện chỉ hoạt động với VND.
A-02
Project 01 không cần xử lý raw card credentials hoặc authentication secrets.
A-03
Current volume estimates là approximate, chưa phải measured capacity specification.
A-04
Forecast 2–3x / 2 years dùng cho planning; không đảm bảo actual growth.
A-05
5–10x chỉ là architecture stress scenario.
A-06
Formal legal/compliance interpretation nằm ngoài phạm vi quyết định của Data Engineer; policy phải đến từ authorized business/legal stakeholders.

