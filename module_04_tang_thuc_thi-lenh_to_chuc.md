# Module 4 — Tầng thực thi / Lệnh tổ chức (Institutional Execution)

## Câu hỏi trung tâm

**Nếu một quỹ cần mua lượng cổ phiếu tương đương nhiều ngày giao dịch, họ làm thế nào để mua đủ mà không tự đẩy giá lên quá mạnh?**

## Thuật ngữ cần nắm trước

| English term | Cách gọi tiếng Việt | Định nghĩa ngắn bằng tiếng Việt |
|---|---|---|
| Institutional Execution | Thực thi lệnh tổ chức | Quá trình chuyển quyết định đầu tư lớn thành giao dịch thực tế. |
| Market Impact | Tác động lên giá | Phần giá thay đổi do chính lệnh tiêu thụ thanh khoản hoặc làm người khác phản ứng. |
| Implementation Shortfall | Chênh lệch thực thi | Khoảng cách giữa kết quả danh mục thực tế và kết quả giả định tại giá quyết định. |
| Opportunity Cost | Chi phí cơ hội | Thiệt hại khi chưa khớp đủ và giá chạy khỏi mức mong muốn. |
| Order Slicing | Chia nhỏ lệnh | Tách lệnh mẹ thành nhiều lệnh con để kiểm soát tác động. |
| VWAP | Giá bình quân theo volume | Benchmark hoặc lịch thực thi bám phân bố volume thị trường. |
| TWAP | Giá bình quân theo thời gian | Benchmark hoặc lịch chia lệnh tương đối đều theo thời gian. |
| POV | Tỷ lệ tham gia | Thực thi theo một tỷ lệ của volume thị trường. |
| Iceberg Order | Lệnh tảng băng | Chỉ hiển thị một phần khối lượng, phần còn lại được bổ sung. |
| Information Leakage | Rò rỉ thông tin | Người khác suy ra nhu cầu lớn và điều chỉnh hành vi bất lợi cho người thực thi. |

---

# 1. Vấn đề mở đầu: mua 3 triệu cổ phiếu trong thị trường chỉ giao dịch 1 triệu/ngày

Quỹ A quyết định mua 3,000,000 cổ phiếu XYZ. Giá quyết định là 50.00. XYZ thường giao dịch 1,000,000 cổ phiếu/ngày, ask gần nhất chỉ có 80,000 cổ phiếu.

Trước khi đọc tiếp:

1. Gửi market buy 3 triệu một lần sẽ gây gì?
2. Chia quá chậm tạo rủi ro nào?
3. Nếu người khác phát hiện quỹ đang mua, họ phản ứng ra sao?
4. Giá khớp đẹp nhưng chỉ mua được 10% có phải thực thi tốt không?
5. Benchmark nào phù hợp với mục tiêu của quỹ?

```text
LỆNH MẸ: MUA 3,000,000 CP
          │
     ┌────┴────────────────────────┐
     │                             │
GỬI NHANH / AGGRESSIVE        CHIA CHẬM / PASSIVE
     │                             │
quét nhiều ask                 impact tức thời thấp
impact + slippage cao          nhưng giá có thể chạy mất
     │                             │
hoàn tất nhanh                 có thể không mua đủ
     └─────────────┬───────────────┘
                   ↓
 TỐI ƯU: CHI PHÍ + RỦI RO + THỜI GIAN + THÔNG TIN
```

**Cách đọc:** hai nhánh là hai cực của bài toán. Không có lựa chọn miễn phí; execution tốt là cân bằng theo mục tiêu và ràng buộc cụ thể.

Phát biểu nhân quả: **size càng lớn so với liquidity, giao dịch nhanh càng tăng impact; giao dịch chậm càng tăng opportunity cost và rủi ro không hoàn tất.**

> **Ghi nhớ:** quỹ không chỉ chọn mua gì; họ phải chọn cách biến quyết định thành vị thế thật.

---

# 2. WHY — Tại sao execution là một bài toán riêng?

