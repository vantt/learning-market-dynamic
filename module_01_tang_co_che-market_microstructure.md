# Module 1 — Tầng cơ chế / Cấu trúc vi mô thị trường (Market Microstructure)

## Câu hỏi trung tâm

**Một lệnh mua/bán thực sự biến thành chuyển động giá bằng cách nào?**

Bài học này không bắt đầu từ mô hình giá (chart pattern), nến, chỉ báo (indicator) hay câu chuyện “cá mập kéo giá”. Ta bắt đầu từ thứ nhỏ nhất có thể quan sát và suy luận: **người tham gia thị trường gửi lệnh; lệnh tương tác với lệnh khác; giao dịch xảy ra tại một mức giá; chuỗi giao dịch đó tạo thành giá thị trường**.

Nếu hiểu phần này, bạn sẽ trả lời được câu hỏi “tại sao giá tăng?” mà không cần nói “vì chart đẹp”, “vì dòng tiền vào”, hoặc “vì cá mập gom”. Bạn sẽ có thể phân rã thành cơ chế:

**Lệnh mua ngay (market buy orders) → tiêu thụ thanh khoản bán đang chờ (available ask liquidity) → người bán ở mức giá hiện tại giảm → lệnh mua tiếp theo phải khớp với giá chào bán (ask) cao hơn → giá giao dịch (transaction price) tăng → giá khớp gần nhất (last price) tăng.**

## Thuật ngữ cần nắm trước

| English term | Cách gọi tiếng Việt | Định nghĩa ngắn bằng tiếng Việt |
|---|---|---|
| Market Microstructure | Cấu trúc vi mô thị trường | Cách thị trường nhận lệnh, xếp hàng, khớp lệnh và biến các giao dịch riêng lẻ thành giá quan sát được. |
| Market Order | Lệnh thị trường / lệnh khớp ngay | Lệnh ưu tiên khớp ngay với giá tốt nhất đang có ở phía đối diện, chấp nhận rủi ro trượt giá. |
| Limit Order | Lệnh giới hạn | Lệnh chỉ mua/bán tại mức giá đã đặt hoặc tốt hơn; đổi lại có thể không được khớp. |
| Bid | Giá chào mua | Mức giá cao nhất mà người mua đang đặt chờ mua. |
| Ask | Giá chào bán | Mức giá thấp nhất mà người bán đang đặt chờ bán. |
| Spread | Chênh lệch mua-bán | Khoảng cách giữa giá chào bán tốt nhất và giá chào mua tốt nhất. |
| Limit Order Book | Sổ lệnh giới hạn | Danh sách các lệnh mua/bán giới hạn đang chờ khớp ở từng mức giá. |
| Liquidity | Thanh khoản | Khả năng thị trường hấp thụ lệnh mua/bán mà không làm giá dịch chuyển quá mạnh. |
| Matching Engine | Hệ thống khớp lệnh | Cơ chế của sàn dùng để ghép lệnh mua và bán theo quy tắc ưu tiên. |
| Transaction Price | Giá giao dịch | Mức giá thực sự xảy ra khi một lệnh mua và một lệnh bán khớp nhau. |
| Last Price | Giá khớp gần nhất | Giá của giao dịch vừa xảy ra gần nhất, không nhất thiết là “giá trị hợp lý”. |
| Market Impact | Tác động lên giá | Mức độ một lệnh làm thay đổi giá do tiêu thụ thanh khoản hiện có. |
| Slippage | Trượt giá | Chênh lệch giữa giá kỳ vọng và giá khớp thực tế. |

---

# 1. Vấn đề mở đầu: Vì sao một lệnh mua có thể làm giá tăng?

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
4. Có phải giá tăng vì người mua “muốn đẩy giá lên”, hay vì cơ chế khớp lệnh buộc giao dịch xảy ra ở các mức chào bán cao hơn?
5. Nếu tại 100.0 có 100,000 cổ phiếu chờ bán thay vì 1,000, cùng lệnh mua 4,500 cổ phiếu có làm giá tăng giống vậy không?

Bây giờ phân rã:

- Người mua cần 4,500 cổ phiếu ngay.
- Ở mức chào bán 100.0 chỉ có 1,000 cổ phiếu chờ bán.
- 1,000 cổ phiếu đầu tiên khớp tại 100.0.
- Còn 3,500 cổ phiếu chưa mua xong.
- Mức chào bán 100.0 đã bị “ăn hết”.
- Lệnh mua tiếp tục khớp với 2,000 cổ phiếu tại 100.1.
- Còn 1,500 cổ phiếu chưa mua xong.
- Lệnh mua tiếp tục khớp với 1,500 trong 3,000 cổ phiếu tại 100.2.
- Giao dịch cuối cùng xảy ra tại 100.2.

Hãy nhìn lệnh này như một nhu cầu mua phải đi lần lượt qua từng “tầng” thanh khoản:

```text
NHU CẦU BAN ĐẦU: MARKET BUY 4,500 CP
                       ↓
ASK 100.0 ── có 1,000 ── dùng hết 1,000 ── ✕ HẾT
                       ↓ còn mua 3,500
ASK 100.1 ── có 2,000 ── dùng hết 2,000 ── ✕ HẾT
                       ↓ còn mua 1,500
ASK 100.2 ── có 3,000 ── dùng 1,500 ────── ● CÒN 1,500
                       ↓
                    HOÀN TẤT
          GIÁ CUỐI = 100.2
          GIÁ TB   ≈ 100.111
```

**Cách đọc:** bắt đầu từ nhu cầu 4,500 cổ phiếu ở trên. Mỗi mũi tên đi xuống chỉ xuất hiện khi lượng bán ở mức hiện tại không đủ hoàn tất lệnh. Giá cuối là giá của phần khớp sau cùng; giá trung bình là tổng giá trị khớp chia cho 4,500 cổ phiếu, nên hai giá này không giống nhau.

Phát biểu nhân quả chính xác là: **khi lượng mua chủ động lớn hơn lượng bán đang chờ tại các mức chào bán thấp nhất, phần chưa khớp phải đi lên mức chào bán cao hơn, khiến giá giao dịch cuối tăng.**

> **Ghi nhớ:** chưa mua đủ thì phải đi lên tầng giá kế tiếp.

Kết quả:

| Giá khớp | Khối lượng khớp |
|---:|---:|
| 100.0 | 1,000 |
| 100.1 | 2,000 |
| 100.2 | 1,500 |

**Giá khớp gần nhất (last price)** sau lệnh này là 100.2, không phải vì thị trường “tự chạy”, mà vì lệnh mua ngay đã tiêu thụ hết người bán sẵn sàng bán ở giá thấp hơn.

Khái niệm xuất hiện tự nhiên từ vấn đề này là **cấu trúc vi mô thị trường (Market Microstructure)**: cơ chế bên dưới việc lệnh được gửi, xếp hàng, khớp, hủy, và biến thành giá giao dịch.

---

# 2. WHY — Tại sao cấu trúc vi mô thị trường phải tồn tại?

Thị trường tài chính không phải một cái bảng giá tự động thay đổi theo cảm xúc. Nó là một hệ thống xử lý xung đột giữa nhiều người:

- Người muốn mua ngay.
- Người muốn bán ngay.
- Người chỉ muốn mua nếu giá đủ thấp.
- Người chỉ muốn bán nếu giá đủ cao.
- Người cung cấp thanh khoản để kiếm chênh lệch mua-bán.
- Người cần thoát vị thế vì rủi ro.
- Người có thông tin mới và muốn hành động trước.
- Thuật toán gửi/hủy lệnh liên tục.

Vấn đề thực tế là: **làm sao chuyển rất nhiều ý định giao dịch khác nhau thành giao dịch cụ thể, công bằng, có thứ tự, có giá và có khối lượng?**

Nếu không có microstructure:

- Không rõ ai được khớp trước khi nhiều người cùng mua/bán.
- Không rõ giá nào được dùng khi người mua và người bán không đồng thuận.
- Không rõ điều gì xảy ra khi khối lượng muốn mua ngay lớn hơn khối lượng muốn bán ở giá hiện tại.
- Không rõ vì sao cùng một lệnh 1 tỷ đồng có thể gần như không làm giá VNM dịch chuyển nhưng có thể làm cổ phiếu kém thanh khoản tăng mạnh.
- Không thể phân biệt giá tăng do thông tin, do thiếu thanh khoản, do mua bắt buộc (forced buying), hay do một lệnh lớn quét qua sổ lệnh.

Cấu trúc vi mô thị trường tồn tại vì thị trường cần một cơ chế để trả lời:

**Ai muốn giao dịch? Giao dịch với ai? Tại giá nào? Với bao nhiêu khối lượng? Theo thứ tự nào? Và nếu không đủ đối ứng ở giá hiện tại thì chuyện gì xảy ra?**

---

# 3. WHAT — Bản chất là gì?

## Tầng 1 — Trực giác (Intuition)

Hãy hình dung thị trường như một cái chợ có bảng rao mua và rao bán.

- Người mua treo giá: “Tôi sẵn sàng mua 1,000 cổ phiếu ở 99.9.”
- Người bán treo giá: “Tôi sẵn sàng bán 1,000 cổ phiếu ở 100.0.”
- Người cần giao dịch ngay sẽ chấp nhận giá tốt nhất đang có ở phía đối diện.
- Nếu họ mua nhiều hơn lượng đang được bán ở giá tốt nhất, họ phải mua tiếp ở giá cao hơn.

Giá không tăng vì bảng giá “quyết định tăng”. Giá tăng vì **những người bán rẻ nhất đã bị tiêu thụ**, nên giao dịch tiếp theo phải xảy ra với người bán đòi giá cao hơn.

## Tầng 2 — Định nghĩa chuẩn (Standard)

**Cấu trúc vi mô thị trường (Market Microstructure)** là lĩnh vực nghiên cứu cách thị trường tổ chức giao dịch: cách lệnh (orders) được gửi vào hệ thống, được ưu tiên, được khớp, bị hủy, tạo thành giá giao dịch, thanh khoản, chênh lệch mua-bán (spread), biến động giá (volatility) và tác động lên giá (market impact).

Trong phần 1, ta tập trung vào:

- **Sàn giao dịch (Exchange)**: nơi tổ chức giao dịch.
- **Công ty môi giới (Broker)**: trung gian đưa lệnh của khách hàng vào thị trường.
- **Hệ thống khớp lệnh (Matching engine)**: hệ thống khớp lệnh mua và bán.
- **Lệnh thị trường (Market order)**: lệnh muốn khớp ngay với giá tốt nhất hiện có.
- **Lệnh giới hạn (Limit order)**: lệnh chỉ khớp tại mức giá giới hạn hoặc tốt hơn.
- **Giá chào mua / giá chào bán / chênh lệch (Bid/Ask/Spread)**: giá mua tốt nhất, giá bán tốt nhất, và khoảng cách giữa chúng.
- **Sổ lệnh giới hạn (Limit Order Book)**: các lệnh giới hạn đang chờ khớp.
- **Giá giao dịch / giá khớp gần nhất (Transaction price / Last price)**: giá thực sự xảy ra trong giao dịch gần nhất.
- **Tác động lên giá / trượt giá (Market impact / Slippage)**: tác động của lệnh lên giá và chênh lệch giữa giá kỳ vọng với giá khớp thực tế.

## Tầng 3 — First Principles

Ở mức cơ bản nhất, thị trường gồm:

**Người tham gia (participants) → động cơ/ràng buộc (incentives) → lệnh (orders) → sổ lệnh (order book) → khớp lệnh (matching) → giao dịch (transactions) → giá (price)**

```text
NGƯỜI THAM GIA
      ↓ có mục tiêu và ràng buộc
ĐỘNG CƠ / QUYẾT ĐỊNH
      ↓ được mã hóa thành
     LỆNH
      ↓ được xếp trong
    SỔ LỆNH
      ↓ gặp lệnh tương thích
   KHỚP LỆNH
      ↓ tạo ra
   GIAO DỊCH
      ↓ để lại dấu vết
 GIÁ QUAN SÁT ĐƯỢC
```

**Cách đọc:** đi từ trên xuống. Mỗi mũi tên là một phép chuyển đổi bắt buộc: ý định phải thành lệnh, lệnh phải được xử lý theo quy tắc, và chỉ giao dịch đã khớp mới tạo ra giá giao dịch.

Vì vậy, **ý định không trực tiếp làm giá đổi; ý định chỉ có thể tác động đến giá sau khi trở thành lệnh và tương tác thành công với thanh khoản phía đối diện.**

> **Ghi nhớ:** giá là đầu ra của quá trình tương tác lệnh, không phải điểm khởi đầu.

Trong đó:

- **Người tham gia (participants)** là người hoặc thuật toán tham gia thị trường.
- **Động cơ/ràng buộc (incentives)** là lý do họ muốn mua/bán, chờ/đánh ngay, giấu/hiện khối lượng, vào/thoát vị thế.
- **Lệnh (orders)** là cách ý định được chuyển thành chỉ thị giao dịch.
- **Sổ lệnh (order book)** là trạng thái tạm thời của các lệnh đang chờ.
- **Khớp lệnh (matching)** là cơ chế ghép lệnh mua và bán tương thích.
- **Giao dịch (transactions)** là các lần mua bán thực sự đã xảy ra.
- **Giá (price)** là dấu vết của các giao dịch đó, không phải nguyên nhân đầu tiên.

## Cấu trúc vi mô thị trường không phải là gì?

- Không phải một chỉ báo (indicator).
- Không phải một chiến lược “thấy X thì mua”.
- Không phải công cụ đọc chắc ý đồ “cá mập”.
- Không phải lời giải thích duy nhất cho mọi biến động giá.
- Không phải mô hình giá ở cấp thấp hơn; nó là cơ chế tạo ra giá và khối lượng mà biểu đồ ghi lại.

---

# 4. MECHANISM — Lệnh biến thành giá bằng cách nào?

Đây là phần quan trọng nhất.

## 4.1 Các thành phần của thị trường

Một lệnh đơn giản thường đi qua chuỗi:

**Nhà giao dịch (trader) → công ty môi giới (broker) → sàn giao dịch (exchange) → hệ thống khớp lệnh (matching engine) → phía đối ứng (counterparty) → bù trừ (clearing) → lưu ký (custody)**

```text
NHÀ GIAO DỊCH
      ↓ gửi chỉ thị
   MÔI GIỚI
      ↓ kiểm tra và chuyển lệnh
 SÀN / HỆ THỐNG KHỚP
      ↓ tìm lệnh tương thích
  PHÍA ĐỐI ỨNG
      ↓ giao dịch được xác lập
    BÙ TRỪ
      ↓ hoàn tất nghĩa vụ
    LƯU KÝ
```

**Cách đọc:** luồng đi từ quyết định giao dịch tới việc hoàn tất quyền và nghĩa vụ. Mũi tên giữa hệ thống khớp và phía đối ứng chỉ xảy ra nếu có lệnh tương thích về giá và khối lượng.

Phát biểu chính xác: **một chỉ thị mua chỉ trở thành giao dịch khi hạ tầng thị trường tìm được một hoặc nhiều lệnh bán tương thích; sau đó bù trừ và lưu ký mới hoàn tất việc chuyển tiền và tài sản.**

> **Ghi nhớ:** mọi giao dịch đều cần một phía đối ứng; “thị trường” không tự bán tài sản cho bạn.

Giải thích từng mắt xích:

- **Nhà giao dịch / nhà đầu tư (trader / investor)**: người ra quyết định mua/bán.
- **Công ty môi giới (broker)**: nhận lệnh từ khách hàng, kiểm tra điều kiện tài khoản, gửi lệnh đến sàn hoặc địa điểm giao dịch phù hợp.
- **Sàn giao dịch (exchange)**: nơi tập trung và tổ chức giao dịch theo luật.
- **Hệ thống khớp lệnh (matching engine)**: hệ thống so khớp lệnh mua và bán.
- **Phía đối ứng (counterparty)**: phía đối diện của giao dịch. Nếu bạn mua, luôn có ai đó bán.
- **Bù trừ (clearing)**: đối chiếu và bảo đảm nghĩa vụ thanh toán/giao hàng.
- **Lưu ký (custody)**: ghi nhận quyền sở hữu tài sản sau khi giao dịch hoàn tất.

Câu hỏi bắt buộc: **Who is on the other side?**

Nếu bạn mua 1,000 cổ phiếu, không có chuyện “mua từ thị trường” theo nghĩa mơ hồ. Bạn mua từ một hoặc nhiều người bán cụ thể đang cung cấp cổ phiếu tại các mức giá cụ thể.

## 4.2 Lệnh (Orders) — Các loại lệnh là cách biểu đạt động cơ

### Lệnh thị trường (Market Order)

**Lệnh thị trường (market order)** là lệnh ưu tiên khớp ngay, chấp nhận giá tốt nhất hiện có ở phía đối diện.

Cơ chế:

**Cần khớp ngay → gửi lệnh thị trường → đánh vào thanh khoản đang có → tiêu thụ giá chào mua/bán tốt nhất → có thể quét qua nhiều mức trong sổ lệnh → giá giao dịch thay đổi**

Ví dụ:

Bạn mua ngay 3,000 cổ phiếu. Phía chào bán trong sổ lệnh:

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

Giá khớp gần nhất sau lệnh là 50.10.

Bạn muốn “mua ngay”, nên chi phí là có thể phải trả giá cao hơn mức chào bán đầu tiên.

### Lệnh giới hạn (Limit Order)

**Lệnh giới hạn (limit order)** là lệnh chỉ khớp tại giá giới hạn hoặc tốt hơn.

Ví dụ:

- Limit buy 49.90: chỉ mua ở 49.90 hoặc thấp hơn.
- Limit sell 50.20: chỉ bán ở 50.20 hoặc cao hơn.

Cơ chế:

**Sẵn sàng chờ → đặt lệnh giới hạn → cung cấp thanh khoản → xếp vào hàng chờ → đợi phía đối ứng chủ động → được khớp hết, khớp một phần, hoặc bị hủy**

Lệnh giới hạn đổi lấy quyền kiểm soát giá bằng việc chấp nhận rủi ro không được khớp.

### Lệnh dừng (Stop Order)

**Lệnh dừng (stop order)** là lệnh được kích hoạt khi giá chạm một mức xác định.

Ví dụ:

- Bạn đang nắm cổ phiếu ở 100, đặt stop-loss tại 95.
- Khi giá giao dịch chạm 95, lệnh bán được kích hoạt, thường trở thành market sell hoặc limit sell tùy loại.

Cơ chế quan trọng:

**Giá chạm mức kích hoạt → lệnh đang ngủ trở thành lệnh hoạt động → lệnh thị trường hoặc lệnh giới hạn mới đi vào thị trường → có thể làm lực giao dịch cùng chiều mạnh hơn**

Lệnh dừng không phải thanh khoản đang nằm sẵn trong sổ lệnh như lệnh giới hạn thông thường. Nó là **lệnh có điều kiện (conditional order)**: chưa hoạt động cho đến khi mức kích hoạt xuất hiện.

### Stop-limit

**Stop-limit** có hai mức:

- Stop price: mức kích hoạt.
- Limit price: mức giá giới hạn sau khi kích hoạt.

Ưu điểm: tránh khớp quá xấu.

Rủi ro: nếu thị trường chạy nhanh qua limit price, lệnh có thể không khớp.

### Lệnh đang chờ (Resting order)

**Lệnh đang chờ (resting order)** là lệnh đang nằm chờ trong sổ lệnh, thường là lệnh giới hạn.

Lệnh đang chờ tạo ra **thanh khoản nhìn thấy được (visible liquidity)** nếu được hiển thị.

### Lệnh chủ động và lệnh thụ động (Aggressive vs Passive)

Một lệnh là **chủ động (aggressive)** nếu nó yêu cầu khớp ngay và tiêu thụ thanh khoản phía đối diện.

Một lệnh là **thụ động (passive)** nếu nó nằm chờ và cung cấp thanh khoản.

Hai loại lệnh thể hiện hai ưu tiên và hai đánh đổi khác nhau:

```text
                         NHU CẦU GIAO DỊCH
                                │
                  ┌─────────────┴─────────────┐
                  │                           │
           MARKET ORDER                  LIMIT ORDER
             ưu tiên “NGAY”                ưu tiên “GIÁ”
                  │                           │
         tiêu thụ thanh khoản        thường cung cấp thanh khoản
                  │                           │
          khớp chắc hơn                 giá được kiểm soát hơn
                  │                           │
        có thể trượt giá                có thể không được khớp
             AGGRESSIVE                     PASSIVE
```

