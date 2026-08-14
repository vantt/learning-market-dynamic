# Bài học Phần 1 — Market Microstructure

## Câu hỏi trung tâm

**Một lệnh mua/bán thực sự biến thành chuyển động giá bằng cách nào?**

Bài học này không bắt đầu từ chart pattern, nến, indicator hay câu chuyện “cá mập kéo giá”. Ta bắt đầu từ thứ nhỏ nhất có thể quan sát và suy luận: **người tham gia thị trường gửi lệnh; lệnh tương tác với lệnh khác; giao dịch xảy ra tại một mức giá; chuỗi giao dịch đó tạo thành giá thị trường**.

Nếu hiểu phần này, bạn sẽ trả lời được câu hỏi “tại sao giá tăng?” mà không cần nói “vì chart đẹp”, “vì dòng tiền vào”, hoặc “vì cá mập gom”. Bạn sẽ có thể phân rã thành cơ chế:

**Market buy orders → consume available ask liquidity → sellers tại mức giá hiện tại giảm → lệnh mua tiếp theo phải khớp với ask cao hơn → transaction price tăng → last price tăng.**

---

# 1. Problem mở đầu: Vì sao một lệnh mua có thể làm giá tăng?

Giả sử cổ phiếu ABC đang có sổ lệnh bán như sau:

| Giá bán (Ask) | Khối lượng đang chờ bán |
|---:|---:|
| 100.0 | 1,000 cổ phiếu |
| 100.1 | 2,000 cổ phiếu |
| 100.2 | 3,000 cổ phiếu |
| 100.3 | 5,000 cổ phiếu |

Một người muốn mua ngay 4,500 cổ phiếu ABC. Người đó không đặt điều kiện “chỉ mua nếu giá tốt”; họ gửi **lệnh mua thị trường (market buy order)**.

Trước khi đọc tiếp, hãy tự hỏi:

1. Lệnh mua 4,500 cổ phiếu sẽ khớp với ai?
2. Tất cả 4,500 cổ phiếu có khớp ở giá 100.0 không?
3. “Giá thị trường” sau giao dịch sẽ là 100.0, 100.1, 100.2 hay một con số khác?
4. Có phải giá tăng vì người mua “muốn đẩy giá lên”, hay vì cơ chế khớp lệnh buộc giao dịch xảy ra ở các mức ask cao hơn?
5. Nếu tại 100.0 có 100,000 cổ phiếu chờ bán thay vì 1,000, cùng lệnh mua 4,500 cổ phiếu có làm giá tăng giống vậy không?

Bây giờ phân rã:

- Người mua cần 4,500 cổ phiếu ngay.
- Ở ask 100.0 chỉ có 1,000 cổ phiếu chờ bán.
- 1,000 cổ phiếu đầu tiên khớp tại 100.0.
- Còn 3,500 cổ phiếu chưa mua xong.
- Mức ask 100.0 đã bị “ăn hết”.
- Lệnh mua tiếp tục khớp với 2,000 cổ phiếu tại 100.1.
- Còn 1,500 cổ phiếu chưa mua xong.
- Lệnh mua tiếp tục khớp với 1,500 trong 3,000 cổ phiếu tại 100.2.
- Giao dịch cuối cùng xảy ra tại 100.2.

Kết quả:

| Giá khớp | Khối lượng khớp |
|---:|---:|
| 100.0 | 1,000 |
| 100.1 | 2,000 |
| 100.2 | 1,500 |

**Last price** sau lệnh này là 100.2, không phải vì thị trường “tự chạy”, mà vì lệnh mua ngay đã tiêu thụ hết người bán sẵn sàng bán ở giá thấp hơn.

Concept xuất hiện tự nhiên từ vấn đề này là **Market Microstructure**: cơ chế vi mô bên dưới việc lệnh được gửi, xếp hàng, khớp, hủy, và biến thành giá giao dịch.

---

# 2. WHY — Tại sao Market Microstructure phải tồn tại?

Thị trường tài chính không phải một cái bảng giá tự động thay đổi theo cảm xúc. Nó là một hệ thống xử lý xung đột giữa nhiều người:

- Người muốn mua ngay.
- Người muốn bán ngay.
- Người chỉ muốn mua nếu giá đủ thấp.
- Người chỉ muốn bán nếu giá đủ cao.
- Người cung cấp thanh khoản để kiếm spread.
- Người cần thoát vị thế vì rủi ro.
- Người có thông tin mới và muốn hành động trước.
- Thuật toán gửi/hủy lệnh liên tục.

Vấn đề thực tế là: **làm sao chuyển rất nhiều ý định giao dịch khác nhau thành giao dịch cụ thể, công bằng, có thứ tự, có giá và có khối lượng?**

Nếu không có microstructure:

- Không rõ ai được khớp trước khi nhiều người cùng mua/bán.
- Không rõ giá nào được dùng khi người mua và người bán không đồng thuận.
- Không rõ điều gì xảy ra khi khối lượng muốn mua ngay lớn hơn khối lượng muốn bán ở giá hiện tại.
- Không rõ vì sao cùng một lệnh 1 tỷ đồng có thể gần như không làm giá VNM dịch chuyển nhưng có thể làm cổ phiếu kém thanh khoản tăng mạnh.
- Không thể phân biệt giá tăng do thông tin, do thiếu thanh khoản, do forced buying, hay do một lệnh lớn quét qua order book.

Market microstructure tồn tại vì thị trường cần một cơ chế để trả lời:

**Ai muốn giao dịch? Giao dịch với ai? Tại giá nào? Với bao nhiêu khối lượng? Theo thứ tự nào? Và nếu không đủ đối ứng ở giá hiện tại thì chuyện gì xảy ra?**

---

# 3. WHAT — Bản chất là gì?

## Tầng 1 — Intuition

Hãy hình dung thị trường như một cái chợ có bảng rao mua và rao bán.

- Người mua treo giá: “Tôi sẵn sàng mua 1,000 cổ phiếu ở 99.9.”
- Người bán treo giá: “Tôi sẵn sàng bán 1,000 cổ phiếu ở 100.0.”
- Người cần giao dịch ngay sẽ chấp nhận giá tốt nhất đang có ở phía đối diện.
- Nếu họ mua nhiều hơn lượng đang được bán ở giá tốt nhất, họ phải mua tiếp ở giá cao hơn.

Giá không tăng vì bảng giá “quyết định tăng”. Giá tăng vì **những người bán rẻ nhất đã bị tiêu thụ**, nên giao dịch tiếp theo phải xảy ra với người bán đòi giá cao hơn.

## Tầng 2 — Standard

**Market Microstructure** là lĩnh vực nghiên cứu cách thị trường tổ chức giao dịch: cách orders được gửi vào hệ thống, được ưu tiên, được khớp, bị hủy, tạo thành giá giao dịch, thanh khoản, spread, volatility và market impact.

