Version 1:
Excercise 1:
1. Business problem: 
ShopVerse đang gặp vấn đề:
business đang tăng trưởng khá nhanh, nhu cầu sử dụng data giữa các bên liên quan ngày càng nhiều, business ngày càng phụ thuộc vào data.
nên vấn đề Data Platform/Data Warehouse trở nên cần thiết.
2. Consumers: Những nhóm sử dụng data là
- Management: muốn xem revenue hôm qua, so với tuần trước, campaign có hiệu quả không?
- Finance: Bao nhiêu order thực sự được thanh toán? Bao nhiêu payment failed? Refund tháng này bao nhiêu? Revenue report có reconcile được với payment records không?
- Sales / Category team: Product nào đang bán tốt nhất?, Category nào tạo nhiều revenue nhất? Conversion/revenue thay đổi thế nào trong campaign?
- Marketing: Customer nào đã mua nhiều lần? Customer acquisition channel nào mang lại nhiều revenue?
- Operations: Bao nhiêu order đang delayed? Thời gian trung bình từ order → shipped → delivered là bao lâu?
- Data analysts: Muốn tự thực hiện analytical queries mà không phải yêu cầu engineering export dữ liệu mỗi lần
3. Workload:
3 đặc điểm của workload sẽ ảnh hưởng tới thiết kế: 
Trong campaign thì operation business tăng, behavioral event tăng lớn; Management muốn trong vài năm tới volume có thể lớn hơn hiện tại 5- 10 lần; Traffic không đều giữa các thời điểm trong ngày, hoặc khoảng thời gian đầu của những ngày diễn ra campaign
4. Correctness
Tính toán Revenue (Doanh thu) phức tạp hơn phép tính SUM(order.amount) vì dữ liệu thực tế trong hệ thống vận hành luôn phát sinh các nghiệp vụ điều chỉnh, hoãn lại hoặc hủy bỏ dòng tiền. Phép toán SUM(order.amount) đơn thuần chỉ phản ánh tổng giá trị đơn hàng được khởi tạo.
5. Production constraint: Requirement nào cho thấy analytics system không thể được thiết kế mà bỏ qua operational production system?
Tôi chưa trả lời được

Excercise 2:
8 requirement tôi cho rằng hiện vẫn ambiguous hoặc thiếu:
- History: Finance cần truy vấn transaction history tối đa bao nhiêu năm? Có regulatory requirement bắt buộc phải giữ financial records trong một khoảng thời gian cụ thể không?
freshness: 
correctness: 
late data: 
deletion: 
failure: 
availability: 
security: 
behavioral events: 
growth: 

Excercise 3: realtime challenge
Marketing muốn realtime analytics.
Tại sao cần realtime? 
realtime là bao lâu: 1 phút? 5 phút? 30 phút?