**Cách đọc:** bắt đầu từ cùng một nhu cầu giao dịch rồi chọn nhánh theo thứ bạn ưu tiên. Nhánh trái đổi khả năng kiểm soát giá lấy tốc độ; nhánh phải đổi khả năng khớp ngay lấy quyền kiểm soát giá. “Limit order” chỉ thụ động khi nó không đặt xuyên qua phía đối diện; limit có thể trở thành **marketable** và khớp ngay.

Vì vậy, **loại lệnh không biểu thị bạn bullish hay bearish; nó biểu thị cách bạn đánh đổi giữa tốc độ khớp và mức giá khớp.**

> **Ghi nhớ:** market order chắc khớp hơn nhưng không chắc giá; limit order chắc giá hơn nhưng không chắc khớp.

Không nên nói đơn giản “buyer làm giá tăng”. Cần hỏi:

- Người mua đó là người mua chủ động (aggressive buyer) hay người mua thụ động (passive buyer)?
- Họ mua bằng lệnh thị trường hay đặt lệnh giới hạn ở phía chào mua?
- Thanh khoản chào bán có đủ hấp thụ không?

## 4.3 Giá chào mua — giá chào bán — chênh lệch (Bid — Ask — Spread)

Giả sử sổ lệnh tốt nhất đang là:

- Giá chào mua tốt nhất: 99.9, khối lượng 5,000.
- Giá chào bán tốt nhất: 100.0, khối lượng 3,000.

**Giá chào mua tốt nhất (best bid)** là giá mua cao nhất đang được treo.

**Giá chào bán tốt nhất (best ask)** là giá bán thấp nhất đang được treo.

**Chênh lệch mua-bán (spread)** là:

`Giá chào bán tốt nhất - giá chào mua tốt nhất = 100.0 - 99.9 = 0.1`

**Giá giữa (mid-price)** là:

`(Giá chào mua tốt nhất + giá chào bán tốt nhất) / 2 = 99.95`

Hai phía của sổ lệnh được ngăn bởi vùng giá chưa có sự đồng thuận:

```text
                 NGƯỜI BÁN ĐANG CHỜ
                         ASK
                          │
                  100.1 ──┤ mức bán cao hơn
                  100.0 ──┤ 3,000 CP ← BEST ASK
══════════════════════════════════════════
                   SPREAD = 0.1
                   MID    = 99.95
══════════════════════════════════════════
                   99.9 ──┤ 5,000 CP ← BEST BID
                   99.8 ──┤ mức mua thấp hơn
                          │
                         BID
                 NGƯỜI MUA ĐANG CHỜ
```

**Cách đọc:** best ask là người bán rẻ nhất ở phía trên; best bid là người mua trả cao nhất ở phía dưới. Khoảng giữa hai mức là spread. Một market buy đi lên phía ask; một market sell đi xuống phía bid.

Phát biểu chính xác: **giao dịch ngay đòi hỏi một bên vượt qua khoảng chưa đồng thuận và chấp nhận giá tốt nhất đang chờ ở phía đối diện.**

> **Ghi nhớ:** ask là giá người bán đòi; bid là giá người mua trả; spread là khoảng hai bên chưa đồng thuận.

### Tại sao chênh lệch mua-bán tồn tại?

Chênh lệch mua-bán tồn tại vì người mua và người bán không có cùng động cơ/ràng buộc.

Người mua thụ động muốn mua rẻ hơn. Người bán thụ động muốn bán đắt hơn. Market maker hoặc người cung cấp thanh khoản cũng cần được bù đắp cho:

- Rủi ro bị adverse selection: giao dịch với người có thông tin tốt hơn.
- Rủi ro inventory: sau khi mua/bán, họ còn giữ vị thế có thể lỗ.
- Chi phí vốn và vận hành.
- Rủi ro giá di chuyển trong lúc họ đang cung cấp thanh khoản.

Cơ chế:

**Bất định + rủi ro tồn kho + nhu cầu khớp ngay → người cung cấp thanh khoản cần được bù đắp → họ đặt giá chào mua thấp hơn giá chào bán → chênh lệch mua-bán xuất hiện**

Nếu chênh lệch bằng 0 trong khi rủi ro tồn tại, người cung cấp thanh khoản có thể không được bù đủ. Họ sẽ rút lệnh, giảm khối lượng, hoặc đặt giá mua/bán xa nhau hơn.

## 4.4 Sổ lệnh giới hạn (Limit Order Book)

**Sổ lệnh giới hạn (Limit Order Book, LOB)** là danh sách các lệnh giới hạn đang chờ khớp, chia theo mức giá.

Ví dụ:

| Khối lượng chào mua | Giá chào mua | Giá chào bán | Khối lượng chào bán |
|---:|---:|---:|---:|
| 4,000 | 99.8 | 100.0 | 1,000 |
| 6,000 | 99.7 | 100.1 | 2,000 |
| 8,000 | 99.6 | 100.2 | 5,000 |
| 10,000 | 99.5 | 100.3 | 8,000 |

### Các mức giá (Price levels)

Mỗi mức giá có thể có nhiều lệnh từ nhiều người khác nhau.

### Hàng chờ lệnh (Queue)

Tại cùng một mức giá, lệnh thường được xếp hàng. Ai đặt trước được ưu tiên trước, tùy quy tắc của địa điểm giao dịch.

### Độ sâu sổ lệnh (Depth)

**Độ sâu (depth)** là khối lượng có sẵn ở các mức giá.

Sổ lệnh sâu nghĩa là có nhiều thanh khoản ở nhiều mức giá. Sổ lệnh mỏng nghĩa là chỉ cần một lệnh thị trường tương đối nhỏ cũng có thể ăn qua nhiều mức giá.

### Ưu tiên theo giá và thời gian (Price-time priority)

Quy tắc phổ biến:

1. Giá tốt hơn được ưu tiên trước.
2. Nếu cùng giá, lệnh đến trước được ưu tiên trước.

Ví dụ bên bán:

- Giá chào bán 100.0 được khớp trước giá chào bán 100.1.
- Trong cùng mức chào bán 100.0, người đặt lệnh trước được khớp trước.

### Hủy lệnh (Order cancellation)

Sổ lệnh không tĩnh. Lệnh có thể bị hủy bất cứ lúc nào nếu chưa khớp.

Điều này quan trọng vì thanh khoản nhìn thấy trong sổ lệnh có thể biến mất.

Cơ chế:

**Thông tin mới / giá tiến gần / rủi ro thay đổi → người cung cấp thanh khoản hủy hoặc chỉnh lại giá đặt → độ sâu khả dụng thay đổi → tác động của lệnh thị trường lên giá thay đổi**

Nếu bạn thấy 100,000 cổ phiếu chờ bán ở 100.0, không có nghĩa chắc chắn 100,000 cổ phiếu đó vẫn ở đó khi giá chạm 100.0.

## 4.5 Khớp lệnh và hình thành giá (Matching & Price Formation)

Giá không “tự chạy”. Giá giao dịch thay đổi khi lệnh chủ động tương tác với thanh khoản có sẵn.

### Khớp lệnh (Matching)

Một lệnh mua thị trường khớp với giá chào bán tốt nhất.

Một lệnh bán thị trường khớp với giá chào mua tốt nhất.

Một lệnh giới hạn có thể khớp ngay nếu mức giá đặt khiến nó có thể giao dịch ngay, tức là **marketable**.

Ví dụ:

- Giá chào bán tốt nhất là 100.0.
- Bạn đặt lệnh giới hạn mua ở 100.2.
- Vì bạn sẵn sàng mua tới 100.2, lệnh của bạn có thể khớp ngay với giá chào bán 100.0, rồi 100.1, rồi tối đa 100.2 nếu đủ điều kiện.

### Giá giao dịch (Transaction price)

**Giá giao dịch (transaction price)** là giá thực sự của giao dịch đã khớp.

### Giá khớp gần nhất (Last price)

**Giá khớp gần nhất (last price)** là giá của giao dịch gần nhất.

Giá khớp gần nhất không nhất thiết bằng “giá trị hợp lý”. Nó chỉ là dấu vết cuối cùng của giao dịch xảy ra.

### Khám phá giá (Price discovery)

**Khám phá giá (price discovery)** là quá trình thị trường tìm mức giá mà tại đó có đủ người sẵn sàng giao dịch.

Chuỗi nhân quả:

**Thông tin/động cơ thay đổi → người tham gia cập nhật lệnh → lệnh chủ động tiêu thụ thanh khoản → một số mức giá không hấp thụ đủ → giao dịch xảy ra ở mức giá mới → thị trường quan sát giá giao dịch mới → người tham gia cập nhật niềm tin lần nữa**

## 4.6 Tác động lên giá và trượt giá (Market Impact và Slippage)

**Tác động lên giá (market impact)** là mức độ một lệnh làm giá thay đổi.

**Trượt giá (slippage)** là chênh lệch giữa giá kỳ vọng và giá khớp thực tế.

Tình huống:

Hai người cùng mua 1 tỷ đồng:

- Người A mua VNM, cổ phiếu thanh khoản cao.
- Người B mua cổ phiếu XYZ, thanh khoản rất thấp.

Tại sao tác động khác nhau?

Không phải vì 1 tỷ ở bên này “thông minh hơn”. Khác biệt nằm ở **quy mô lệnh so với thanh khoản có sẵn (size relative to liquidity)**.

Ví dụ đơn giản:

### VNM giả định

Phía chào bán trong sổ lệnh:

| Ask | Giá trị chờ bán |
|---:|---:|
| 70.00 | 5 tỷ |
| 70.10 | 8 tỷ |
| 70.20 | 10 tỷ |

Lệnh mua 1 tỷ chỉ ăn một phần lượng chào bán ở 70.00. Giá khớp gần nhất có thể vẫn là 70.00 hoặc dịch chuyển rất ít.

### XYZ giả định

Phía chào bán trong sổ lệnh:

| Ask | Giá trị chờ bán |
|---:|---:|
| 10.00 | 100 triệu |
| 10.20 | 150 triệu |
| 10.50 | 200 triệu |
| 11.00 | 300 triệu |
| 11.50 | 500 triệu |

