# Bài học Phần 3 — Dòng lệnh (Order Flow)

## Câu hỏi trung tâm

**Ai đang chủ động yêu cầu giao dịch ngay, và phía đối diện có hấp thụ được không?**

Phần 1 giải thích lệnh biến thành giá. Phần 2 giải thích thị trường có thể hấp thụ bao nhiêu lệnh. Phần 3 nối hai phần đó theo thời gian: lệnh chủ động đang đi theo hướng nào, có kéo dài không, và thanh khoản phía đối diện phản ứng ra sao?

## Thuật ngữ cần nắm trước

| English term | Cách gọi tiếng Việt | Định nghĩa ngắn bằng tiếng Việt |
|---|---|---|
| Order Flow | Dòng lệnh | Chuỗi lệnh và giao dịch cho thấy bên nào đang yêu cầu khớp ngay. |
| Aggressive Buyer/Seller | Người mua/bán chủ động | Người chấp nhận giá phía đối diện để giao dịch ngay. |
| Passive Buyer/Seller | Người mua/bán thụ động | Người đặt lệnh chờ và cung cấp thanh khoản. |
| Order Imbalance | Mất cân bằng lệnh | Một phía chủ động hoặc thanh khoản một phía chiếm ưu thế trong phạm vi xác định. |
| Absorption | Hấp thụ | Lệnh chủ động lớn được phía đối diện tiếp nhận mà giá tiến triển ít. |
| Exhaustion | Cạn lực | Lực chủ động suy yếu nên giá không còn tiếp diễn. |
| Delta | Chênh lệch mua–bán chủ động | Khối lượng khớp tại ask trừ khối lượng khớp tại bid theo quy ước phổ biến. |
| Footprint | Biểu đồ dấu chân lệnh | Hiển thị khối lượng giao dịch tại bid/ask ở từng mức giá. |
| Volume Profile | Hồ sơ khối lượng | Phân bố khối lượng đã giao dịch theo mức giá. |

---

# 1. Vấn đề mở đầu: volume bán rất lớn nhưng giá không giảm

ABC đang giao dịch quanh 100. Trong 5 phút:

- 300,000 cổ phiếu khớp tại bid 99.9–100.0.
- Delta là -220,000 cổ phiếu.
- Giá thấp nhất chỉ là 99.8.
- Mỗi lần bid 99.9 bị ăn, lượng mua mới lại xuất hiện.
- Sau đó giá tăng lên 100.6.

Trước khi đọc tiếp, hãy tự hỏi:

1. Ai đang chủ động bán?
2. Nếu bán chủ động lớn, tại sao giá không giảm tương xứng?
3. Ai đang đứng phía đối diện?
4. Đây là người mua hấp thụ hay người bán đã cạn lực?
5. Bằng chứng tiếp theo nào phân biệt hai cơ chế?

```text
MARKET SELL TÍCH LŨY: 300,000 CP
              ↓ đánh vào
BID 99.9–100.0 LIÊN TỤC BỊ TIÊU THỤ
              ↓ nhưng
BID ĐƯỢC BỔ SUNG + GIÁ CHỈ XUỐNG 99.8
              │
       ┌──────┴──────────────┐
       │                     │
BUYER HẤP THỤ          SELLER CẠN LỰC
bid tiếp tục đứng       bán mới giảm dần
       │                     │
       └──── cần dữ liệu tiếp theo ────┘
                     ↓
              GIÁ TĂNG LÊN 100.6
```

**Cách đọc:** bắt đầu từ lượng bán chủ động. Việc giá không giảm là dữ kiện; hai nhánh là giả thuyết. Nếu bid liên tục được bổ sung trong lúc bán vẫn mạnh, hấp thụ được ủng hộ. Nếu bán giảm trước khi giá quay lên, cạn lực có thể giải thích tốt hơn.

Phát biểu nhân quả: **giá không giảm dù market sell lớn khi thanh khoản mua được bổ sung đủ nhanh hoặc khi lực bán mới suy yếu; chỉ nhìn delta âm không phân biệt được hai cơ chế.**