Trong phần 1, ta tập trung vào:

- **Exchange**: nơi tổ chức giao dịch.
- **Broker**: trung gian đưa lệnh của khách hàng vào thị trường.
- **Matching engine**: hệ thống khớp lệnh.
- **Market order**: lệnh muốn khớp ngay với giá tốt nhất hiện có.
- **Limit order**: lệnh chỉ khớp tại mức giá giới hạn hoặc tốt hơn.
- **Bid/Ask/Spread**: giá mua tốt nhất, giá bán tốt nhất, và chênh lệch giữa chúng.
- **Limit Order Book**: sổ lệnh giới hạn đang chờ khớp.
- **Transaction price / Last price**: giá giao dịch thực sự xảy ra gần nhất.
- **Market impact / Slippage**: tác động của lệnh lên giá và chênh lệch giữa giá kỳ vọng với giá khớp thực tế.

## Tầng 3 — First Principles

Ở mức cơ bản nhất, thị trường gồm:

**Participants → Incentives → Orders → Order Book → Matching → Transactions → Price**

Trong đó:

- **Participants** là người hoặc thuật toán tham gia.
- **Incentives** là lý do họ muốn mua/bán, chờ/đánh ngay, giấu/hiện size, vào/thoát vị thế.
- **Orders** là cách ý định được chuyển thành chỉ thị giao dịch.
- **Order Book** là trạng thái tạm thời của các lệnh đang chờ.
- **Matching** là cơ chế ghép lệnh mua và bán tương thích.
- **Transactions** là giao dịch thực sự đã xảy ra.
- **Price** là dấu vết của các giao dịch đó, không phải nguyên nhân đầu tiên.

## Market Microstructure không phải là gì?

- Không phải một indicator.
- Không phải một chiến lược “thấy X thì mua”.
- Không phải công cụ đọc chắc ý đồ “cá mập”.
- Không phải lời giải thích duy nhất cho mọi biến động giá.
- Không phải chart pattern ở cấp thấp hơn; nó là cơ chế tạo ra price/volume mà chart ghi lại.

---

# 4. MECHANISM — Lệnh biến thành giá bằng cách nào?

Đây là phần quan trọng nhất.

## 4.1 Các thành phần của thị trường

Một lệnh đơn giản thường đi qua chuỗi:

**Trader → Broker → Exchange → Matching Engine → Counterparty → Clearing → Custody**

Giải thích từng mắt xích:

- **Trader / Investor**: người ra quyết định mua/bán.
- **Broker**: nhận lệnh từ khách hàng, kiểm tra điều kiện tài khoản, gửi lệnh đến sàn hoặc venue phù hợp.
- **Exchange**: nơi tập trung và tổ chức giao dịch theo luật.
- **Matching engine**: hệ thống so khớp lệnh mua và bán.
- **Counterparty**: phía đối diện của giao dịch. Nếu bạn mua, luôn có ai đó bán.
- **Clearing**: đối chiếu và bảo đảm nghĩa vụ thanh toán/giao hàng.
- **Custody**: ghi nhận quyền sở hữu tài sản sau khi giao dịch hoàn tất.

Câu hỏi bắt buộc: **Who is on the other side?**

Nếu bạn mua 1,000 cổ phiếu, không có chuyện “mua từ thị trường” theo nghĩa mơ hồ. Bạn mua từ một hoặc nhiều người bán cụ thể đang cung cấp cổ phiếu tại các mức giá cụ thể.

## 4.2 Orders — Các loại lệnh là cách biểu đạt incentive

### Market Order

**Market order** là lệnh ưu tiên khớp ngay, chấp nhận giá tốt nhất hiện có ở phía đối diện.

Mechanism:

**Need immediacy → send market order → hit existing liquidity → consume best bid/ask → maybe walk the book → transaction price changes**

Ví dụ:

Bạn mua ngay 3,000 cổ phiếu. Ask book:

| Ask | Khối lượng |
|---:|---:|
| 50.00 | 1,000 |
| 50.05 | 1,000 |
| 50.10 | 2,000 |

Lệnh mua 3,000 sẽ khớp:

- 1,000 tại 50.00
- 1,000 tại 50.05
- 1,000 tại 50.10

Giá khớp trung bình:

`(1,000×50.00 + 1,000×50.05 + 1,000×50.10) / 3,000 = 50.05`

Last price sau lệnh là 50.10.

Bạn muốn “mua ngay”, nên chi phí là có thể phải trả giá cao hơn mức ask đầu tiên.

### Limit Order

**Limit order** là lệnh chỉ khớp tại giá giới hạn hoặc tốt hơn.

Ví dụ:

- Limit buy 49.90: chỉ mua ở 49.90 hoặc thấp hơn.
- Limit sell 50.20: chỉ bán ở 50.20 hoặc cao hơn.

Mechanism:

**Willing to wait → post limit order → provide liquidity → join queue → wait for aggressive counterparty → either filled, partially filled, or canceled**

Limit order đổi lấy quyền kiểm soát giá bằng việc chấp nhận rủi ro không được khớp.

### Stop Order

**Stop order** là lệnh được kích hoạt khi giá chạm một mức xác định.

Ví dụ:

- Bạn đang nắm cổ phiếu ở 100, đặt stop-loss tại 95.
- Khi giá giao dịch chạm 95, lệnh bán được kích hoạt, thường trở thành market sell hoặc limit sell tùy loại.

Cơ chế quan trọng:

**Price reaches trigger → dormant order becomes active → new market/limit order enters market → can add aggression in direction of move**

Stop order không phải liquidity đang nằm sẵn trong order book như limit order thông thường. Nó là **conditional order**: chưa hoạt động cho đến khi trigger xảy ra.

### Stop-limit

**Stop-limit** có hai mức:

- Stop price: mức kích hoạt.
- Limit price: mức giá giới hạn sau khi kích hoạt.

Ưu điểm: tránh khớp quá xấu.

Rủi ro: nếu thị trường chạy nhanh qua limit price, lệnh có thể không khớp.

### Resting order

**Resting order** là lệnh đang nằm chờ trong order book, thường là limit order.

Resting orders tạo ra **visible liquidity** nếu được hiển thị.

### Aggressive vs Passive

Một lệnh là **aggressive** nếu nó yêu cầu khớp ngay và tiêu thụ thanh khoản phía đối diện.

Một lệnh là **passive** nếu nó nằm chờ và cung cấp thanh khoản.

Không nên nói đơn giản “buyer làm giá tăng”. Cần hỏi:

- Buyer đó là aggressive buyer hay passive buyer?
- Họ mua bằng market order hay đặt limit bid?
- Ask liquidity có đủ hấp thụ không?