Giá trên màn hình áp dụng cho lượng nhỏ ở đầu sổ lệnh, không phải toàn bộ 3 triệu cổ phiếu. Quyết định đầu tư đúng vẫn có thể tạo kết quả kém nếu:

- Trượt giá quá lớn.
- Tác động lên giá làm chính phần còn lại đắt hơn.
- Chờ quá lâu và bỏ lỡ chuyển động.
- Rò rỉ nhu cầu khiến người khác front-run hoặc rút liquidity.
- Không hoàn tất đủ size để danh mục đạt mục tiêu.

Nếu execution không tồn tại như một chức năng chuyên môn, alpha dự báo có thể bị chi phí giao dịch ăn hết.

---

# 3. WHAT — Bản chất là gì?

## Tầng 1 — Trực giác

Mua một chai nước khác mua toàn bộ kho. Giá niêm yết của chai đầu không phải chi phí bình quân của cả kho; người bán còn thay đổi giá khi nhận ra bạn cần mua nhiều.

## Tầng 2 — Định nghĩa chuẩn

Institutional execution là quá trình lập kế hoạch, định tuyến, chia nhỏ và điều chỉnh lệnh lớn để đạt mục tiêu danh mục trong giới hạn chi phí, rủi ro, thời gian, benchmark và quy định.

## Tầng 3 — First Principles

```text
QUYẾT ĐỊNH ĐẦU TƯ
      ↓ target position
LỆNH MẸ (PARENT ORDER)
      ↓ chia theo chiến lược
LỆNH CON (CHILD ORDERS)
      ↓ tương tác liquidity/order flow
GIAO DỊCH THỰC TẾ
      ↓
VỊ THẾ + GIÁ BÌNH QUÂN + CHI PHÍ + PHẦN CHƯA KHỚP
```

**Cách đọc:** quyết định chỉ là mục tiêu. Execution chuyển mục tiêu thành lệnh con, và thị trường phản hồi sau từng giao dịch.

Phát biểu nhân quả: **kết quả thực tế phụ thuộc không chỉ dự báo mà còn cách lệnh con tác động vào thanh khoản và làm thay đổi điều kiện cho các lệnh tiếp theo.**

> **Ghi nhớ:** decision price là điểm xuất phát; execution price mới là kết quả thật.

## Execution không phải là gì?

- Không phải luôn dùng VWAP.
- Không phải mọi lệnh chia nhỏ đều là “gom hàng”.
- Không phải giá đi ngang chứng minh tổ chức đang mua.
- Không phải minimization of impact bằng mọi giá; có thể phải ưu tiên hoàn tất.
- Không phải mọi hành vi tổ chức là thao túng.

---

# 4. MECHANISM

## 4.1 Các loại chi phí

```text
TỔNG CHI PHÍ THỰC THI
       │
 ┌─────┼──────────┬──────────────┐
 │     │          │              │
PHÍ   SPREAD   SLIPPAGE/IMPACT  OPPORTUNITY COST
trực  vượt giá  do thanh khoản   phần chưa khớp khi
tiếp  đối diện  và phản ứng      giá chạy mất
```

**Cách đọc:** phí môi giới chỉ là phần dễ thấy. Với lệnh lớn, impact và opportunity cost thường quan trọng hơn.

Phát biểu nhân quả: **giảm một chi phí có thể tăng chi phí khác; đặt passive giảm spread/impact nhưng tăng non-completion và opportunity cost.**

> **Ghi nhớ:** execution cost là hệ thống trade-off, không phải một con số phí.

## 4.2 Implementation Shortfall

Giả sử quỹ quyết định mua 100,000 cổ phiếu tại 50.00.

- Khớp 80,000 tại giá bình quân 50.40.
- Cuối kỳ, 20,000 chưa khớp; giá đóng 51.00.
- Phí trực tiếp 5,000.