> **Ghi nhớ:** nỗ lực lớn mà kết quả nhỏ là câu hỏi, chưa phải đáp án “hấp thụ”.

---

# 2. WHY — Tại sao cần khái niệm dòng lệnh?

Giá cuối chỉ cho biết giao dịch gần nhất. Volume chỉ cho biết bao nhiêu đã đổi chủ. Ta vẫn chưa biết:

- Bên nào yêu cầu khớp ngay?
- Lực chủ động có kéo dài không?
- Thanh khoản đối diện bị quét hay được bổ sung?
- Giá tiến triển tương xứng với nỗ lực không?

Nếu không có tư duy dòng lệnh, ta dễ rút gọn sai:

> Volume tăng = có dòng tiền mua.

Mọi giao dịch luôn có người mua và người bán. Điều cần tìm không phải “có mua hay không”, mà là **ai từ bỏ quyền chờ để chấp nhận giá phía đối diện**, và bên kia có đủ khả năng hấp thụ không.

---

# 3. WHAT — Bản chất là gì?

## Tầng 1 — Trực giác

Hãy hình dung một cánh cửa. Người chủ động đang cố đi qua; thanh khoản đối diện là lực giữ cửa. Chỉ biết có bao nhiêu người đẩy chưa đủ; phải xem cửa có di chuyển không.

## Tầng 2 — Định nghĩa chuẩn

**Dòng lệnh (order flow)** là chuỗi lệnh, hủy lệnh và giao dịch thể hiện áp lực mua/bán chủ động cùng phản ứng của thanh khoản thụ động theo thời gian.

## Tầng 3 — First Principles

```text
ĐỘNG CƠ / RÀNG BUỘC
        ↓
CHỌN CHỦ ĐỘNG HAY THỤ ĐỘNG
        ↓
LỆNH GẶP THANH KHOẢN ĐỐI DIỆN
        ↓
TIÊU THỤ / BỔ SUNG / RÚT LỆNH
        ↓
GIÁ TIẾN TRIỂN HOẶC BỊ HẤP THỤ
```

**Cách đọc:** động cơ tạo lựa chọn loại lệnh. Kết quả giá phụ thuộc đồng thời vào lực chủ động và phản ứng thanh khoản, không chỉ một phía.

Phát biểu nhân quả: **dòng lệnh tạo chuyển động giá khi lệnh chủ động kéo dài hơn khả năng hấp thụ và bổ sung của phía đối diện.**

> **Ghi nhớ:** dòng lệnh là cuộc tương tác giữa người đòi khớp ngay và người sẵn sàng đứng chờ.

## Dòng lệnh không phải là gì?

- Không phải tổng volume.
- Không phải delta dương thì chắc chắn giá tăng.
- Không phải cách biết chắc actor nào giao dịch.
- Không phải tín hiệu “thấy imbalance thì mua”.
- Không phải toàn bộ ý định thị trường; dữ liệu chỉ ghi phần đã hiển thị hoặc đã khớp.

---

# 4. MECHANISM — Dòng lệnh tác động tới giá như thế nào?

## 4.1 Chủ động và thụ động

```text
                       MỘT GIAO DỊCH
                            │
             ┌──────────────┴──────────────┐
             │                             │
      PHÍA CHỦ ĐỘNG                   PHÍA THỤ ĐỘNG
      chấp nhận giá khác             đặt giá và chờ
             │                             │
market buy → đánh ask          limit sell → cung cấp ask
market sell → đánh bid         limit buy  → cung cấp bid
```

**Cách đọc:** giao dịch luôn có cả hai phía. “Buyer initiated” nghĩa là người mua chủ động chấp nhận ask, không có nghĩa chỉ tồn tại người mua.

Phát biểu nhân quả: **giao dịch tại ask thường cho thấy bên mua chủ động; giao dịch tại bid thường cho thấy bên bán chủ động, nhưng kết quả giá còn phụ thuộc lượng thanh khoản thụ động phía đối diện.**

> **Ghi nhớ:** chủ động nói về ai vượt spread, không nói ai thông minh hơn.

## 4.2 Giao dịch tại bid/ask