## 4.3 Bid — Ask — Spread

Giả sử order book tốt nhất:

- Best bid: 99.9, khối lượng 5,000.
- Best ask: 100.0, khối lượng 3,000.

**Best bid** là giá mua cao nhất đang được treo.

**Best ask** là giá bán thấp nhất đang được treo.

**Spread** là:

`Best ask - Best bid = 100.0 - 99.9 = 0.1`

**Mid-price** là:

`(Best bid + Best ask) / 2 = 99.95`

### Tại sao spread tồn tại?

Spread tồn tại vì người mua và người bán không có cùng incentive.

Người mua thụ động muốn mua rẻ hơn. Người bán thụ động muốn bán đắt hơn. Market maker hoặc người cung cấp thanh khoản cũng cần được bù đắp cho:

- Rủi ro bị adverse selection: giao dịch với người có thông tin tốt hơn.
- Rủi ro inventory: sau khi mua/bán, họ còn giữ vị thế có thể lỗ.
- Chi phí vốn và vận hành.
- Rủi ro giá di chuyển trong lúc họ đang cung cấp thanh khoản.

Mechanism:

**Uncertainty + inventory risk + immediacy demand → liquidity provider requires compensation → quotes bid below ask → spread exists**

Nếu spread bằng 0 trong khi rủi ro tồn tại, người cung cấp thanh khoản có thể không được bù đủ. Họ sẽ rút lệnh, giảm size, hoặc quote rộng hơn.

## 4.4 Limit Order Book

**Limit Order Book (LOB)** là danh sách các lệnh giới hạn đang chờ khớp, chia theo mức giá.

Ví dụ:

| Bid size | Bid price | Ask price | Ask size |
|---:|---:|---:|---:|
| 4,000 | 99.8 | 100.0 | 1,000 |
| 6,000 | 99.7 | 100.1 | 2,000 |
| 8,000 | 99.6 | 100.2 | 5,000 |
| 10,000 | 99.5 | 100.3 | 8,000 |

### Price levels

Mỗi mức giá có thể có nhiều lệnh từ nhiều người khác nhau.

### Queue

Tại cùng một mức giá, lệnh thường được xếp hàng. Ai đặt trước được ưu tiên trước, tùy quy tắc venue.

### Depth

**Depth** là khối lượng có sẵn ở các mức giá.

Book sâu nghĩa là có nhiều liquidity ở nhiều mức giá. Book mỏng nghĩa là chỉ cần một lệnh market tương đối nhỏ cũng có thể ăn qua nhiều mức giá.

### Price-time priority

Quy tắc phổ biến:

1. Giá tốt hơn được ưu tiên trước.
2. Nếu cùng giá, lệnh đến trước được ưu tiên trước.

Ví dụ bên bán:

- Ask 100.0 được khớp trước ask 100.1.
- Trong ask 100.0, người đặt lệnh trước được khớp trước.

### Order cancellation

Order book không tĩnh. Lệnh có thể bị hủy bất cứ lúc nào nếu chưa khớp.

Điều này quan trọng vì liquidity nhìn thấy trong book có thể biến mất.

Mechanism:

**New information / price approaches / risk changes → liquidity provider cancels or revises quotes → available depth changes → market order impact changes**

Nếu bạn thấy 100,000 cổ phiếu chờ bán ở 100.0, không có nghĩa chắc chắn 100,000 cổ phiếu đó vẫn ở đó khi giá chạm 100.0.

## 4.5 Matching & Price Formation

Giá không “tự chạy”. Giá giao dịch thay đổi khi lệnh aggressive tương tác với liquidity có sẵn.

### Matching

Một market buy khớp với best ask.

Một market sell khớp với best bid.

Một limit order có thể khớp ngay nếu nó marketable.

Ví dụ:

- Best ask là 100.0.
- Bạn đặt limit buy 100.2.
- Vì bạn sẵn sàng mua tới 100.2, lệnh của bạn có thể khớp ngay với ask 100.0, rồi 100.1, rồi tối đa 100.2 nếu đủ điều kiện.

### Transaction price

**Transaction price** là giá thực sự của giao dịch đã khớp.

### Last price

**Last price** là giá của giao dịch gần nhất.

Last price không nhất thiết bằng “giá trị hợp lý”. Nó chỉ là dấu vết cuối cùng của giao dịch xảy ra.

### Price discovery

**Price discovery** là quá trình thị trường tìm mức giá mà tại đó có đủ người sẵn sàng giao dịch.

Causal chain:

**Information/incentive changes → participants update orders → aggressive orders consume liquidity → some price levels fail to absorb → trades occur at new levels → market observes new transaction prices → participants update beliefs again**

## 4.6 Market Impact và Slippage

**Market impact** là tác động của lệnh lên giá.

**Slippage** là chênh lệch giữa giá kỳ vọng và giá khớp thực tế.

Case:

Hai người cùng mua 1 tỷ đồng:

- Người A mua VNM, cổ phiếu thanh khoản cao.
- Người B mua cổ phiếu XYZ, thanh khoản rất thấp.

Tại sao tác động khác nhau?

Không phải vì 1 tỷ ở bên này “thông minh hơn”. Khác biệt nằm ở **size relative to liquidity**.

Ví dụ đơn giản:

### VNM giả định

Ask book:

| Ask | Giá trị chờ bán |
|---:|---:|
| 70.00 | 5 tỷ |
| 70.10 | 8 tỷ |
| 70.20 | 10 tỷ |

Lệnh mua 1 tỷ chỉ ăn một phần ask 70.00. Last price có thể vẫn là 70.00 hoặc dịch chuyển rất ít.

### XYZ giả định

Ask book:

| Ask | Giá trị chờ bán |
|---:|---:|
| 10.00 | 100 triệu |
| 10.20 | 150 triệu |
| 10.50 | 200 triệu |
| 11.00 | 300 triệu |
| 11.50 | 500 triệu |

Lệnh mua 1 tỷ có thể ăn qua nhiều mức giá. Last price có thể lên 11.50.

Mechanism:

**Same notional size → different available ask liquidity → different number of price levels consumed → different slippage → different market impact**

### Temporary impact vs Permanent impact

**Temporary impact**: giá dịch chuyển do lệnh ăn liquidity tạm thời, sau đó liquidity quay lại và giá hồi một phần.

**Permanent impact**: giá thay đổi bền hơn vì lệnh phản ánh thông tin mới, hoặc thị trường cập nhật lại nhận định về giá trị.

Không thể chỉ nhìn một cú tăng giá và kết luận ngay đó là permanent. Cần evidence tiếp theo:

- Giá có giữ được vùng mới không?
- Liquidity có tái xuất hiện ở vùng cũ không?
- Volume sau đó xác nhận tiếp tục hay chỉ là một cú quét mỏng?
- Có thông tin/catalyst mới không?