```text
KẾ HOẠCH GIẢ ĐỊNH: 100,000 × 50.00
             ↓
ĐÃ KHỚP 80,000 × 50.40
   impact/slippage = 80,000 × 0.40 = 32,000
             ↓
CHƯA KHỚP 20,000; GIÁ CUỐI 51.00
   opportunity cost = 20,000 × 1.00 = 20,000
             ↓
PHÍ TRỰC TIẾP = 5,000
             ↓
IMPLEMENTATION SHORTFALL ≈ 57,000
```

**Cách đọc:** so toàn bộ danh mục thực tế với phương án giả định mua đủ tại decision price. Ví dụ đơn giản bỏ qua một số chi tiết kế toán và hướng dấu.

Phát biểu nhân quả: **một lệnh chưa khớp không có slippage đã trả nhưng vẫn gây opportunity cost nếu giá chạy theo hướng quyết định.**

> **Ghi nhớ:** không giao dịch cũng có thể là một chi phí.

## 4.3 Chia nhỏ lệnh

Nếu ask gần có 80,000, gửi 3 triệu sẽ để lộ nhu cầu và quét sâu. Chia lệnh cho phép quan sát phản ứng sau từng module.

```text
LỆNH MẸ 3,000,000
       ↓
50,000 → đo impact/replenishment
       ↓ cập nhật
40,000 → đổi mức chủ động
       ↓ cập nhật
70,000 → tăng/giảm participation
       ↓
LẶP CHO TỚI KHI HOÀN TẤT HOẶC ĐIỀU KIỆN THAY ĐỔI
```

**Cách đọc:** execution là vòng feedback, không nhất thiết lịch cố định. Mỗi child order tạo thông tin về liquidity và market response.

Phát biểu nhân quả: **chia nhỏ giảm impact tức thời và cho phép học từ thị trường, nhưng kéo dài thời gian và tạo pattern có thể bị phát hiện.**

> **Ghi nhớ:** slicing đổi một cú impact lớn thành chuỗi trade-off nhỏ hơn.

## 4.4 VWAP, TWAP và POV

```text
                     MỤC TIÊU THỰC THI
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
       VWAP                TWAP                POV
theo hình dạng volume   đều theo thời gian   tỷ lệ volume thực tế
        │                   │                   │
phụ thuộc dự báo volume dễ hiểu, kém thích nghi tự co giãn với activity
```

**Cách đọc:** đây là họ chiến lược/benchmark, không phải ba cấp độ tốt–xấu. Phù hợp phụ thuộc mục tiêu.

- VWAP: hữu ích khi muốn bám volume curve; sai forecast làm lịch lệch.
- TWAP: đơn giản; dễ lộ nhịp đều và bỏ qua biến động liquidity.
- POV: tham gia x% volume; thời gian hoàn tất không chắc chắn.

Phát biểu nhân quả: **lịch thực thi thay đổi dấu vết order flow; chiến lược bám activity giảm tỷ lệ tham gia khi volume thấp nhưng có thể không hoàn tất đúng hạn.**

> **Ghi nhớ:** benchmark đo kết quả; algorithm là cách theo đuổi kết quả.

## 4.5 Iceberg và hidden liquidity

```text
SELL ICEBERG TỔNG 500,000
HIỂN THỊ 20,000
      ↓ bị mua hết
BỔ SUNG 20,000
      ↓ lặp lại
VOLUME KHỚP LỚN NHƯNG ASK KHÔNG BIẾN MẤT
```

**Cách đọc:** phần hiển thị nhỏ không phản ánh tổng size. Replenishment lặp lại là evidence gợi ý, không xác nhận danh tính hoặc tổng lượng.

Phát biểu nhân quả: **ẩn size giảm information leakage trực tiếp nhưng giao dịch lặp lại vẫn có thể để lại dấu vết và bị thuật toán khác suy luận.**

> **Ghi nhớ:** hidden không có nghĩa invisible sau khi bắt đầu khớp.

## 4.6 Tác động tạm thời và lâu dài