Giả sử best bid 99.9, best ask 100.0:

- Market buy 10,000 khớp tại ask 100.0: buyer initiated.
- Market sell 8,000 khớp tại bid 99.9: seller initiated.
- Limit buy 100.1 có thể khớp ngay và cũng được xem là mua chủ động.

Việc phân loại có thể sai nếu dữ liệu bị trễ, giao dịch ngoài sổ hoặc thuật toán phân loại chỉ suy đoán từ quote.

## 4.3 Mất cân bằng lệnh

**Mất cân bằng (imbalance)** chỉ có nghĩa trong một phạm vi: tại mức giá, cây nến, khoảng thời gian hoặc chuỗi giao dịch.

```text
BUY IMBALANCE 80,000 CP
          ↓
ASK LIQUIDITY 20,000 CP
          ↓ ask bị quét
GIÁ ĐI LÊN MỨC MỚI

BUY IMBALANCE 80,000 CP
          ↓
ASK LIQUIDITY BỔ SUNG > 80,000 CP
          ↓ lực mua bị hấp thụ
GIÁ ÍT TIẾN TRIỂN
```

**Cách đọc:** cùng lượng mua chủ động tạo hai kết quả khác nhau vì khả năng bổ sung phía ask khác nhau.

Phát biểu nhân quả: **imbalance chỉ đẩy giá khi nó vượt khả năng hấp thụ trong phạm vi đang xét; imbalance lớn không bảo đảm price progress lớn.**

> **Ghi nhớ:** imbalance là lực; price progress cho biết vật cản có chịu thua không.

## 4.4 Hấp thụ và cạn lực

```text
GIÁ KHÔNG TIẾP DIỄN
        │
   ┌────┴──────────────────┐
   │                       │
HẤP THỤ                 CẠN LỰC
Absorption              Exhaustion
   │                       │
lực chủ động vẫn lớn    lực chủ động giảm
phía đối diện đứng lại  không cần vật cản lớn
   │                       │
volume có thể cao       volume thường suy yếu
```

**Cách đọc:** hai cơ chế có cùng kết quả bề ngoài. Hấp thụ cần bằng chứng phía đối diện tiếp tục nhận lệnh; cạn lực cần bằng chứng nguồn lệnh chủ động giảm.

Phát biểu nhân quả: **giá dừng vì phía đối diện đủ mạnh là hấp thụ; giá dừng vì phía chủ động hết lực là cạn lực. Hai cơ chế có thể đồng thời xảy ra.**

> **Ghi nhớ:** hấp thụ hỏi “ai đứng lại”; cạn lực hỏi “ai ngừng đẩy”.

## 4.5 Ví dụ số nhiều bước

Ask đang có:

| Ask | Khối lượng ban đầu | Bổ sung |
|---:|---:|---:|
| 100.0 | 20,000 | 60,000 |
| 100.1 | 30,000 | 0 |

Market buy tổng cộng 70,000:

```text
MARKET BUY 70,000
       ↓
ASK 100.0 có 20,000 → dùng hết → còn mua 50,000
       ↓ seller bổ sung 60,000 tại 100.0
ASK 100.0 dùng thêm 50,000 → còn lại 10,000 ask
       ↓
LỆNH MUA HOÀN TẤT; KHÔNG CẦN KHỚP 100.1
GIÁ CUỐI = GIÁ TB = 100.0
```

**Cách đọc:** nếu chỉ nhìn snapshot ban đầu, ta dự đoán giá phải lên 100.1. Nhưng thanh khoản động được bổ sung đã hấp thụ phần còn lại.

Phát biểu nhân quả: **resiliency cao có thể làm một mức giá hấp thụ lượng lệnh lớn hơn nhiều so với depth hiển thị ban đầu.**

> **Ghi nhớ:** order book là ảnh chụp; order flow là bộ phim.

### Thay đổi một biến

Nếu không có 60,000 bổ sung, 50,000 còn lại sẽ dùng 30,000 ở 100.1 và vẫn thiếu 20,000 ngoài phần sổ lệnh đã cho. Giá cuối chưa xác định.