## 4.7 Volatility hình thành từ đâu?

**Volatility** là mức độ dao động giá. Từ microstructure, volatility có thể tăng khi:

- Liquidity mỏng.
- Spread rộng.
- Order imbalance kéo dài.
- Nhiều market orders cùng hướng.
- Tin tức làm participants hủy lệnh và quote lại.
- Stop/forced orders bị kích hoạt.
- Participants không đồng thuận mạnh về giá trị.

Causal chain:

**Uncertainty rises → passive liquidity withdraws/widens quotes → book becomes thinner → aggressive orders consume levels faster → transaction prices jump more → observed volatility increases**

Volatility không chỉ là “giá biến động mạnh”. Nó thường là kết quả của sự kết hợp giữa **order aggression** và **available liquidity**.

---

# 5. Thay đổi từng biến để hiểu nhân quả

## Nếu market order size tăng thì sao?

Giả sử ask book:

| Ask | Size |
|---:|---:|
| 100.0 | 1,000 |
| 100.1 | 2,000 |
| 100.2 | 3,000 |

Market buy 500:

- Chỉ khớp tại 100.0.
- Last price = 100.0.

Market buy 4,000:

- Ăn hết 1,000 tại 100.0.
- Ăn hết 2,000 tại 100.1.
- Ăn 1,000 tại 100.2.
- Last price = 100.2.

Kết luận đúng không phải “mua nhiều thì giá tăng”. Kết luận chính xác hơn:

**Nếu aggressive buy size lớn hơn ask liquidity ở các mức giá gần nhất, lệnh phải khớp lên các mức ask cao hơn, làm transaction price tăng.**

## Nếu ask liquidity tăng thì sao?

Ask 100.0 từ 1,000 tăng thành 10,000.

Market buy 4,000 giờ khớp toàn bộ tại 100.0.

Giá không cần tăng qua 100.0, dù có lực mua ngay.

Mechanism:

**More available ask liquidity at best ask → same buy order absorbed at current level → no need to trade higher → lower immediate price impact**

## Nếu liquidity biến mất thì sao?

Trước tin tức lớn, market makers và passive sellers có thể hủy lệnh vì không muốn bị giao dịch ở giá cũ.

Ask book từ:

| Ask | Size |
|---:|---:|
| 100.0 | 10,000 |
| 100.1 | 15,000 |

thành:

| Ask | Size |
|---:|---:|
| 100.0 | 500 |
| 100.5 | 1,000 |
| 101.5 | 2,000 |

Cùng market buy 3,000 giờ có thể đẩy last price lên 101.5.

Đây là lý do tin tức có thể làm giá nhảy mạnh: không chỉ vì nhiều người mua/bán, mà còn vì phía cung cấp liquidity rút bớt lệnh.

## Nếu market order không tiếp tục thì sao?

Một lệnh mua lớn có thể đẩy giá lên 100.2. Nhưng nếu sau đó không có lệnh mua mới, và sellers quay lại đặt ask dày hơn, giá có thể không tiếp tục tăng.

Mechanism:

**One-time liquidity consumption → price moves → no follow-through aggression → passive sellers replenish → price stalls or reverts**

Vì vậy, “giá vừa tăng” không tự động nghĩa là “xu hướng tăng đã hình thành”.

---

# 6. ACTORS — Ai đang tham gia?

## Retail trader/investor

- Có thể dùng market order vì muốn vào/thoát nhanh.
- Có thể đặt stop-loss tạo triggered orders.
- Thường size nhỏ so với liquidity của cổ phiếu lớn, nhưng có thể đáng kể trong cổ phiếu rất kém thanh khoản.

**Who is on the other side?** Có thể là retail khác, market maker, fund, algorithm, hoặc người bán limit order.

## Institutional investor / Mutual fund / Pension fund

- Size lớn.
- Bị ràng buộc bởi mandate, benchmark, risk limit, liquidity.
- Không thể tùy tiện dùng market order lớn nếu order book không đủ sâu.

Nếu họ cần mua lớn, vấn đề chính không phải “muốn kéo giá”, mà là:

**Làm sao mua đủ size mà không tự làm giá xấu đi quá nhiều?**

## Hedge fund / Prop trader

- Có thể giao dịch ngắn hạn hơn.
- Có thể dùng thông tin, model, statistical edge, event-driven thesis.
- Có thể vừa cung cấp liquidity vừa tiêu thụ liquidity tùy chiến lược.

## Market maker

- Cung cấp bid và ask.
- Kiếm spread hoặc rebate, quản lý inventory.
- Có rủi ro bị giao dịch bởi người có thông tin tốt hơn.

Market maker không nhất thiết “cố tình kéo giá”. Nhiều hành vi quote rộng, hủy lệnh, giảm size có thể là phản ứng hợp lý trước rủi ro.

## Algorithm

- Có thể là execution algo chia nhỏ lệnh lớn.
- Có thể là market-making algo.
- Có thể là arbitrage algo.
- Có thể phản ứng nhanh với order book, volume, price, news.

## Arbitrageur

- Tìm sai lệch giá giữa tài sản liên quan.
- Có thể mua một nơi, bán nơi khác.
- Góp phần đưa giá liên quan về cân bằng tương đối.

## Major shareholder

- Size rất lớn, time horizon dài.
- Nếu giao dịch, họ đối mặt với disclosure, liquidity, impact và signal risk.

---

# 7. INCENTIVES — Mỗi bên muốn gì?

## Người gửi market order

- **Objective**: khớp ngay.
- **Constraint**: có thể phải chấp nhận giá xấu.
- **Cost**: spread + slippage + impact.
- **Risk**: order book mỏng hơn tưởng tượng.
- **Information**: có thể biết ít hoặc nhiều hơn thị trường.
- **Time horizon**: thường cần hành động ngay, nhưng lý do có thể ngắn hoặc dài hạn.

## Người đặt limit order

- **Objective**: kiểm soát giá khớp, kiếm spread, hoặc chờ giá mong muốn.
- **Constraint**: có thể không được khớp.
- **Cost**: opportunity cost; adverse selection nếu bị khớp trước khi giá chạy ngược.
- **Risk**: bị “picked off” khi thông tin thay đổi.
- **Information**: quan sát book, flow, volatility, news.
- **Time horizon**: từ vài mili-giây đến nhiều ngày tùy actor.

## Market maker

- **Objective**: kiếm spread/rebate và quản lý inventory.
- **Constraint**: phải quote trong điều kiện cạnh tranh và rủi ro.
- **Cost**: adverse selection, inventory loss, technology, capital.
- **Risk**: market order một chiều liên tục khiến inventory lệch.
- **Information**: order flow, book dynamics, volatility.
- **Time horizon**: rất ngắn đến trung hạn tùy mô hình.