- Temporary: liquidity bị ăn rồi quay lại, giá hồi một phần.
- Permanent/information impact: thị trường suy luận lệnh có thông tin hoặc cập nhật giá trị, giá không quay lại.

Không thể tách hoàn hảo theo thời gian thực; cần counterfactual không quan sát được: giá sẽ thế nào nếu lệnh không tồn tại?

## 4.7 Nếu thay đổi biến

- Size/order volume tăng → participation và impact thường tăng.
- Deadline ngắn → ưu tiên immediacy, impact tăng.
- Volatility/adverse selection tăng → liquidity rút, execution khó hơn.
- Alpha decay nhanh → chờ đợi đắt hơn.
- Benchmark khác → lịch tối ưu khác.

---

# 5. ACTORS

- Portfolio manager: quyết định target và urgency.
- Trader/execution desk: chọn chiến lược, venue, mức chủ động.
- Algorithm/smart order router: chia và định tuyến lệnh.
- Broker: cung cấp hạ tầng/agency/principal liquidity.
- Market maker: phía đối diện, quản lý inventory.
- Arbitrageur/prop trader: phản ứng với dấu vết flow.
- Nhà đầu tư khác: cung cấp hoặc tiêu thụ liquidity.

Who's on the other side? Quỹ mua lớn cần seller: nhà đầu tư thoát vị thế, market maker, arbitrageur hoặc tổ chức khác.

---

# 6. INCENTIVES

```text
OBJECTIVE → CONSTRAINTS → COST → RISK → INFORMATION → TIME
```

Portfolio manager muốn exposure; trader muốn execution tốt; broker muốn doanh thu và chất lượng; market maker muốn spread nhưng tránh adverse selection. Các mục tiêu không hoàn toàn trùng nhau, nên governance và benchmark quan trọng.

---

# 7. EVIDENCE

## Quan sát được

- Trades lặp lại cùng phía/size.
- Participation rate, fill rate, average price.
- Spread, depth, impact, reversion.
- Volume curve và venue fills.
- Giá trước, trong, sau execution nếu chính bạn là người thực thi.

## Chỉ suy luận từ bên ngoài

- Một quỹ cụ thể đang gom/xả.
- Lệnh mẹ còn bao nhiêu.
- Benchmark/urgency thật.
- Iceberg thuộc actor nào.

Nếu execution algorithm đang mua, ta có thể kỳ vọng buy flow lặp lại, nhịp điều chỉnh theo volume/liquidity và impact tăng khi participation tăng. Nhưng nhiều actor nhỏ có thể tạo dấu vết giống vậy.

---

# 8. ALTERNATIVE EXPLANATIONS

Hiện tượng: buy flow đều đặn cả ngày, giá tăng chậm.

| Giả thuyết | Cơ chế | Ủng hộ | Làm yếu | Tiếp theo |
|---|---|---|---|---|
| Quỹ dùng algo mua | Child orders theo lịch | Nhịp flow và participation ổn định | Flow biến mất ngẫu nhiên | Kéo dài tới deadline |
| Nhiều retail cùng mua | Phản ứng chung với tin | Ticket nhỏ, catalyst | Size/pattern quá đều | Phụ thuộc sentiment |
| Market maker hedge | Mua để cân inventory | Liên hệ derivatives | Không có exposure liên quan | Dừng khi hedge đủ |
| Index/rebalance | Flow bắt buộc | Gần close, nhiều mã đồng thời | Chỉ một mã | Không nhất thiết tiếp diễn |

```text
FLOW MUA ĐỀU
   ├── ALGO THỰC THI
   ├── ĐÁM ĐÔNG PHẢN ỨNG CÙNG TIN
   ├── HEDGE / ARBITRAGE
   └── REBALANCE / FLOW BẮT BUỘC
```

**Cách đọc:** hình dạng flow không định danh actor. Context, timing và tài sản liên quan tạo bằng chứng phân biệt.

> **Ghi nhớ:** execution pattern là inference; ownership là story nếu thiếu dữ liệu.

---