Lệnh mua 1 tỷ có thể ăn qua nhiều mức giá. Giá khớp gần nhất có thể lên 11.50.

Cơ chế:

**Cùng giá trị giao dịch danh nghĩa → thanh khoản chào bán khả dụng khác nhau → số mức giá bị tiêu thụ khác nhau → trượt giá khác nhau → tác động lên giá khác nhau**

Điều quyết định không phải quy mô lệnh đứng riêng, mà là quy mô lệnh **so với** khả năng hấp thụ của thị trường:

```text
                    CÙNG LỆNH MUA 1 TỶ
                             │
              ┌──────────────┴──────────────┐
              │                             │
       ASK LIQUIDITY DÀY              ASK LIQUIDITY MỎNG
       ████████████████                      ██
              │                             │
       hấp thụ ở mức gần              quét qua nhiều mức
              │                             │
       trượt giá thấp hơn             trượt giá cao hơn
              │                             │
       giá ít dịch chuyển             giá dịch chuyển mạnh
```

*Các thanh ký tự chỉ minh họa độ lớn tương đối, không theo tỷ lệ định lượng.*

**Cách đọc:** từ cùng một lệnh ở trên, chọn nhánh theo độ dày thanh khoản. Mũi tên đi xuống cho thấy mức độ hấp thụ quyết định số tầng giá phải đi qua.

Đây là mô hình trực giác, không phải công thức định lượng chính xác:

```text
TÁC ĐỘNG LÊN GIÁ  ≈  QUY MÔ LỆNH CHỦ ĐỘNG / THANH KHOẢN KHẢ DỤNG
```

> **Ghi nhớ:** lệnh chủ động là lực đánh; thanh khoản là vật cản; giá dịch chuyển theo tương quan giữa hai bên.

### Tác động tạm thời và tác động lâu dài (Temporary impact vs Permanent impact)

**Tác động tạm thời (temporary impact)**: giá dịch chuyển do lệnh ăn thanh khoản tạm thời, sau đó thanh khoản quay lại và giá hồi một phần.

**Tác động lâu dài (permanent impact)**: giá thay đổi bền hơn vì lệnh phản ánh thông tin mới, hoặc thị trường cập nhật lại nhận định về giá trị.

Không thể chỉ nhìn một cú tăng giá và kết luận ngay đó là tác động lâu dài. Cần bằng chứng tiếp theo:

- Giá có giữ được vùng mới không?
- Liquidity có tái xuất hiện ở vùng cũ không?
- Volume sau đó xác nhận tiếp tục hay chỉ là một cú quét mỏng?
- Có thông tin/catalyst mới không?

## 4.7 Biến động giá (Volatility) hình thành từ đâu?

**Biến động giá (volatility)** là mức độ dao động giá. Từ góc nhìn cấu trúc vi mô, biến động giá có thể tăng khi:

- Liquidity mỏng.
- Spread rộng.
- Order imbalance kéo dài.
- Nhiều lệnh thị trường cùng hướng.
- Tin tức làm participants hủy lệnh và quote lại.
- Stop/forced orders bị kích hoạt.
- Participants không đồng thuận mạnh về giá trị.

Chuỗi nhân quả:

**Bất định tăng → thanh khoản thụ động rút đi hoặc đặt giá rộng hơn → sổ lệnh mỏng hơn → lệnh chủ động tiêu thụ các mức giá nhanh hơn → giá giao dịch nhảy mạnh hơn → biến động quan sát được tăng**

Biến động giá không chỉ là “giá chạy mạnh”. Nó thường là kết quả của sự kết hợp giữa **mức độ chủ động của lệnh (order aggression)** và **thanh khoản khả dụng (available liquidity)**.

---

# 5. Thay đổi từng biến để hiểu nhân quả

## Nếu quy mô lệnh thị trường tăng thì sao?

Giả sử phía chào bán trong sổ lệnh:

| Ask | Size |
|---:|---:|
| 100.0 | 1,000 |
| 100.1 | 2,000 |
| 100.2 | 3,000 |

Market buy 500:

- Chỉ khớp tại 100.0.
- Giá khớp gần nhất = 100.0.

Market buy 4,000:

- Ăn hết 1,000 tại 100.0.
- Ăn hết 2,000 tại 100.1.
- Ăn 1,000 tại 100.2.
- Giá khớp gần nhất = 100.2.

Kết luận đúng không phải “mua nhiều thì giá tăng”. Kết luận chính xác hơn:

**Nếu quy mô mua chủ động lớn hơn thanh khoản chào bán ở các mức giá gần nhất, lệnh phải khớp lên các mức chào bán cao hơn, làm giá giao dịch tăng.**

## Nếu thanh khoản chào bán tăng thì sao?

Ask 100.0 từ 1,000 tăng thành 10,000.

Market buy 4,000 giờ khớp toàn bộ tại 100.0.

Giá không cần tăng qua 100.0, dù có lực mua ngay.

Cơ chế:

**Thanh khoản chào bán tại giá tốt nhất tăng → cùng lệnh mua được hấp thụ ở mức hiện tại → không cần giao dịch lên giá cao hơn → tác động tức thời lên giá thấp hơn**

## Nếu thanh khoản biến mất thì sao?

Trước tin tức lớn, nhà tạo lập thị trường và người bán thụ động có thể hủy lệnh vì không muốn bị giao dịch ở giá cũ.

Phía chào bán trong sổ lệnh từ:

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

Cùng lệnh mua thị trường 3,000 giờ có thể đẩy giá khớp gần nhất lên 101.5.

Đây là lý do tin tức có thể làm giá nhảy mạnh: không chỉ vì nhiều người mua/bán, mà còn vì phía cung cấp thanh khoản rút bớt lệnh.

## Nếu lệnh thị trường không tiếp tục thì sao?

Một lệnh mua lớn có thể đẩy giá lên 100.2. Nhưng nếu sau đó không có lệnh mua mới, và người bán quay lại đặt lượng chào bán dày hơn, giá có thể không tiếp tục tăng.

Cơ chế:

**Thanh khoản bị tiêu thụ một lần → giá dịch chuyển → không có lực chủ động tiếp diễn → người bán thụ động bổ sung lại lệnh → giá chững lại hoặc quay về**

Một cú đẩy ban đầu có thể tách thành hai kịch bản rất khác:

```text
MỘT LỆNH MUA LỚN QUÉT ASK → GIÁ LÊN 100.2
                         │
             ┌───────────┴───────────┐
             │                       │
       CÓ MUA TIẾP              KHÔNG CÓ MUA TIẾP
             +                       +
      ASK tiếp tục bị ăn       ASK được bổ sung lại
             │                       │
      có thể tăng tiếp          chững hoặc quay về
```

**Cách đọc:** điểm xuất phát giống nhau, nhưng mũi tên chỉ tiếp tục theo điều kiện quan sát sau cú đẩy. Nhánh trái cần lực mua chủ động tiếp diễn; nhánh phải xuất hiện khi lực đó dừng và người bán tái cung cấp thanh khoản.

Vì vậy, **một cú tăng chỉ tạo ra mức giá mới tức thời; xu hướng chỉ có cơ sở khi lực chủ động tiếp diễn hoặc thị trường chấp nhận giao dịch ở vùng giá mới.**

> **Ghi nhớ:** cú đẩy giá không đồng nghĩa với xu hướng.

Vì vậy, “giá vừa tăng” không tự động nghĩa là “xu hướng tăng đã hình thành”.

---

# 6. ACTORS — Ai đang tham gia?

## Nhà đầu tư/cá nhân nhỏ lẻ (Retail trader/investor)

- Có thể dùng lệnh thị trường vì muốn vào/thoát nhanh.
- Có thể đặt stop-loss tạo triggered orders.
- Thường có quy mô lệnh nhỏ so với thanh khoản của cổ phiếu lớn, nhưng có thể đáng kể trong cổ phiếu rất kém thanh khoản.

**Ai ở phía đối diện? (Who is on the other side?)** Có thể là nhà đầu tư nhỏ lẻ khác, nhà tạo lập thị trường, quỹ, thuật toán, hoặc người bán bằng lệnh giới hạn.

## Nhà đầu tư tổ chức / quỹ mở / quỹ hưu trí (Institutional investor / Mutual fund / Pension fund)

- Size lớn.
- Bị ràng buộc bởi nhiệm vụ đầu tư (mandate), chuẩn so sánh (benchmark), giới hạn rủi ro (risk limit), và thanh khoản.
- Không thể tùy tiện dùng lệnh thị trường lớn nếu sổ lệnh không đủ sâu.

Nếu họ cần mua lớn, vấn đề chính không phải “muốn kéo giá”, mà là:

**Làm sao mua đủ khối lượng mà không tự làm giá xấu đi quá nhiều?**

## Quỹ phòng hộ / nhà giao dịch tự doanh (Hedge fund / Prop trader)

- Có thể giao dịch ngắn hạn hơn.
- Có thể dùng thông tin, model, statistical edge, event-driven thesis.
- Có thể vừa cung cấp thanh khoản vừa tiêu thụ thanh khoản tùy chiến lược.

## Nhà tạo lập thị trường (Market maker)

- Cung cấp giá chào mua và giá chào bán.
- Kiếm chênh lệch mua-bán hoặc phí hoàn lại (rebate), quản lý lượng tài sản đang nắm giữ (inventory).
- Có rủi ro bị giao dịch bởi người có thông tin tốt hơn.

Nhà tạo lập thị trường không nhất thiết “cố tình kéo giá”. Nhiều hành vi đặt giá mua/bán rộng hơn, hủy lệnh, giảm khối lượng có thể là phản ứng hợp lý trước rủi ro.

## Thuật toán giao dịch (Algorithm)

- Có thể là execution algo chia nhỏ lệnh lớn.
- Có thể là market-making algo.
- Có thể là arbitrage algo.
- Có thể phản ứng nhanh với sổ lệnh, khối lượng, giá và tin tức.

## Người giao dịch chênh lệch giá (Arbitrageur)

- Tìm sai lệch giá giữa tài sản liên quan.
- Có thể mua một nơi, bán nơi khác.
- Góp phần đưa giá liên quan về cân bằng tương đối.

## Cổ đông lớn (Major shareholder)