## Tổ chức lớn

Nếu bạn là một quỹ cần mua 2 triệu cổ phiếu nhưng ask book gần nhất chỉ có vài chục nghìn cổ phiếu, bạn gặp vấn đề:

- Mua quá nhanh → tự đẩy giá lên → average cost xấu.
- Mua quá chậm → lỡ cơ hội nếu giá tăng trước.
- Lộ ý định → người khác có thể front-run hoặc điều chỉnh quote.
- Chờ liquidity → có thể không đủ hàng.

Hành vi hợp lý có thể là chia nhỏ lệnh, dùng limit order, dùng algo, giao dịch theo volume thị trường, hoặc chờ vùng có nhiều liquidity. Đây là reasoning từ constraints, không cần giả định họ “muốn vẽ chart”.

---

# 8. EVIDENCE — Nếu cơ chế xảy ra, ta quan sát được gì?

## Directly observable

Tùy dữ liệu bạn có, có thể quan sát:

- **Price**: last price, high/low, close.
- **Volume**: tổng khối lượng giao dịch.
- **Bid/Ask**: best bid, best ask, spread.
- **Order Book**: depth ở từng price level.
- **Time & Sales**: từng giao dịch khớp ở giá nào, size bao nhiêu.
- **Delta / Footprint**: nếu có dữ liệu phân loại bid/ask transactions.
- **Volatility**: biên độ dao động.
- **Market/sector context**: thị trường chung, ngành, tin tức.

## Inferred

Những thứ chỉ là suy luận:

- “Buying pressure đang mạnh.”
- “Liquidity tại ask bị tiêu thụ.”
- “Có thể có large buyer.”
- “Có thể market maker rút liquidity.”
- “Có thể stop orders bị kích hoạt.”

Những câu sau là story nếu không có đủ evidence:

- “Cá mập đang gom hàng.”
- “Market maker cố tình kéo giá.”
- “Tổ chức lớn đang quét stop.”

## Nếu hypothesis đúng, ta kỳ vọng thấy gì tiếp theo?

Hypothesis: “Giá tăng vì aggressive buyers tiêu thụ ask liquidity.”

Kỳ vọng quan sát:

- Trades xảy ra nhiều tại ask hoặc quét lên nhiều ask levels.
- Ask depth tại các mức gần bị tiêu thụ nhanh.
- Spread có thể mở rộng nếu liquidity provider rút bớt.
- Last price tăng theo từng transaction.
- Nếu buying aggression tiếp tục và sellers không replenish đủ, giá tiếp tục tìm ask cao hơn.

Falsification tiềm năng:

- Giá tăng nhưng volume rất thấp trong book cực mỏng; không đủ để nói có buying pressure bền.
- Ask bị tiêu thụ nhưng ngay lập tức được replenish dày và giá không giữ được.
- Giá tăng chủ yếu do gap/news repricing, không phải do order flow quan sát trong phiên.

---

# 9. ALTERNATIVE EXPLANATIONS — Cùng hiện tượng, nhiều cơ chế

Hiện tượng: giá ABC tăng từ 100 lên 103 trong 10 phút, volume tăng mạnh.

| Hypothesis | Mechanism | Supporting Evidence | Contradicting Evidence | What Should Happen Next? |
|---|---|---|---|---|
| Aggressive buying thật sự | Market buy orders liên tục ăn ask liquidity | Nhiều trades tại ask; ask depth bị tiêu thụ; giá giữ vùng cao | Giá tăng bằng vài lệnh nhỏ trong book mỏng; không có follow-through | Nếu đúng, pullback nông và buyers tiếp tục hấp thụ sellers |
| Low-liquidity move | Book mỏng, ít sellers nên lệnh vừa phải cũng làm giá nhảy | Depth mỏng; spread rộng; volume không quá lớn | Volume lớn, nhiều mức ask dày vẫn bị ăn | Giá dễ revert khi liquidity quay lại |
| News repricing | Tin mới làm participants cập nhật giá trị | Tin/catalyst rõ; nhiều người cùng điều chỉnh quotes | Không có tin; sector không phản ứng | Giá giữ vùng mới nếu thông tin được thị trường chấp nhận |
| Short covering / forced buying | Người short phải mua lại | Tăng nhanh, squeeze-like; borrow/derivative context phù hợp | Không có evidence về short interest/forced orders | Có thể tăng nhanh rồi yếu khi forced buying hết |

Mục tiêu không phải chọn câu chuyện hấp dẫn nhất. Mục tiêu là hỏi: **evidence nào phân biệt các hypothesis này?**

---

# 10. FALSIFICATION — Điều gì chứng minh hypothesis yếu?

Hypothesis chính:

> Giá tăng vì aggressive buying tiêu thụ ask liquidity và tạo imbalance.

Confirmation evidence:

- Nhiều giao dịch khớp tại ask.
- Ask levels bị ăn liên tục.
- Pullback có volume thấp hơn và không phá vùng tăng.
- Sellers replenish nhưng tiếp tục bị hấp thụ.

Falsification evidence:

- Giá tăng chỉ vì spread rộng và một vài giao dịch nhỏ.
- Sau cú tăng, không có thêm aggressive buying và giá quay lại ngay.
- Book cho thấy ask liquidity không bị tiêu thụ đáng kể; giá nhảy do quote update/hủy lệnh.
- Tin tức hoặc auction/rebalance giải thích tốt hơn hiện tượng.
- Thị trường chung/ngành cùng tăng mạnh, khiến explanation riêng về ABC yếu đi.

Confirmation bias dễ mắc:

- Thấy giá tăng rồi đi tìm mọi dấu hiệu để chứng minh “có dòng tiền lớn”.
- Gọi mọi cú tăng volume là accumulation.
- Bỏ qua liquidity mỏng.
- Bỏ qua market/sector context.
- Kể lại câu chuyện quá hoàn hảo sau khi đã biết giá tăng.

---

# 11. APPLICATION — Dùng concept này như thế nào?

Quy trình:

**Observe → Interpret → Hypothesize → Predict → Test → Update probability → Decide**

Ví dụ:

## Observe

ABC đang ở 100. Best ask 100.1 có 5,000 cổ phiếu. Trong 2 phút, nhiều lệnh mua ăn qua 100.1, 100.2, 100.3. Volume tăng. Last price lên 100.4.

## Interpret

Aggressive buyers đang tiêu thụ ask liquidity gần nhất.

## Hypothesize

Hypothesis A: buying pressure thật sự đang mạnh.

Hypothesis B: book mỏng nên giá dễ nhảy, không nhất thiết có demand bền.

Hypothesis C: tin tức/sector move khiến sellers rút ask và repricing xảy ra.

## Predict