# 9. FALSIFICATION

Giả thuyết: tổ chức đang tích lũy bằng VWAP.

Làm mạnh: participation bám volume curve, flow kéo dài, fills phân tán, không phụ thuộc mốc chart đơn lẻ.

Làm yếu: flow chỉ xuất hiện sau tin; tập trung closing auction; nhiều mã index cùng pattern; không có persistence; thanh khoản mỏng tạo ảo giác lệnh lớn.

Confirmation bias: gọi mọi range là accumulation; chỉ nhìn mua mà quên seller; suy actor từ round size; giải thích sau khi giá tăng.

---

# 10. APPLICATION

- Observe: volume, flow, impact, time, context.
- Interpret: có persistence và response nào?
- Hypothesize: algo, hedge, rebalance, crowd.
- Predict: lịch/timing và điều kiện kết thúc.
- Test: so với volume curve, close, peers, derivatives.
- Update probability.
- Decide: execution strategy hoặc không kết luận actor.

Đối với nhà đầu tư cá nhân, bài học hữu ích nhất là hiểu rằng dấu vết tổ chức không tạo entry tự động; liquidity, timing và invalidation vẫn cần thiết.

---

# 11. FACT → INFERENCE → STORY

```text
FACT                         INFERENCE                    STORY
20k ask tái xuất hiện 8 lần  có thể là iceberg           “quỹ X đang xả”
POV gần 10% trong 2 giờ      có thể là algo thực thi      “smart money gom”
giá tăng chậm cùng flow mua  demand có persistence        “họ biết tin nội bộ”
```

Mọi câu ở cột Story cần dữ liệu định danh/động cơ mà thị trường công khai thường không cung cấp.

---

# 12. CASE STUDIES

## Case A — Mua lớn với lịch thích nghi

Facts: quỹ cần 500k; volume tăng; algo duy trì POV 12%; spread bình thường; hoàn tất 95%.

Mechanism: child orders co giãn theo market volume → impact được phân tán → completion cao.

Actors/Incentives: portfolio manager cần exposure; execution algo cân impact và deadline; sellers cung cấp đối ứng.

Hypotheses/Evidence: adaptive POV phù hợp nếu participation bám volume thực và impact ổn định.

Falsification: impact tăng phi tuyến hoặc alpha decay nhanh làm POV quá chậm.

Conclusion: strategy hợp mục tiêu, không có nghĩa giá rẻ tuyệt đối.

## Case B — VWAP-looking flow nhưng là rebalance

Facts: flow mua gần close ở nhiều mã cùng index.

Mechanism: benchmark/rebalance bắt buộc, không phải tích lũy thông tin.

Actors/Incentives: passive funds cần bám chỉ số; arbitrageurs và market makers cung cấp đối ứng.

Hypotheses/Evidence: rebalance được ủng hộ bởi timing đồng thời và basket-wide flow; accumulation riêng mã yếu đi.

Falsification: flow kéo dài ngoài kỳ rebalance và chỉ tập trung một mã với context riêng.

Conclusion: hình dạng mua tổ chức giống nhau nhưng động cơ và persistence khác.

## Case C — Mơ hồ từ chart

Facts: range 10 ngày, volume tăng ở đáy.

Hypotheses: accumulation, distribution, market making, two-way transfer, index flow.

Mechanism/Actors/Incentives: nhiều actor có thể chia lệnh hoặc cân inventory; range chỉ là kết quả tổng hợp.

Evidence: cần order flow, timing, response ở hai biên, peers và catalyst.

Falsification: mỗi hypothesis phải có prediction riêng; không có prediction thì không thể phân biệt.

Conclusion: chart không đủ để suy execution program.

---

# 13. Câu hỏi Socratic