- Size rất lớn, time horizon dài.
- Nếu giao dịch, họ đối mặt với yêu cầu công bố thông tin, thanh khoản, tác động lên giá và rủi ro làm lộ tín hiệu (signal risk).

---

# 7. INCENTIVES — Mỗi bên muốn gì?

## Người gửi lệnh thị trường

- **Mục tiêu (objective)**: khớp ngay.
- **Ràng buộc (constraint)**: có thể phải chấp nhận giá xấu.
- **Chi phí (cost)**: chênh lệch mua-bán + trượt giá + tác động lên giá.
- **Rủi ro (risk)**: sổ lệnh mỏng hơn tưởng tượng.
- **Thông tin (information)**: có thể biết ít hoặc nhiều hơn thị trường.
- **Khung thời gian (time horizon)**: thường cần hành động ngay, nhưng lý do có thể ngắn hoặc dài hạn.

## Người đặt lệnh giới hạn

- **Mục tiêu (objective)**: kiểm soát giá khớp, kiếm chênh lệch mua-bán, hoặc chờ giá mong muốn.
- **Ràng buộc (constraint)**: có thể không được khớp.
- **Chi phí (cost)**: chi phí cơ hội (opportunity cost); lựa chọn bất lợi (adverse selection) nếu bị khớp ngay trước khi giá chạy ngược.
- **Rủi ro (risk)**: bị giao dịch ở thế bất lợi khi thông tin thay đổi.
- **Thông tin (information)**: quan sát sổ lệnh, dòng lệnh, biến động giá, tin tức.
- **Khung thời gian (time horizon)**: từ vài mili-giây đến nhiều ngày tùy actor.

## Nhà tạo lập thị trường (Market maker)

- **Mục tiêu (objective)**: kiếm chênh lệch mua-bán/phí hoàn lại và quản lý inventory.
- **Ràng buộc (constraint)**: phải đặt giá trong điều kiện cạnh tranh và rủi ro.
- **Chi phí (cost)**: lựa chọn bất lợi, lỗ do inventory, công nghệ, vốn.
- **Rủi ro (risk)**: lệnh thị trường một chiều liên tục khiến inventory lệch.
- **Thông tin (information)**: dòng lệnh, biến động của sổ lệnh, biến động giá.
- **Khung thời gian (time horizon)**: rất ngắn đến trung hạn tùy mô hình.

## Tổ chức lớn

Nếu bạn là một quỹ cần mua 2 triệu cổ phiếu nhưng phía chào bán gần nhất trong sổ lệnh chỉ có vài chục nghìn cổ phiếu, bạn gặp vấn đề:

- Mua quá nhanh → tự đẩy giá lên → average cost xấu.
- Mua quá chậm → lỡ cơ hội nếu giá tăng trước.
- Lộ ý định → người khác có thể front-run hoặc điều chỉnh quote.
- Chờ thanh khoản → có thể không đủ hàng.

Hành vi hợp lý có thể là chia nhỏ lệnh, dùng lệnh giới hạn, dùng thuật toán, giao dịch theo khối lượng thị trường, hoặc chờ vùng có nhiều thanh khoản. Đây là suy luận từ ràng buộc thực tế, không cần giả định họ “muốn vẽ chart”.

---

# 8. EVIDENCE — Nếu cơ chế xảy ra, ta quan sát được gì?

## Dữ liệu quan sát trực tiếp (Directly observable)

Tùy dữ liệu bạn có, có thể quan sát:

- **Giá (Price)**: giá khớp gần nhất, giá cao nhất/thấp nhất, giá đóng cửa.
- **Volume**: tổng khối lượng giao dịch.
- **Giá chào mua / giá chào bán (Bid/Ask)**: giá mua tốt nhất, giá bán tốt nhất, chênh lệch mua-bán.
- **Sổ lệnh (Order Book)**: độ sâu ở từng mức giá.
- **Danh sách giao dịch đã khớp (Time & Sales)**: từng giao dịch khớp ở giá nào, khối lượng bao nhiêu.
- **Delta / Footprint**: nếu có dữ liệu phân loại giao dịch xảy ra tại giá chào mua hay giá chào bán.
- **Volatility**: biên độ dao động.
- **Market/sector context**: thị trường chung, ngành, tin tức.

## Suy luận từ dữ liệu (Inferred)

Những thứ chỉ là suy luận:

- “Buying pressure đang mạnh.”
- “Thanh khoản tại giá chào bán bị tiêu thụ.”
- “Có thể có large buyer.”
- “Có thể nhà tạo lập thị trường rút thanh khoản.”
- “Có thể stop orders bị kích hoạt.”

Những câu sau là câu chuyện diễn giải (story) nếu không có đủ bằng chứng:

- “Cá mập đang gom hàng.”
- “Market maker cố tình kéo giá.”
- “Tổ chức lớn đang quét stop.”

## Nếu giả thuyết đúng, ta kỳ vọng thấy gì tiếp theo?

Giả thuyết: “Giá tăng vì người mua chủ động tiêu thụ thanh khoản chào bán.”

Kỳ vọng quan sát:

- Giao dịch xảy ra nhiều tại giá chào bán hoặc quét lên nhiều mức chào bán.
- Ask depth tại các mức gần bị tiêu thụ nhanh.
- Chênh lệch mua-bán có thể mở rộng nếu người cung cấp thanh khoản rút bớt.
- Giá khớp gần nhất tăng theo từng giao dịch.
- Nếu lực mua chủ động tiếp tục và người bán không bổ sung đủ lệnh, giá tiếp tục tìm mức chào bán cao hơn.

Falsification tiềm năng:

- Giá tăng nhưng khối lượng rất thấp trong sổ lệnh cực mỏng; không đủ để nói có lực mua bền.
- Ask bị tiêu thụ nhưng ngay lập tức được replenish dày và giá không giữ được.
- Giá tăng chủ yếu do gap/news repricing, không phải do order flow quan sát trong phiên.

---

# 9. ALTERNATIVE EXPLANATIONS — Cùng hiện tượng, nhiều cách giải thích

Hiện tượng: giá ABC tăng từ 100 lên 103 trong 10 phút, khối lượng tăng mạnh.

Trước khi chọn lời giải thích, hãy mở cây giả thuyết từ cùng một hiện tượng:

```text
                  GIÁ 100 → 103, VOLUME TĂNG
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
 MUA CHỦ ĐỘNG          THANH KHOẢN MỎNG       TIN TỨC ĐỊNH GIÁ LẠI
       │                      │                      │
 ask bị ăn liên tục      ít ask để hấp thụ       quote được cập nhật
                              │
                    SHORT COVERING / MUA BẮT BUỘC
```

**Cách đọc:** bắt đầu từ hiện tượng quan sát được ở trên, rồi đi xuống các cơ chế cạnh tranh. Các nhánh không loại trừ nhau; nhiều cơ chế có thể cùng xảy ra. Bảng dưới đây cung cấp bằng chứng để tăng hoặc giảm xác suất từng nhánh.

Phát biểu chính xác: **giá và volume chỉ giới hạn tập hợp lời giải thích; muốn chọn giả thuyết có xác suất cao hơn, phải tìm dấu vết riêng của cơ chế trong order flow, liquidity, tin tức và diễn biến tiếp theo.**

> **Ghi nhớ:** một hiện tượng có thể có nhiều cơ chế; bằng chứng phân biệt mới quyết định giả thuyết nào mạnh hơn.

| Giả thuyết (Hypothesis) | Cơ chế (Mechanism) | Bằng chứng ủng hộ | Bằng chứng chống lại | Nếu đúng, tiếp theo nên thấy gì? |
|---|---|---|---|---|
| Lực mua chủ động thật sự (aggressive buying) | Lệnh mua thị trường liên tục ăn thanh khoản chào bán | Nhiều giao dịch tại giá chào bán; độ sâu phía bán bị tiêu thụ; giá giữ vùng cao | Giá tăng bằng vài lệnh nhỏ trong sổ lệnh mỏng; không có lực tiếp diễn | Nếu đúng, nhịp lùi thường nông và người mua tiếp tục hấp thụ người bán |
| Cú tăng do thiếu thanh khoản (low-liquidity move) | Sổ lệnh mỏng, ít người bán nên lệnh vừa phải cũng làm giá nhảy | Độ sâu mỏng; chênh lệch rộng; khối lượng không quá lớn | Khối lượng lớn, nhiều mức chào bán dày vẫn bị ăn | Giá dễ quay về khi thanh khoản xuất hiện lại |
| Định giá lại do tin tức (news repricing) | Tin mới làm người tham gia cập nhật nhận định về giá trị | Tin/catalyst rõ; nhiều người cùng điều chỉnh giá đặt | Không có tin; ngành không phản ứng | Giá giữ vùng mới nếu thông tin được thị trường chấp nhận |
| Mua lại vị thế bán khống / mua bắt buộc (short covering / forced buying) | Người đang bán khống phải mua lại | Tăng nhanh; bối cảnh short interest/phái sinh phù hợp | Không có bằng chứng về vị thế bán khống hoặc lệnh bắt buộc | Có thể tăng nhanh rồi yếu khi lực mua bắt buộc kết thúc |

Mục tiêu không phải chọn câu chuyện hấp dẫn nhất. Mục tiêu là hỏi: **bằng chứng nào phân biệt các giả thuyết này?**

---

# 10. FALSIFICATION — Điều gì chứng minh giả thuyết yếu?

Giả thuyết chính:

> Giá tăng vì lực mua chủ động tiêu thụ thanh khoản chào bán và tạo mất cân bằng (imbalance).

Bằng chứng xác nhận (confirmation evidence):

- Nhiều giao dịch khớp tại giá chào bán.
- Các mức chào bán bị ăn liên tục.
- Nhịp lùi (pullback) có khối lượng thấp hơn và không phá vùng tăng.
- Người bán bổ sung lệnh nhưng tiếp tục bị hấp thụ.

Bằng chứng làm giả thuyết yếu đi (falsification evidence):

- Giá tăng chỉ vì chênh lệch mua-bán rộng và một vài giao dịch nhỏ.
- Sau cú tăng, không có thêm lực mua chủ động và giá quay lại ngay.
- Sổ lệnh cho thấy thanh khoản chào bán không bị tiêu thụ đáng kể; giá nhảy do cập nhật/hủy giá đặt.
- Tin tức hoặc đấu giá/tái cân bằng giải thích tốt hơn hiện tượng.
- Thị trường chung/ngành cùng tăng mạnh, khiến cách giải thích riêng về ABC yếu đi.