Nếu A đúng, giá có khả năng giữ vùng cao hơn, pullback gặp buyers, và sellers tại ask tiếp tục bị hấp thụ.

Nếu B đúng, khi liquidity quay lại, giá có thể revert nhanh.

Nếu C đúng, cần kiểm tra tin tức và phản ứng đồng pha ở ngành/thị trường.

## Test

Quan sát tiếp order book, time & sales, volume sau cú tăng, spread, market context.

## Update probability

Không kết luận nhị phân. Cập nhật xác suất theo evidence mới.

## Decide

Quyết định có thể là:

- Không giao dịch vì evidence chưa đủ.
- Chờ pullback để kiểm tra absorption.
- Chỉ ghi nhận cơ chế cho case study.
- Nếu giao dịch, xác định invalidation rõ ràng.

Concept này hữu ích khi bạn muốn hiểu **giá vừa di chuyển bằng cơ chế nào**. Nó không đủ để tự tạo edge giao dịch nếu thiếu context, risk management, timeframe, hypothesis testing và execution plan.

---

# 12. FACT → INFERENCE → STORY

## Ví dụ 1

> Giá ABC tăng từ 100 lên 102 trong 5 phút, volume gấp 3 lần trung bình 20 phút trước đó.

- **Fact**: Giá tăng 2%; volume tăng mạnh so với trung bình ngắn hạn.
- **Inference**: Có thể có aggressive buying hoặc repricing do thông tin.
- **Story**: “Cá mập đang gom hàng.” Đây chỉ là hypothesis, chưa phải fact.

## Ví dụ 2

> Best ask tại 50.0 có 20,000 cổ phiếu. Trong 10 giây, nhiều giao dịch khớp tại 50.0 nhưng giá không vượt lên.

- **Fact**: Nhiều volume giao dịch tại 50.0; giá chưa vượt 50.0.
- **Inference**: Ask liquidity tại 50.0 đang hấp thụ buy orders.
- **Story**: “Tổ chức lớn đang xả hàng ở 50.0.” Có thể đúng, nhưng cần thêm evidence.

## Ví dụ 3

> Spread mở rộng từ 0.1 lên 0.8 ngay trước tin tức, depth hai bên giảm mạnh.

- **Fact**: Spread rộng hơn; visible depth giảm.
- **Inference**: Liquidity providers có thể đang giảm rủi ro trước uncertainty.
- **Story**: “Market maker cố tình làm giá biến động.” Chưa đủ evidence.

## Ví dụ 4

> Giá vượt 100, chạm 101 rất nhanh, sau đó rơi lại 99.8.

- **Fact**: Giá breakout lên 101 rồi thất bại và quay lại dưới 100.
- **Inference**: Cú vượt 100 không tìm được demand tiếp diễn, hoặc liquidity phía trên bị tiêu thụ rồi aggression cạn.
- **Story**: “Cá mập quét stop rồi đạp xuống.” Chỉ là một hypothesis cần kiểm chứng.

---

# 13. Multiple hypotheses cho một tình huống mơ hồ

Tình huống:

> Cổ phiếu DEF đi ngang quanh 80 trong cả buổi sáng. Cuối phiên, giá bất ngờ tăng lên 82 với volume lớn, nhưng ngay sau đó đóng cửa ở 80.5.

Hãy tự xây ít nhất 3 hypothesis trước khi xem bảng.

| Hypothesis | Mechanism | Supporting Evidence | Contradicting Evidence | What Should Happen Next? |
|---|---|---|---|---|
| Aggressive buying thất bại | Buyers ăn ask lên 82 nhưng không có follow-through; sellers quay lại | Time & sales cho thấy buying tại ask; sau đó ask replenish mạnh | Giá giữ trên 82 và tiếp tục tăng ngày sau | Ngày sau nếu buyers yếu, vùng 82 tiếp tục là supply |
| Low-liquidity spike | Cuối phiên book mỏng; lệnh vừa phải đẩy giá lên rồi revert | Depth mỏng, spread rộng, volume thực tế không quá lớn so với giá trị giao dịch | Book dày mà vẫn bị ăn qua nhiều mức | Giá quay về vùng cũ khi liquidity bình thường trở lại |
| News/rumor chưa được xác nhận | Một số participants mua nhanh theo tin; sau đó tin không đủ mạnh | Có rumor/news timestamp tương ứng | Không có catalyst; toàn ngành không phản ứng | Giá cần phản ứng theo tin chính thức; nếu không, move yếu đi |
| Rebalance/forced execution | Lệnh cuối phiên từ cơ chế tái cân bằng hoặc forced buying/selling | Thời điểm gần close; nhiều mã cùng có flow bất thường | Chỉ riêng DEF có move cùng microstructure riêng | Move có thể không tiếp diễn nếu chỉ là flow kỹ thuật |

Kết luận đúng ở thời điểm này có thể là: **không đủ evidence để kết luận ý định của actor chính**. Nhưng ta có thể mô tả mechanism có thể xảy ra và lập kế hoạch kiểm chứng.

---

# 14. Case studies

## Case A — Normal case: Market buy quét qua ask mỏng

### Facts

- Best ask 100.0 có 1,000 cổ phiếu.
- Ask 100.1 có 2,000.
- Ask 100.2 có 3,000.
- Market buy 5,000 xuất hiện.
- Last price tăng từ 100.0 lên 100.2.

### Mechanism

**Market buy 5,000 → consume 1,000 at 100.0 → consume 2,000 at 100.1 → consume 2,000 at 100.2 → last transaction at 100.2 → last price increases**

### Actors

- Aggressive buyer.
- Passive sellers ở 100.0, 100.1, 100.2.
- Có thể có market maker hoặc trader thường ở phía bán.

### Incentives

Buyer ưu tiên khớp ngay hơn kiểm soát giá. Sellers sẵn sàng bán tại giá đã treo.

### Hypotheses

- Buyer cần vào vị thế ngay.
- Buyer phản ứng với thông tin.
- Buyer là execution algo đang mua từng phần.

### Evidence

Time & sales và order book đủ để xác nhận cơ chế khớp qua nhiều ask levels. Không đủ để xác nhận động cơ sâu hơn của buyer.

### Falsification

Nếu dữ liệu book cho thấy không có ask bị ăn mà giá chỉ nhảy do quote update, hypothesis “market buy quét ask” yếu đi.

### Conclusion

Có thể kết luận giá tăng do lệnh mua aggressive tiêu thụ ask liquidity. Không nên kết luận “cá mập gom hàng” nếu thiếu evidence về actor, size tổng, lặp lại, context.

## Case B — Counterexample: Giá tăng nhưng không phải buying pressure mạnh

### Facts