## 4.6 Price + Volume: không học thành bốn quy tắc

| Giá và volume | Cơ chế có thể | Cần kiểm tra thêm |
|---|---|---|
| Giá ↑, volume ↑ | Mua chủ động kéo dài; short covering; tin tức | Ask có bị ăn? Giá có giữ? |
| Giá ↑, volume ↓ | Sổ lệnh mỏng; thiếu seller; drift | Spread/depth, mức chấp nhận giá mới |
| Giá ↓, volume ↑ | Bán chủ động; forced selling; bid rút | Có absorption hay panic? |
| Giá ↓, volume ↓ | Thiếu cầu; drift; người mua đứng ngoài | Có bán chủ động thật sự không? |

Correlation không phải mechanism. Mỗi tổ hợp chỉ mở ra tập giả thuyết.

## 4.7 Delta, Footprint và Volume Profile

- **Delta** đo chênh lệch volume phân loại tại ask và bid; không đo toàn bộ nhu cầu, lệnh chưa khớp hay danh tính actor.
- **Footprint** cho thấy phân bố giao dịch tại giá; không tự xác nhận hấp thụ nếu thiếu price response và bối cảnh.
- **Volume Profile** cho biết nơi đã giao dịch nhiều/ít; không phải bản đồ chắc chắn của liquidity tương lai.

```text
DỮ LIỆU CÔNG CỤ
Delta / Footprint / Volume Profile
             ↓ đo một phần
GIAO DỊCH ĐÃ XẢY RA THEO PHÍA / GIÁ
             ↓ không trực tiếp đo
Ý ĐỊNH + LỆNH CHƯA KHỚP + ACTOR + LIQUIDITY TƯƠNG LAI
```

**Cách đọc:** công cụ nằm giữa dữ liệu và suy luận. Không được nâng một phép đo thành sự thật về ý định.

> **Ghi nhớ:** công cụ đo dấu vết; cơ chế vẫn phải được kiểm chứng.

---

# 5. ACTORS — Ai tham gia?

- Retail: ưu tiên khác nhau; có thể chủ động hoặc thụ động.
- Tổ chức/quỹ: chia nhỏ lệnh, quan tâm market impact và benchmark.
- Market maker: cung cấp hai phía, quản lý inventory và adverse selection.
- Prop trader/hedge fund: tìm lợi thế ngắn hoặc trung hạn.
- Algorithm: thực thi nhanh, chia lệnh, phản ứng với quote.
- Arbitrageur: giao dịch sai lệch tương đối giữa tài sản.
- Người bị buộc giao dịch: stop, margin call, thanh lý, hedging.

Luôn hỏi: **Ai ở phía đối diện?** Một market buy lớn cần seller thụ động; một market sell lớn cần buyer thụ động.

---

# 6. INCENTIVES — Mỗi bên muốn gì?

| Actor | Mục tiêu | Ràng buộc/chi phí | Rủi ro | Khung thời gian |
|---|---|---|---|---|
| Người chủ động | Khớp ngay | Spread, slippage, impact | Giá chạy trước khi khớp đủ | Ngắn |
| Người thụ động | Giá tốt hơn | Chờ, có thể không khớp | Adverse selection | Linh hoạt |
| Market maker | Kiếm spread, cân inventory | Vốn, quote obligation | Tin mới, inventory lệch | Rất ngắn |
| Tổ chức | Hoàn tất size gần benchmark | Liquidity, rò rỉ thông tin | Opportunity cost | Dài hơn |
| Forced trader | Giảm rủi ro bắt buộc | Ít quyền chọn thời điểm | Trượt giá mạnh | Khẩn cấp |

Nếu bạn phải bán 2 triệu cổ phiếu, bạn sẽ không chỉ hỏi giá sẽ giảm hay không. Bạn phải hỏi bid ở đâu, bán nhanh hay chậm, người khác có phát hiện pattern thực thi và rút liquidity không.

---

# 7. EVIDENCE — Ta quan sát được gì?

## Quan sát trực tiếp