Thiên kiến xác nhận (confirmation bias) dễ mắc:

- Thấy giá tăng rồi đi tìm mọi dấu hiệu để chứng minh “có dòng tiền lớn”.
- Gọi mọi cú tăng khối lượng là gom hàng (accumulation).
- Bỏ qua thanh khoản mỏng.
- Bỏ qua bối cảnh thị trường/ngành.
- Kể lại câu chuyện quá hoàn hảo sau khi đã biết giá tăng.

---

# 11. APPLICATION — Dùng khái niệm này như thế nào?

Quy trình:

**Quan sát (Observe) → diễn giải (Interpret) → lập giả thuyết (Hypothesize) → dự đoán (Predict) → kiểm tra (Test) → cập nhật xác suất → quyết định**

Ví dụ:

## Quan sát (Observe)

ABC đang ở 100. Giá chào bán tốt nhất 100.1 có 5,000 cổ phiếu. Trong 2 phút, nhiều lệnh mua ăn qua 100.1, 100.2, 100.3. Khối lượng tăng. Giá khớp gần nhất lên 100.4.

## Diễn giải (Interpret)

Người mua chủ động đang tiêu thụ thanh khoản chào bán gần nhất.

## Lập giả thuyết (Hypothesize)

Giả thuyết A: lực mua thật sự đang mạnh.

Giả thuyết B: sổ lệnh mỏng nên giá dễ nhảy, không nhất thiết có cầu bền.

Giả thuyết C: tin tức hoặc chuyển động theo ngành khiến người bán rút giá chào bán và thị trường định giá lại.

## Dự đoán (Predict)

Nếu A đúng, giá có khả năng giữ vùng cao hơn, nhịp lùi gặp người mua, và người bán tại giá chào bán tiếp tục bị hấp thụ.

Nếu B đúng, khi thanh khoản quay lại, giá có thể quay về nhanh.

Nếu C đúng, cần kiểm tra tin tức và phản ứng đồng pha ở ngành/thị trường.

## Kiểm tra (Test)

Quan sát tiếp sổ lệnh, danh sách giao dịch đã khớp, khối lượng sau cú tăng, chênh lệch mua-bán và bối cảnh thị trường.

## Cập nhật xác suất (Update probability)

Không kết luận nhị phân. Cập nhật xác suất theo bằng chứng mới.

## Quyết định (Decide)

Quyết định có thể là:

- Không giao dịch vì bằng chứng chưa đủ.
- Chờ nhịp lùi để kiểm tra hấp thụ (absorption).
- Chỉ ghi nhận cơ chế cho nghiên cứu tình huống.
- Nếu giao dịch, xác định điều kiện vô hiệu hóa giả thuyết (invalidation) rõ ràng.

Khái niệm này hữu ích khi bạn muốn hiểu **giá vừa di chuyển bằng cơ chế nào**. Nó không đủ để tự tạo lợi thế giao dịch (edge) nếu thiếu bối cảnh, quản trị rủi ro, khung thời gian, kiểm định giả thuyết và kế hoạch thực thi.

---

# 12. FACT → INFERENCE → STORY

Ba tầng này khác nhau về mức độ trực tiếp và độ chắc chắn:

```text
FACT                        INFERENCE                     STORY
DỮ KIỆN                     SUY LUẬN                      CÂU CHUYỆN
  │                            │                             │
thấy trực tiếp             giải thích có xác suất        gán actor/ý định sâu
  │                            │                             │
giá, volume, bid/ask       mua chủ động, hấp thụ         “cá mập gom hàng”
  │                            │                             │
CHẮC NHẤT                  CẦN KIỂM CHỨNG                CHƯA ĐỦ BẰNG CHỨNG
```

**Cách đọc:** đi từ trái sang phải, khoảng cách với dữ liệu gốc tăng dần. Mũi chuyển tầng không làm câu sau thành sự thật; nó chỉ tạo một giả thuyết cần kiểm chứng.

Phát biểu chính xác: **Fact giới hạn những gì có thể đã xảy ra, inference đề xuất cơ chế phù hợp, còn story gán danh tính hoặc ý định mà dữ liệu thường không quan sát trực tiếp được.**

> **Ghi nhớ:** thấy được là Fact; suy ra là Inference; kể thêm động cơ là Story.

## Ví dụ 1

> Giá ABC tăng từ 100 lên 102 trong 5 phút, khối lượng gấp 3 lần trung bình 20 phút trước đó.

- **Dữ kiện (Fact)**: Giá tăng 2%; khối lượng tăng mạnh so với trung bình ngắn hạn.
- **Suy luận (Inference)**: Có thể có lực mua chủ động hoặc định giá lại do thông tin.
- **Câu chuyện (Story)**: “Cá mập đang gom hàng.” Đây chỉ là giả thuyết, chưa phải dữ kiện.

## Ví dụ 2

> Giá chào bán tốt nhất tại 50.0 có 20,000 cổ phiếu. Trong 10 giây, nhiều giao dịch khớp tại 50.0 nhưng giá không vượt lên.

- **Dữ kiện (Fact)**: Nhiều khối lượng giao dịch tại 50.0; giá chưa vượt 50.0.
- **Suy luận (Inference)**: Thanh khoản chào bán tại 50.0 đang hấp thụ các lệnh mua.
- **Câu chuyện (Story)**: “Tổ chức lớn đang xả hàng ở 50.0.” Có thể đúng, nhưng cần thêm bằng chứng.

## Ví dụ 3

> Chênh lệch mua-bán mở rộng từ 0.1 lên 0.8 ngay trước tin tức, độ sâu hai bên giảm mạnh.

- **Dữ kiện (Fact)**: Chênh lệch mua-bán rộng hơn; độ sâu nhìn thấy được giảm.
- **Suy luận (Inference)**: Người cung cấp thanh khoản có thể đang giảm rủi ro trước bất định.
- **Câu chuyện (Story)**: “Nhà tạo lập thị trường cố tình làm giá biến động.” Chưa đủ bằng chứng.

## Ví dụ 4

> Giá vượt 100, chạm 101 rất nhanh, sau đó rơi lại 99.8.

- **Dữ kiện (Fact)**: Giá phá lên 101 rồi thất bại và quay lại dưới 100.
- **Suy luận (Inference)**: Cú vượt 100 không tìm được cầu tiếp diễn, hoặc thanh khoản phía trên bị tiêu thụ rồi lực chủ động cạn.
- **Câu chuyện (Story)**: “Cá mập quét stop rồi đạp xuống.” Chỉ là một giả thuyết cần kiểm chứng.

---

# 13. Nhiều giả thuyết cho một tình huống mơ hồ (Multiple hypotheses)

Tình huống:

> Cổ phiếu DEF đi ngang quanh 80 trong cả buổi sáng. Cuối phiên, giá bất ngờ tăng lên 82 với khối lượng lớn, nhưng ngay sau đó đóng cửa ở 80.5.

Hãy tự xây ít nhất 3 giả thuyết trước khi xem bảng.

| Giả thuyết (Hypothesis) | Cơ chế (Mechanism) | Bằng chứng ủng hộ | Bằng chứng chống lại | Nếu đúng, tiếp theo nên thấy gì? |
|---|---|---|---|---|
| Lực mua chủ động thất bại | Người mua ăn lượng chào bán lên 82 nhưng không có lực tiếp diễn; người bán quay lại | Time & Sales cho thấy nhiều giao dịch mua tại giá chào bán; sau đó lượng chào bán được bổ sung mạnh | Giá giữ trên 82 và tiếp tục tăng ngày sau | Ngày sau nếu người mua yếu, vùng 82 tiếp tục là vùng cung |
| Cú nhảy do thiếu thanh khoản (low-liquidity spike) | Cuối phiên sổ lệnh mỏng; lệnh vừa phải đẩy giá lên rồi quay về | Độ sâu mỏng, chênh lệch rộng, khối lượng thực tế không quá lớn so với giá trị giao dịch | Sổ lệnh dày mà vẫn bị ăn qua nhiều mức | Giá quay về vùng cũ khi thanh khoản bình thường trở lại |
| Tin tức/tin đồn chưa được xác nhận | Một số người tham gia mua nhanh theo tin; sau đó tin không đủ mạnh | Có dấu thời gian tin tức/tin đồn tương ứng | Không có catalyst; toàn ngành không phản ứng | Giá cần phản ứng theo tin chính thức; nếu không, cú tăng yếu đi |
| Tái cân bằng hoặc thực thi bắt buộc | Lệnh cuối phiên từ cơ chế tái cân bằng hoặc mua/bán bắt buộc | Thời điểm gần đóng cửa; nhiều mã cùng có dòng lệnh bất thường | Chỉ riêng DEF có cú tăng với cơ chế riêng | Cú tăng có thể không tiếp diễn nếu chỉ là dòng lệnh kỹ thuật |

Kết luận đúng ở thời điểm này có thể là: **không đủ bằng chứng để kết luận ý định của actor chính**. Nhưng ta có thể mô tả cơ chế có thể xảy ra và lập kế hoạch kiểm chứng.

---

# 14. Nghiên cứu tình huống (Case studies)

## Case A — Trường hợp rõ: lệnh mua thị trường quét qua phía chào bán mỏng

### Dữ kiện quan sát được (Facts)

- Giá chào bán tốt nhất 100.0 có 1,000 cổ phiếu.
- Ask 100.1 có 2,000.
- Ask 100.2 có 3,000.
- Market buy 5,000 xuất hiện.
- Giá khớp gần nhất tăng từ 100.0 lên 100.2.

### Cơ chế (Mechanism)

**Lệnh mua thị trường 5,000 → khớp 1,000 tại 100.0 → khớp 2,000 tại 100.1 → khớp 2,000 tại 100.2 → giao dịch cuối cùng ở 100.2 → giá khớp gần nhất tăng**

### Người tham gia (Actors)

- Aggressive buyer.
- Người bán thụ động ở 100.0, 100.1, 100.2.
- Có thể có market maker hoặc trader thường ở phía bán.

### Động cơ/ràng buộc (Incentives)