- Cổ phiếu GHI spread rộng.
- Best ask 20.0 có 100 cổ phiếu.
- Ask kế tiếp 20.8 có 100 cổ phiếu.
- Một market buy 100 cổ phiếu khớp tại 20.8 sau khi ask 20.0 bị hủy.
- Last price tăng 4%.

### Mechanism

**Thin book + cancellation at lower ask → next available ask far away → tiny trade prints higher → last price jumps**

### Actors

- Một buyer nhỏ.
- Passive sellers ít.
- Liquidity providers có thể đã rút quote.

### Incentives

Buyer muốn khớp ngay hoặc không kiểm soát tốt lệnh. Sellers không muốn bán gần giá cũ.

### Hypotheses

- Low-liquidity print.
- Quote withdrawal.
- Small uninformed market order.

### Evidence

Volume rất nhỏ, spread rộng, depth mỏng.

### Falsification

Nếu sau đó có volume lớn tiếp tục ăn ask dày và giá giữ vùng cao, “chỉ là low-liquidity print” yếu đi.

### Conclusion

Bề ngoài là giá tăng mạnh, nhưng mechanism không chứng minh demand mạnh. Đây là counterexample quan trọng: **price change size ≠ buying pressure strength** nếu không xét liquidity.

## Case C — Ambiguous case: Volume lớn, giá đứng yên

### Facts

- ABC giao dịch quanh 100.
- Volume lớn bất thường.
- Nhiều trades xảy ra tại 100.0–100.1.
- Giá không vượt 100.2.

### Mechanism có thể

- Buyers aggressive nhưng ask liquidity hấp thụ.
- Sellers aggressive nhưng bid liquidity hấp thụ.
- Hai phía cùng lớn, tạo transfer of inventory.
- Execution algo chia nhỏ lệnh.
- Tin tức làm cả buyers và sellers tham gia mạnh.

### Actors

Retail, fund, market maker, algo, arbitrageur đều có thể liên quan.

### Incentives

- Một bên muốn vào/thoát size lớn.
- Market maker quản lý inventory.
- Traders ngắn hạn phản ứng với vùng giá.
- Tổ chức có thể execution theo benchmark.

### Hypotheses

1. Absorption bởi seller lớn.
2. Absorption bởi buyer lớn.
3. Two-way institutional transfer.
4. Event/news-driven churn.

### Evidence cần thêm

- Trades chủ yếu tại bid hay ask?
- Order book có replenish lặp lại không?
- Giá phản ứng sau vùng này ra sao?
- Market/sector context thế nào?
- Có news/catalyst không?

### Falsification

Nếu nói “seller lớn hấp thụ”, nhưng sau đó giá break mạnh lên với buyers tiếp tục kiểm soát và sellers không còn replenish, hypothesis đó yếu đi.

### Conclusion

Không đủ evidence để kết luận một câu chuyện duy nhất. Bài học đúng là xây nhiều hypothesis và theo dõi evidence phân biệt.

---

# 15. Socratic questions

Hãy trả lời trước khi xem phần đáp án.

1. Nếu best ask có 1,000 cổ phiếu ở 100 và bạn gửi market buy 500, tại sao last price có thể không vượt 100?
2. Nếu cũng book đó nhưng bạn gửi market buy 5,000, tại sao last price có thể tăng?
3. Tại sao limit order cung cấp liquidity còn market order tiêu thụ liquidity?
4. Spread bù đắp rủi ro gì cho người cung cấp liquidity?
5. Một cú tăng giá 5% với volume rất nhỏ có thể giải thích bằng mechanism nào?
6. Nếu giá tăng mạnh nhưng sau đó quay lại ngay, hypothesis “buying pressure bền” yếu ở điểm nào?
7. Ai ở phía đối diện khi bạn mua bằng market order?
8. Vì sao cùng một lệnh 1 tỷ đồng tác động khác nhau ở cổ phiếu thanh khoản cao và thấp?
9. Nếu order book hiển thị ask rất dày nhưng khi giá tới gần thì ask biến mất, điều đó ảnh hưởng gì tới market impact?
10. Điều gì phân biệt fact “giá tăng, volume tăng” với story “cá mập gom hàng”?
11. Nếu market maker hủy quote trước tin tức, có nhất thiết là thao túng không?
12. Evidence nào khiến bạn thừa nhận “giá tăng không phải do aggressive buying bền”?

## Đáp án và reasoning

1. Vì market buy 500 nhỏ hơn ask liquidity 1,000 tại 100. Toàn bộ lệnh được hấp thụ ở 100, không cần giao dịch ở ask cao hơn.
2. Vì lệnh 5,000 lớn hơn liquidity tại best ask. Sau khi ăn hết 100, nó phải khớp với các ask cao hơn nếu muốn hoàn tất.
3. Limit order nằm chờ để người khác giao dịch với nó, nên nó thêm liquidity. Market order yêu cầu giao dịch ngay với lệnh đang chờ, nên nó lấy đi liquidity.
4. Spread bù cho adverse selection, inventory risk, volatility, chi phí vốn và chi phí vận hành.
5. Có thể do book mỏng, spread rộng, quote cancellation, hoặc một lệnh nhỏ khớp ở mức giá xa. Không nhất thiết là demand mạnh.
6. Nếu không có follow-through và giá revert nhanh, cú tăng có thể chỉ là temporary impact hoặc low-liquidity move.
7. Bạn mua từ passive sellers đang treo ask, có thể là retail, fund, market maker, algo hoặc actor khác.
8. Tác động phụ thuộc vào size relative to liquidity. 1 tỷ nhỏ so với depth của cổ phiếu lớn nhưng lớn so với depth của cổ phiếu kém thanh khoản.
9. Visible liquidity giảm, nên cùng market order sẽ ăn qua nhiều mức giá hơn, làm impact/slippage tăng.
10. Fact là dữ liệu quan sát được. Story là diễn giải về ý định actor. “Cá mập gom” cần evidence thêm về size, lặp lại, absorption, context và alternatives.
11. Không. Hủy quote có thể là quản trị rủi ro hợp lý trước uncertainty.
12. Volume thấp, book mỏng, lack of follow-through, giá revert nhanh, hoặc explanation khác như news/rebalance/sector move phù hợp hơn.

---

# 16. Kiểm tra “thực sự hiểu”

## Reverse reasoning

Kết quả: giá nhảy từ 50 lên 52 chỉ trong vài giao dịch.

Hãy nêu ít nhất 4 mechanism có thể:

- Market buy lớn ăn qua ask levels.
- Ask liquidity bị hủy/rút trước khi giao dịch.
- Book rất mỏng nên lệnh nhỏ cũng làm last price nhảy.
- Tin tức khiến sellers quote lại cao hơn.
- Stop/forced buying kích hoạt thêm lệnh mua.

## What-if

Nếu ask liquidity tại best ask tăng từ 1,000 lên 100,000, cùng một market buy 5,000 sẽ thế nào?