- Giá, volume, bid/ask, spread.
- Giao dịch theo thời gian và mức giá.
- Depth hiển thị và thay đổi quote.
- Delta/footprint nếu nguồn dữ liệu hỗ trợ.
- Price progress sau một lượng giao dịch.
- Bối cảnh thị trường, ngành, tin tức.

## Suy luận

- Buyer/seller đang chủ động chiếm ưu thế.
- Có absorption hoặc exhaustion.
- Có iceberg/hidden liquidity.
- Một tổ chức đang chia nhỏ lệnh.

## Nếu giả thuyết hấp thụ mua đúng

Ta kỳ vọng market sell tiếp tục nhưng giá khó giảm, bid được bổ sung, sau đó giá lấy lại mức gần và seller mất khả năng tạo đáy mới.

---

# 8. ALTERNATIVE EXPLANATIONS

Hiện tượng: delta âm lớn, volume cao, giá đi ngang rồi tăng.

```text
DELTA ÂM LỚN + GIÁ KHÔNG GIẢM
                 │
       ┌─────────┼─────────┬──────────────┐
       │         │         │              │
BUYER HẤP THỤ  SELLER CẠN  PHÂN LOẠI SAI  ARBITRAGE/HEDGE
       │         │         │              │
cần bid bổ sung  cần bán giảm cần kiểm data cần context liên thị trường
```

| Giả thuyết | Cơ chế | Ủng hộ | Làm yếu | Nếu đúng, tiếp theo |
|---|---|---|---|---|
| Buyer hấp thụ | Bid nhận bán lớn | Bid replenish, giá giữ | Bid rút, giá thủng | Giá có thể tăng khi bán giảm |
| Seller cạn lực | Nguồn bán suy yếu | Sell volume giảm dần | Bán vẫn lớn liên tục | Giá hồi nhưng không nhất thiết có buyer lớn |
| Phân loại dữ liệu sai | Trades gán sai bid/ask | Quote trễ, feed khác biệt | Dữ liệu chuẩn nhiều nguồn | Delta mất giá trị giải thích |
| Hedge/arbitrage | Một chân giao dịch nằm nơi khác | Tài sản liên quan đồng thời | Không có quan hệ liên thị trường | Price response theo spread tương đối |

---

# 9. FALSIFICATION

Giả thuyết: buyer lớn đang hấp thụ tại 100.

**Bằng chứng ủng hộ:** bán tại bid lớn; 100 giữ; bid bổ sung; giá bật khi bán giảm.

**Bằng chứng làm yếu:** bid biến mất; giá giảm tương xứng; volume bán đơn giản suy kiệt; dữ liệu phân loại sai; tin tức chung giải thích tốt hơn.

Thiên kiến dễ mắc: thấy volume lớn rồi gán tổ chức; chỉ chọn đoạn dữ liệu hợp câu chuyện; bỏ qua khung thời gian; xem divergence là tín hiệu chắc chắn.

---

# 10. APPLICATION

```text
OBSERVE → INTERPRET → HYPOTHESIZE → PREDICT → TEST → UPDATE → DECIDE
```

Ví dụ: volume bán lớn tại 100 nhưng giá giữ.

- Observe: trades tại bid, delta âm, low không mở rộng.
- Interpret: nỗ lực bán lớn nhưng kết quả giảm nhỏ.
- Hypothesize: absorption; exhaustion; lỗi phân loại.
- Predict: absorption cần bid replenish và giá bật khi bán còn hoạt động.
- Test: theo dõi Time & Sales, depth, retest và context.
- Update: tăng/giảm xác suất từng giả thuyết.
- Decide: chờ, không giao dịch, hoặc hành động với invalidation rõ.

Order flow hữu ích ở phạm vi dữ liệu chi tiết và ngắn hạn. Nó kém hữu ích khi feed không đầy đủ, thị trường phân mảnh hoặc khung quyết định dài hạn không phụ thuộc microstructure tức thời.

---

# 11. FACT → INFERENCE → STORY