Buyer ưu tiên khớp ngay hơn kiểm soát giá. Sellers sẵn sàng bán tại giá đã treo.

### Giả thuyết (Hypotheses)

- Buyer cần vào vị thế ngay.
- Buyer phản ứng với thông tin.
- Buyer là execution algo đang mua từng module.

### Bằng chứng (Evidence)

Danh sách giao dịch đã khớp và sổ lệnh đủ để xác nhận cơ chế khớp qua nhiều mức chào bán. Không đủ để xác nhận động cơ sâu hơn của người mua.

### Điều kiện bác bỏ (Falsification)

Nếu dữ liệu sổ lệnh cho thấy không có thanh khoản chào bán bị ăn mà giá chỉ nhảy do cập nhật giá đặt, giả thuyết “lệnh mua thị trường quét qua phía chào bán” yếu đi.

### Kết luận (Conclusion)

Có thể kết luận giá tăng do lệnh mua chủ động tiêu thụ thanh khoản chào bán. Không nên kết luận “cá mập gom hàng” nếu thiếu bằng chứng về actor, tổng khối lượng, tính lặp lại và bối cảnh.

## Case B — Phản ví dụ: giá tăng nhưng không phải lực mua mạnh

### Dữ kiện quan sát được (Facts)

- Cổ phiếu GHI có chênh lệch mua-bán rộng.
- Giá chào bán tốt nhất 20.0 có 100 cổ phiếu.
- Ask kế tiếp 20.8 có 100 cổ phiếu.
- Một lệnh mua thị trường 100 cổ phiếu khớp tại 20.8 sau khi giá chào bán 20.0 bị hủy.
- Giá khớp gần nhất tăng 4%.

### Cơ chế (Mechanism)

**Sổ lệnh mỏng + lệnh bán thấp hơn bị hủy → giá chào bán khả dụng kế tiếp nằm xa hơn → giao dịch rất nhỏ khớp ở giá cao hơn → giá khớp gần nhất nhảy lên**

### Người tham gia (Actors)

- Một buyer nhỏ.
- Người bán thụ động ít.
- Liquidity providers có thể đã rút quote.

### Động cơ/ràng buộc (Incentives)

Buyer muốn khớp ngay hoặc không kiểm soát tốt lệnh. Sellers không muốn bán gần giá cũ.

### Giả thuyết (Hypotheses)

- Giao dịch khớp giá cao do thiếu thanh khoản.
- Quote withdrawal.
- Lệnh thị trường nhỏ từ người không có thông tin đặc biệt.

### Bằng chứng (Evidence)

Khối lượng rất nhỏ, chênh lệch mua-bán rộng, độ sâu sổ lệnh mỏng.

### Điều kiện bác bỏ (Falsification)

Nếu sau đó có khối lượng lớn tiếp tục ăn lượng chào bán dày và giá giữ vùng cao, giả thuyết “chỉ là cú khớp do thiếu thanh khoản” yếu đi.

### Kết luận (Conclusion)

Bề ngoài là giá tăng mạnh, nhưng cơ chế không chứng minh cầu mạnh. Đây là phản ví dụ quan trọng: **mức tăng giá không đồng nghĩa với sức mạnh lực mua** nếu không xét thanh khoản.

## Case C — Trường hợp mơ hồ: khối lượng lớn, giá đứng yên

### Dữ kiện quan sát được (Facts)

- ABC giao dịch quanh 100.
- Volume lớn bất thường.
- Nhiều trades xảy ra tại 100.0–100.1.
- Giá không vượt 100.2.

### Cơ chế có thể (Mechanism)

- Người mua chủ động nhưng thanh khoản chào bán hấp thụ.
- Người bán chủ động nhưng thanh khoản chào mua hấp thụ.
- Hai phía cùng lớn, tạo transfer of inventory.
- Execution algo chia nhỏ lệnh.
- Tin tức làm cả người mua và người bán tham gia mạnh.

### Người tham gia (Actors)

Retail, fund, market maker, algo, arbitrageur đều có thể liên quan.

### Động cơ/ràng buộc (Incentives)

- Một bên muốn vào/thoát khối lượng lớn.
- Market maker quản lý inventory.
- Traders ngắn hạn phản ứng với vùng giá.
- Tổ chức có thể execution theo benchmark.

### Giả thuyết (Hypotheses)

1. Absorption bởi seller lớn.
2. Absorption bởi buyer lớn.
3. Two-way institutional transfer.
4. Event/news-driven churn.

### Bằng chứng cần thêm (Evidence)

- Giao dịch chủ yếu tại giá chào mua hay giá chào bán?
- Sổ lệnh có được bổ sung lại liên tục không?
- Giá phản ứng sau vùng này ra sao?
- Market/sector context thế nào?
- Có news/catalyst không?

### Điều kiện bác bỏ (Falsification)

Nếu nói “người bán lớn hấp thụ”, nhưng sau đó giá phá lên mạnh với người mua tiếp tục kiểm soát và người bán không còn bổ sung lệnh, giả thuyết đó yếu đi.

### Kết luận (Conclusion)

Không đủ bằng chứng để kết luận một câu chuyện duy nhất. Bài học đúng là xây nhiều giả thuyết và theo dõi bằng chứng phân biệt.

---

# 15. Câu hỏi Socratic

Hãy trả lời trước khi xem phần đáp án.

1. Nếu giá chào bán tốt nhất có 1,000 cổ phiếu ở 100 và bạn gửi lệnh mua thị trường 500, tại sao giá khớp gần nhất có thể không vượt 100?
2. Nếu cũng sổ lệnh đó nhưng bạn gửi lệnh mua thị trường 5,000, tại sao giá khớp gần nhất có thể tăng?
3. Tại sao lệnh giới hạn cung cấp thanh khoản còn lệnh thị trường tiêu thụ thanh khoản?
4. Chênh lệch mua-bán bù đắp rủi ro gì cho người cung cấp thanh khoản?
5. Một cú tăng giá 5% với khối lượng rất nhỏ có thể giải thích bằng cơ chế nào?
6. Nếu giá tăng mạnh nhưng sau đó quay lại ngay, giả thuyết “lực mua bền” yếu ở điểm nào?
7. Ai ở phía đối diện khi bạn mua bằng lệnh thị trường?
8. Vì sao cùng một lệnh 1 tỷ đồng tác động khác nhau ở cổ phiếu thanh khoản cao và thấp?
9. Nếu sổ lệnh hiển thị lượng chào bán rất dày nhưng khi giá tới gần thì lượng chào bán biến mất, điều đó ảnh hưởng gì tới tác động lên giá?
10. Điều gì phân biệt dữ kiện “giá tăng, khối lượng tăng” với câu chuyện “cá mập gom hàng”?
11. Nếu market maker hủy quote trước tin tức, có nhất thiết là thao túng không?
12. Bằng chứng nào khiến bạn thừa nhận “giá tăng không phải do lực mua chủ động bền”?

## Đáp án và chuỗi suy luận

1. Vì lệnh mua thị trường 500 nhỏ hơn thanh khoản chào bán 1,000 tại 100. Toàn bộ lệnh được hấp thụ ở 100, không cần giao dịch ở giá chào bán cao hơn.
2. Vì lệnh 5,000 lớn hơn thanh khoản tại giá chào bán tốt nhất. Sau khi ăn hết 100, nó phải khớp với các mức chào bán cao hơn nếu muốn hoàn tất.
3. Lệnh giới hạn nằm chờ để người khác giao dịch với nó, nên nó thêm thanh khoản. Lệnh thị trường yêu cầu giao dịch ngay với lệnh đang chờ, nên nó lấy đi thanh khoản.
4. Chênh lệch mua-bán bù cho lựa chọn bất lợi, rủi ro inventory, biến động giá, chi phí vốn và chi phí vận hành.
5. Có thể do sổ lệnh mỏng, chênh lệch mua-bán rộng, hủy giá đặt, hoặc một lệnh nhỏ khớp ở mức giá xa. Không nhất thiết là demand mạnh.
6. Nếu không có lực tiếp diễn và giá quay về nhanh, cú tăng có thể chỉ là tác động tạm thời hoặc cú dịch chuyển do thiếu thanh khoản.
7. Bạn mua từ người bán thụ động đang treo giá chào bán, có thể là nhà đầu tư nhỏ lẻ, quỹ, nhà tạo lập thị trường, thuật toán hoặc actor khác.
8. Tác động phụ thuộc vào quy mô lệnh so với thanh khoản. 1 tỷ nhỏ so với độ sâu của cổ phiếu lớn nhưng lớn so với độ sâu của cổ phiếu kém thanh khoản.
9. Thanh khoản nhìn thấy được giảm, nên cùng lệnh thị trường sẽ ăn qua nhiều mức giá hơn, làm tác động lên giá và trượt giá tăng.
10. Fact là dữ liệu quan sát được. Story là diễn giải về ý định actor. “Cá mập gom” cần bằng chứng thêm về khối lượng, tính lặp lại, hấp thụ, bối cảnh và explanation cạnh tranh.
11. Không. Hủy quote có thể là quản trị rủi ro hợp lý trước uncertainty.
12. Khối lượng thấp, sổ lệnh mỏng, thiếu lực tiếp diễn, giá quay về nhanh, hoặc cách giải thích khác như tin tức/tái cân bằng/dịch chuyển theo ngành phù hợp hơn.

---

# 16. Kiểm tra “thực sự hiểu”

## Suy luận ngược (Reverse reasoning)

Kết quả: giá nhảy từ 50 lên 52 chỉ trong vài giao dịch.

Hãy nêu ít nhất 4 cơ chế có thể:

- Lệnh mua thị trường lớn ăn qua nhiều mức chào bán.
- Thanh khoản chào bán bị hủy/rút trước khi giao dịch.
- Sổ lệnh rất mỏng nên lệnh nhỏ cũng làm giá khớp gần nhất nhảy.
- Tin tức khiến người bán đặt lại giá cao hơn.
- Stop/forced buying kích hoạt thêm lệnh mua.

## Nếu thay đổi một biến thì sao? (What-if)

Nếu thanh khoản chào bán tại giá chào bán tốt nhất tăng từ 1,000 lên 100,000, cùng một lệnh mua thị trường 5,000 sẽ thế nào?

Suy luận: lệnh có khả năng khớp toàn bộ tại giá chào bán tốt nhất, tác động lên giá thấp hơn.