Reasoning: lệnh có khả năng khớp toàn bộ tại best ask, impact thấp hơn.

## Counterexample

“Giá tăng mạnh nghĩa là có dòng tiền lớn vào.”

Counterexample: cổ phiếu thanh khoản thấp, spread rộng, chỉ 200 cổ phiếu khớp ở ask xa hơn làm last price tăng 5%. Giá tăng mạnh nhưng không chứng minh dòng tiền lớn.

## Falsification

Nhận định: “Giá tăng do aggressive buyers kiểm soát.”

Evidence làm yếu nhận định:

- Trades không chủ yếu xảy ra tại ask.
- Volume thấp.
- Spread rộng và book mỏng.
- Giá không giữ vùng tăng.
- Sellers replenish và hấp thụ dễ dàng.
- Market/sector/news giải thích tốt hơn.

## Teach-back

Hãy giải thích không dùng thuật ngữ chuyên môn:

> Giá tăng khi những người muốn mua ngay mua hết hàng của người bán rẻ nhất. Khi hàng ở giá rẻ hết, ai còn muốn mua tiếp phải chấp nhận mua từ người bán đòi giá cao hơn. Giao dịch mới xảy ra ở giá cao hơn, nên bảng giá hiển thị giá cao hơn.

---

# 17. Connection Map

## Prerequisite

Trước bài này, chỉ cần hiểu:

- Giao dịch luôn có hai phía.
- Giá là giá của giao dịch đã xảy ra.
- Mua/bán là hành động giữa người tham gia có incentive khác nhau.

## Upstream

Điều tạo ra market microstructure:

**Participants → different objectives → need to express buy/sell interest → orders → market rules**

## Current concept

Market microstructure giải thích:

**Orders → Order Book → Matching → Transactions → Price Formation**

## Downstream

Từ bài này, ta có nền tảng để học:

- Liquidity.
- Order flow.
- Absorption.
- Exhaustion.
- Institutional execution.
- Support/resistance từ orders và liquidity.
- Breakout/pullback/failure từ cơ chế giao dịch.

## Map nối với toàn khóa

**Participants → Incentives → Orders → Order Book → Order Flow ↔ Liquidity → Imbalance/Absorption → Price Discovery → Price & Volume → Market Structure → Price Action → Patterns**

Phần 1 chủ yếu bao phủ:

**Participants → Incentives → Orders → Order Book → Matching/Price Discovery**

Phần tiếp theo, **Liquidity**, xuất hiện tự nhiên vì sau khi hiểu lệnh khớp thế nào, câu hỏi kế tiếp là:

> Nếu tôi muốn giao dịch một lượng rất lớn, ai sẽ đứng phía bên kia và thị trường có đủ khả năng hấp thụ không?

---

# 18. Gate 1 — Giải thích “Tại sao giá tăng?” không dùng chart pattern hoặc indicator

Câu trả lời đạt chuẩn:

> Giá tăng khi các giao dịch thực tế bắt đầu xảy ra ở mức giá cao hơn. Điều này thường xảy ra khi lệnh mua aggressive tiêu thụ hết lượng bán đang chờ ở các mức ask thấp hơn, hoặc khi sellers rút/hủy liquidity khiến ask tốt nhất nhảy lên cao hơn. Nếu người mua vẫn cần khớp ngay, họ phải giao dịch với sellers ở mức giá cao hơn. Last price vì vậy tăng. Tuy nhiên, để biết đây là demand bền, low-liquidity move, news repricing hay forced buying, cần thêm evidence về volume, order book, bid/ask transactions, spread, context và follow-through.

Câu trả lời chưa đạt:

- “Giá tăng vì nhiều người mua hơn người bán.”
- “Giá tăng vì dòng tiền vào.”
- “Giá tăng vì cá mập kéo.”
- “Giá tăng vì breakout.”

Các câu này có thể là shorthand, nhưng thiếu cơ chế. Để đạt chuẩn, phải nói rõ:

**Ai aggressive? Họ tiêu thụ liquidity nào? Book có đủ hấp thụ không? Giao dịch xảy ra tại giá nào? Evidence nào quan sát được? Explanation nào khác có thể đúng?**

---

# 19. Kết thúc bài

## 1. First-Principles Summary

1. Giá không tự di chuyển; giá thay đổi khi orders tương tác và transaction price mới xuất hiện.
2. Market orders ưu tiên khớp ngay và tiêu thụ liquidity; limit orders ưu tiên giá và cung cấp liquidity.
3. Bid/ask/spread tồn tại vì participants có incentives, rủi ro và time horizon khác nhau.
4. Market impact phụ thuộc vào size của lệnh so với available liquidity, không chỉ phụ thuộc vào size tuyệt đối.
5. Cùng một biến động giá có thể có nhiều mechanism; phải phân biệt fact, inference và story.

## 2. Mental Model

Hãy nghĩ về giá như dấu vết của một cuộc đấu giá liên tục:

**Người muốn giao dịch ngay phải đi tới nơi có người đang chờ giao dịch. Nếu lượng chờ ở giá hiện tại không đủ, giao dịch phải đi tới giá tiếp theo.**

## 3. Không được nhầm

- Không nhầm **giá tăng** với **demand bền**.
- Không nhầm **volume lớn** với **liquidity cao**.
- Không nhầm **last price** với **giá trị hợp lý**.
- Không nhầm **market maker quản trị rủi ro** với **thao túng**.
- Không gọi “cá mập gom/xả” là fact nếu chỉ có chart và volume cơ bản.

## 4. Tôi đã hiểu nếu...

Bạn đã hiểu phần 1 nếu có thể:

- Giải thích một market buy làm giá tăng qua order book như thế nào.
- Phân biệt market order, limit order, stop order, stop-limit, resting order.
- Giải thích bid, ask, spread, mid-price từ incentive và risk.
- Vẽ causal chain từ participants đến price.
- Phân biệt directly observable data với inference.
- Đưa ra ít nhất 3 explanation cho cùng một cú tăng/giảm giá.
- Nêu evidence có thể chứng minh hypothesis của bạn sai.
- Trả lời “Who is on the other side?” trong mọi giao dịch.

## 5. Bridge to Next Lesson

Sau khi hiểu rằng lệnh mua/bán làm giá thay đổi bằng cách tiêu thụ liquidity trong order book, câu hỏi tiếp theo gần như bắt buộc là:

> **Liquidity thực sự là gì, nằm ở đâu, và vì sao dòng tiền lớn cần liquidity?**

Đó là Phần 2. Nếu Phần 1 giải thích **lệnh biến thành giá như thế nào**, thì Phần 2 giải thích **thị trường có thể hấp thụ bao nhiêu lệnh trước khi giá phải di chuyển mạnh**.