```text
FACT                         INFERENCE                   STORY
300k khớp tại bid            có thể có hấp thụ          “quỹ X đang gom”
giá chỉ giảm 0.2             hoặc seller cạn lực        “MM cố tình giữ giá”
CHẮC NHẤT                    CẦN KIỂM CHỨNG              CHƯA ĐỦ BẰNG CHỨNG
```

1. “Delta -220k” là Fact nếu phép đo đáng tin; “buyer hấp thụ” là Inference; “cá mập gom” là Story.
2. “Ask được bổ sung 5 lần” là Fact; “iceberg seller” là Inference; “tổ chức xả” là Story.
3. “Giá tăng với volume giảm” là Fact; “thiếu seller” là Inference; “smart money kéo giá” là Story.

---

# 12. CASE STUDIES

## Case A — Hấp thụ tương đối rõ

**Facts:** market sell lớn; bid 50 liên tục bổ sung; giá không thủng; sau đó tăng.

**Mechanism:** seller chủ động → buyer thụ động tiếp nhận → bid giữ → seller giảm → giá hồi.

**Actors/Incentives:** seller cần thoát; buyer chấp nhận tích lũy; market maker có thể trung gian.

**Hypotheses:** absorption; exhaustion; hidden order.

**Evidence/Falsification:** replenish và retest giữ ủng hộ; thủng 50 với bán tiếp diễn bác bỏ.

**Conclusion:** absorption có xác suất cao, nhưng danh tính buyer chưa biết.

## Case B — Trông giống hấp thụ nhưng là cạn lực

**Facts:** bán lớn ban đầu, volume giảm nhanh, giá đứng rồi hồi; không thấy bid bổ sung đáng kể.

**Mechanism:** nguồn bán kết thúc → không còn áp lực → giá hồi bằng lượng mua vừa phải.

**Actors/Incentives:** seller ngắn hạn hoàn tất thoát vị thế; buyer phản ứng khi áp lực giảm.

**Hypotheses/Evidence:** exhaustion được ủng hộ bởi sell flow suy giảm; absorption yếu vì thiếu replenish.

**Falsification:** nếu market sell vẫn lớn và bid liên tục tái xuất hiện, absorption mạnh hơn.

**Conclusion:** price response giống Case A nhưng không cần buyer lớn hấp thụ.

## Case C — Mơ hồ do dữ liệu phân mảnh

**Facts:** delta âm ở một feed, giá tăng; thị trường có nhiều venue.

**Hypotheses:** absorption; trades ngoài feed; hedge; phân loại sai.

**Mechanism/Actors/Incentives:** flow tại venue quan sát có thể chỉ là một chân hedge hoặc một phần execution; actor tối ưu toàn danh mục, không riêng mã/feed này.

**Evidence:** cần consolidated feed, quote timing, tài sản liên quan và phương pháp phân loại.

**Falsification:** một giả thuyết yếu khi dữ liệu hợp nhất cho thấy dấu vết dự báo của nó không tồn tại.

**Conclusion:** không đủ evidence để kết luận order flow toàn thị trường.

---

# 13. Câu hỏi Socratic

1. Tại sao mọi giao dịch có buyer và seller nhưng vẫn nói buyer initiated?
2. Market buy tác động vào phía nào?
3. Delta dương nhưng giá giảm có thể xảy ra bằng cơ chế nào?
4. Volume bán lớn, giá đứng yên: absorption khác exhaustion ở đâu?
5. Tại sao snapshot depth không đủ để đánh giá absorption?
6. Ai ở phía đối diện của market sell lớn?
7. Nếu ask liên tục replenish, bạn kỳ vọng price progress thế nào?
8. Cơ chế nào tạo divergence giữa delta và giá?
9. Volume Profile không cho biết điều gì về liquidity tương lai?
10. Bằng chứng nào làm giả thuyết buyer absorption sai?

## Đáp án và reasoning