## Phản ví dụ (Counterexample)

“Giá tăng mạnh nghĩa là có dòng tiền lớn vào.”

Phản ví dụ: cổ phiếu thanh khoản thấp, chênh lệch mua-bán rộng, chỉ 200 cổ phiếu khớp ở giá chào bán xa hơn làm giá khớp gần nhất tăng 5%. Giá tăng mạnh nhưng không chứng minh dòng tiền lớn.

## Điều kiện bác bỏ (Falsification)

Nhận định: “Giá tăng do người mua chủ động kiểm soát.”

Bằng chứng làm yếu nhận định:

- Giao dịch không chủ yếu xảy ra tại giá chào bán.
- Volume thấp.
- Chênh lệch mua-bán rộng và sổ lệnh mỏng.
- Giá không giữ vùng tăng.
- Sellers replenish và hấp thụ dễ dàng.
- Market/sector/news giải thích tốt hơn.

## Tự giảng lại (Teach-back)

Hãy giải thích không dùng thuật ngữ chuyên môn:

> Giá tăng khi những người muốn mua ngay mua hết hàng của người bán rẻ nhất. Khi hàng ở giá rẻ hết, ai còn muốn mua tiếp phải chấp nhận mua từ người bán đòi giá cao hơn. Giao dịch mới xảy ra ở giá cao hơn, nên bảng giá hiển thị giá cao hơn.

---

# 17. Bản đồ liên kết

## Kiến thức cần có trước

Trước bài này, chỉ cần hiểu:

- Giao dịch luôn có hai phía.
- Giá là giá của giao dịch đã xảy ra.
- Mua/bán là hành động giữa những người tham gia có mục tiêu và ràng buộc khác nhau.

## Phần nằm trước trong chuỗi nhân quả

Điều tạo ra cấu trúc vi mô thị trường (market microstructure) là nhu cầu biến ý định giao dịch thành lệnh cụ thể, rồi xử lý các lệnh đó theo quy tắc chung.

Chuỗi nhân quả:

**Người tham gia (participants) → mục tiêu khác nhau → nhu cầu thể hiện ý định mua/bán → lệnh (orders) → quy tắc thị trường → cấu trúc vi mô thị trường**

## Khái niệm hiện tại

Cấu trúc vi mô thị trường giải thích cách một lệnh trở thành giao dịch và sau đó trở thành giá quan sát được.

Chuỗi nhân quả trung tâm:

**Lệnh (orders) → sổ lệnh (order book) → khớp lệnh (matching) → giao dịch (transactions) → hình thành giá (price formation)**

Nói ngắn gọn: giá không tự thay đổi. Giá thay đổi vì lệnh mua và lệnh bán tương tác với nhau trong một cơ chế khớp lệnh cụ thể.

## Phần được giải thích tiếp theo

Từ bài này, ta có nền tảng để học:

- Thanh khoản (liquidity).
- Dòng lệnh (order flow).
- Hấp thụ (absorption).
- Cạn lực (exhaustion).
- Thực thi lệnh của tổ chức (institutional execution).
- Hỗ trợ/kháng cự từ lệnh và thanh khoản.
- Phá vỡ, nhịp lùi và thất bại phá vỡ từ cơ chế giao dịch.

## Bản đồ nối với toàn khóa

Toàn khóa đi theo chuỗi:

**Người tham gia (participants) → động cơ/ràng buộc (incentives) → lệnh (orders) → sổ lệnh (order book) → dòng lệnh (order flow) ↔ thanh khoản (liquidity) → mất cân bằng/hấp thụ (imbalance/absorption) → khám phá giá (price discovery) → giá và khối lượng (price & volume) → cấu trúc thị trường (market structure) → hành động giá (price action) → mô hình giá (patterns)**

```text
PREVIOUS / UPSTREAM                  MODULE 1                  NEXT / DOWNSTREAM

NGƯỜI THAM GIA                 LỆNH → SỔ LỆNH              DÒNG LỆNH ↔ THANH KHOẢN
       ↓                              ↓                              ↓
ĐỘNG CƠ / RÀNG BUỘC          KHỚP LỆNH → GIAO DỊCH       MẤT CÂN BẰNG / HẤP THỤ
                                      ↓                              ↓
                              GIÁ ĐƯỢC HÌNH THÀNH          CẤU TRÚC / HÀNH ĐỘNG GIÁ
```

**Cách đọc:** cột trái tạo ra quyết định, cột giữa biến quyết định thành giao dịch và giá, cột phải nghiên cứu khả năng thị trường hấp thụ dòng lệnh rồi nối sang cấu trúc giá quan sát được.

Phát biểu nhân quả: **participants với động cơ khác nhau tạo ra orders; orders tương tác với liquidity để hình thành transactions và price; chuỗi transaction lặp lại mới tạo thành order flow, market structure và price action.**

> **Ghi nhớ:** Module 1 đi từ ý định đến giá; Module 2 bắt đầu từ câu hỏi thị trường hấp thụ lệnh được bao nhiêu.

Module 1 chủ yếu bao phủ đoạn đầu:

**Người tham gia → động cơ/ràng buộc → lệnh → sổ lệnh → khớp lệnh/khám phá giá**

Phần tiếp theo, **thanh khoản (liquidity)**, xuất hiện tự nhiên vì sau khi hiểu lệnh khớp thế nào, câu hỏi kế tiếp là:

> Nếu tôi muốn giao dịch một lượng rất lớn, ai sẽ đứng phía bên kia và thị trường có đủ khả năng hấp thụ không?

---

# 18. Gate 1 — Giải thích “Tại sao giá tăng?” không dùng mô hình giá hoặc chỉ báo

Câu trả lời đạt chuẩn:

> Giá tăng khi các giao dịch thực tế bắt đầu xảy ra ở mức giá cao hơn. Điều này thường xảy ra khi lệnh mua chủ động tiêu thụ hết lượng bán đang chờ ở các mức chào bán thấp hơn, hoặc khi người bán rút/hủy thanh khoản khiến giá chào bán tốt nhất nhảy lên cao hơn. Nếu người mua vẫn cần khớp ngay, họ phải giao dịch với người bán ở mức giá cao hơn. Giá khớp gần nhất vì vậy tăng. Tuy nhiên, để biết đây là cầu bền, cú tăng do thiếu thanh khoản, định giá lại do tin tức hay mua bắt buộc, cần thêm bằng chứng về khối lượng, sổ lệnh, giao dịch tại bid/ask, chênh lệch mua-bán, bối cảnh và lực tiếp diễn.

Câu trả lời chưa đạt:

- “Giá tăng vì nhiều người mua hơn người bán.”
- “Giá tăng vì dòng tiền vào.”
- “Giá tăng vì cá mập kéo.”
- “Giá tăng vì breakout.”

Các câu này có thể là shorthand, nhưng thiếu cơ chế. Để đạt chuẩn, phải nói rõ:

**Ai đang chủ động? Họ tiêu thụ thanh khoản nào? Sổ lệnh có đủ hấp thụ không? Giao dịch xảy ra tại giá nào? Bằng chứng nào quan sát được? Cách giải thích nào khác có thể đúng?**

---

# 19. Kết thúc bài

## 1. Tóm tắt theo First Principles

1. Giá không tự di chuyển; giá thay đổi khi lệnh tương tác và giá giao dịch mới xuất hiện.
2. Lệnh thị trường ưu tiên khớp ngay và tiêu thụ thanh khoản; lệnh giới hạn ưu tiên giá và cung cấp thanh khoản.
3. Giá chào mua, giá chào bán và chênh lệch mua-bán tồn tại vì người tham gia có động cơ, rủi ro và khung thời gian khác nhau.
4. Tác động lên giá phụ thuộc vào quy mô của lệnh so với thanh khoản khả dụng, không chỉ phụ thuộc vào quy mô tuyệt đối.
5. Cùng một biến động giá có thể có nhiều cơ chế; phải phân biệt dữ kiện, suy luận và câu chuyện.

## 2. Mô hình tư duy (Mental Model)

Hãy nghĩ về giá như dấu vết của một cuộc đấu giá liên tục:

**Người muốn giao dịch ngay phải đi tới nơi có người đang chờ giao dịch. Nếu lượng chờ ở giá hiện tại không đủ, giao dịch phải đi tới giá tiếp theo.**

## 3. Không được nhầm

- Không nhầm **giá tăng** với **demand bền**.
- Không nhầm **khối lượng lớn** với **thanh khoản cao**.
- Không nhầm **giá khớp gần nhất (last price)** với **giá trị hợp lý**.
- Không nhầm **nhà tạo lập thị trường quản trị rủi ro** với **thao túng**.
- Không gọi “cá mập gom/xả” là dữ kiện nếu chỉ có biểu đồ và khối lượng cơ bản.

## 4. Tôi đã hiểu nếu...

Bạn đã hiểu phần 1 nếu có thể:

- Giải thích một lệnh mua thị trường làm giá tăng qua sổ lệnh như thế nào.
- Phân biệt lệnh thị trường, lệnh giới hạn, lệnh dừng, stop-limit và lệnh đang chờ.
- Giải thích giá chào mua, giá chào bán, chênh lệch mua-bán và giá giữa từ động cơ và rủi ro.
- Vẽ chuỗi nhân quả từ người tham gia đến giá.
- Phân biệt dữ liệu quan sát trực tiếp với suy luận.
- Đưa ra ít nhất 3 cách giải thích cho cùng một cú tăng/giảm giá.
- Nêu bằng chứng có thể chứng minh giả thuyết của bạn sai.
- Trả lời “ai ở phía đối diện?” trong mọi giao dịch.

## 5. Cầu nối sang bài tiếp theo

Sau khi hiểu rằng lệnh mua/bán làm giá thay đổi bằng cách tiêu thụ thanh khoản trong sổ lệnh, câu hỏi tiếp theo gần như bắt buộc là:

> **Thanh khoản (liquidity) thực sự là gì, nằm ở đâu, và vì sao dòng tiền lớn cần thanh khoản?**

Đó là Module 2. Nếu Module 1 giải thích **lệnh biến thành giá như thế nào**, thì Module 2 giải thích **thị trường có thể hấp thụ bao nhiêu lệnh trước khi giá phải di chuyển mạnh**.