1. Tại sao decision price khác execution price?
2. Gửi lệnh nhanh giảm chi phí nào và tăng chi phí nào?
3. Tại sao unfilled shares vẫn tạo cost?
4. VWAP khác TWAP ở đầu vào lịch thế nào?
5. POV có rủi ro completion gì?
6. Iceberg giảm và không loại bỏ leakage ra sao?
7. Ai ở phía đối diện khi quỹ mua?
8. Vì sao range không chứng minh accumulation?
9. Khi alpha decay nhanh, urgency nên đổi ra sao?
10. Điều gì bác bỏ giả thuyết VWAP accumulation?

## Đáp án ngắn

1. Thị trường và chính lệnh thay đổi sau quyết định.
2. Giảm opportunity/non-completion; tăng spread/impact.
3. Giá có thể chạy khỏi decision price.
4. VWAP theo volume curve; TWAP theo thời gian.
5. Volume thấp khiến lệnh kéo dài.
6. Replenishment/pattern vẫn bị phát hiện.
7. Sellers thụ động hoặc chủ động ở venue khác.
8. Nhiều cơ chế tạo range.
9. Tăng urgency nếu lợi thế biến mất nhanh.
10. Timing theo rebalance/news, thiếu persistence, context khác tốt hơn.

---

# 14. Kiểm tra thực sự hiểu

**Reverse:** giá tăng đều cả ngày — nêu algo buy, short covering, hedge, crowd, thin liquidity.

**What-if:** deadline giảm một nửa → urgency/participation tăng, expected impact tăng.

**Counterexample:** giá bình quân tốt hơn VWAP nhưng chỉ hoàn tất 20%, trong khi giá sau đó tăng mạnh; execution có thể vẫn kém.

**Teach-back:** mua quá nhiều làm hàng rẻ hết và tiết lộ nhu cầu; chia nhỏ giảm dấu chân nhưng tăng nguy cơ mua không đủ.

---

# 15. Connection Map

```text
ORDER FLOW                  INSTITUTIONAL EXECUTION              MARKET STRUCTURE
dấu vết lệnh chủ động  →   kiểm soát nhịp lệnh lớn       →     dấu vết tích lũy theo thời gian
absorption/exhaustion       slicing/impact/benchmark             trend/range/breakout
```

**Cách đọc:** Module 4 giải thích một nguồn tạo persistence trong Module 3 và một nguồn tạo cấu trúc quan sát ở Module 5.

> **Ghi nhớ:** execution lặp lại biến bài toán một lệnh thành dấu vết nhiều phiên.

---

# 16. Gate 4

Phân tích được quỹ cần mua nhiều ngày liquidity bình thường: objective, constraints, costs, risk, information leakage, time; trade-off nhanh/chậm; strategy; evidence; alternatives; falsification.

---

# 17. Kết thúc bài

## First-Principles Summary

1. Quyết định đầu tư và kết quả execution là hai bài toán khác nhau.
2. Size lớn so với liquidity tạo trade-off impact–opportunity–completion.
3. Slicing cho phép thích nghi nhưng kéo dài và có thể lộ pattern.
4. VWAP/TWAP/POV là benchmark hoặc cách thực thi có mục tiêu khác nhau.
5. Dấu vết execution không đủ định danh actor hay chứng minh accumulation.

## Mental Model

**Đi nhanh làm nước bắn; đi chậm có thể lỡ chuyến.**

## Không được nhầm

- VWAP với “giá trị hợp lý”.
- Iceberg với thao túng.
- Range với accumulation.
- Institutional flow với informed flow.
- Giá đẹp trên phần đã khớp với execution tốt toàn lệnh.

## Tôi đã hiểu nếu...

- Tính và giải thích implementation shortfall đơn giản.
- Nêu trade-off của speed/impact/completion.
- So sánh VWAP/TWAP/POV.
- Giải thích slicing/iceberg từ incentives.
- Xây alternatives và falsification cho dấu vết tổ chức.

## Cầu nối sang bài tiếp theo

Chuỗi child orders, absorption và liquidity migration lặp lại tạo đỉnh, đáy, range và breakout. Module 5 quay lại chart để đọc các cấu trúc đó như kết quả, không như nguyên nhân.