1. “Initiated” nói bên chấp nhận giá đối diện để khớp ngay.
2. Ask liquidity.
3. Buyer chủ động bị seller thụ động hấp thụ; phân loại sai; giao dịch liên thị trường.
4. Absorption cần phía đối diện đứng lại; exhaustion là phía chủ động suy yếu.
5. Depth thay đổi và có hidden/replenished liquidity.
6. Buyer thụ động hoặc actor cung cấp bid.
7. Giá có thể tiến triển ít dù volume mua lớn.
8. Hấp thụ, dữ liệu phân mảnh, hedging, thời điểm tổng hợp khác nhau.
9. Lệnh chưa đặt, hidden liquidity, ý định và phản ứng tương lai.
10. Bid rút, giá thủng với bán tiếp diễn, hoặc bán đã cạn trước phản ứng.

---

# 14. Kiểm tra “thực sự hiểu”

## Reverse reasoning

Giá đứng yên, volume rất lớn. Nêu ít nhất bốn cơ chế: hai phía lớn giao dịch; absorption; churn do tin tức; market maker inventory transfer; dữ liệu tổng hợp.

## What-if

Nếu market sell không đổi nhưng bid replenish tăng, price impact giảm. Nếu replenish biến mất, giá phải tìm bid thấp hơn.

## Counterexample

Delta dương rất lớn nhưng giá giảm vì seller thụ động liên tục bổ sung ask rồi bid phía dưới rút.

## Teach-back

> Dòng lệnh hỏi ai đang đòi giao dịch ngay. Giá chỉ đi xa nếu người đứng chờ phía đối diện không đủ sức nhận lượng đó.

---

# 15. Bản đồ liên kết

```text
PHẦN 2 — THANH KHOẢN             PHẦN 3 — DÒNG LỆNH             PHẦN 4 — THỰC THI
sức chứa theo giá         →      lực dùng sức chứa        →      cách chia lệnh lớn
depth/resiliency                 aggressive/passive              impact/benchmark/time
                                 absorption/exhaustion
```

**Cách đọc:** Phần 2 cung cấp vật cản, Phần 3 nghiên cứu lực và phản ứng, Phần 4 giải thích cách actor lớn điều khiển tốc độ của lực đó.

> **Ghi nhớ:** liquidity là sức chứa; order flow là dòng đang chảy; execution là cách kiểm soát dòng lớn.

---

# 16. Gate 3

Bạn đạt Gate 3 nếu có thể phân tích:

> **Volume tăng đột biến nhưng giá gần như đứng yên.**

Câu trả lời phải có: phía chủ động; thanh khoản đối diện; absorption vs exhaustion; dữ liệu quan sát; giả thuyết thay thế; dự đoán tiếp theo; điều kiện bác bỏ.

---

# 17. Kết thúc bài

## 1. First-Principles Summary

1. Dòng lệnh là tương tác giữa lệnh chủ động và thanh khoản thụ động theo thời gian.
2. Giao dịch tại ask/bid giúp suy luận phía chủ động nhưng không cho biết chắc danh tính.
3. Imbalance chỉ làm giá tiến triển khi vượt khả năng hấp thụ.
4. Absorption và exhaustion có thể tạo cùng kết quả bề ngoài nhưng khác cơ chế.
5. Delta, footprint và profile là phép đo, không phải câu trả lời về ý định.

## 2. Mental Model

**Lực đẩy → vật cản → mức di chuyển.** Luôn đo cả nỗ lực lẫn kết quả.

## 3. Không được nhầm

- Delta dương với chắc chắn bullish.
- Volume cao với buying pressure.
- Giá đứng với absorption chắc chắn.
- Footprint với toàn bộ thị trường.
- Divergence với tín hiệu giao dịch tự động.

## 4. Tôi đã hiểu nếu...

- Phân biệt chủ động/thụ động và bid/ask transactions.
- Giải thích imbalance theo phạm vi.
- Phân biệt absorption/exhaustion.
- Nêu giới hạn của delta, footprint, profile.
- Xây nhiều giả thuyết và falsification từ cùng dữ kiện.

## 5. Cầu nối sang bài tiếp theo

Nếu một actor có lệnh quá lớn để gửi một lần, họ phải kiểm soát order flow của chính mình. Phần 4 nghiên cứu cách tổ chức chia lệnh và cân bằng **market impact ↔ thời gian ↔ rò rỉ thông tin ↔ rủi ro không hoàn tất**.
