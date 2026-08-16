# Bài học Phần 2 — Thanh khoản (Liquidity)

## Câu hỏi trung tâm

**Nếu tôi muốn giao dịch một lượng rất lớn, ai sẽ đứng phía bên kia?**

Ở Phần 1, ta đã thấy giá không tự di chuyển. Giá thay đổi khi lệnh chủ động tiêu thụ thanh khoản đang chờ trong sổ lệnh. Câu hỏi tiếp theo gần như bắt buộc là: **thị trường có bao nhiêu khả năng hấp thụ trước khi giá phải di chuyển mạnh?**

Đó là bài toán của **thanh khoản (Liquidity)**.

Không nên hiểu thanh khoản đơn giản là “có nhiều người mua bán” hoặc “khối lượng cao”. Cách hiểu sâu hơn là:

**Một lượng lệnh muốn giao dịch ngay → gặp lượng lệnh đối ứng đang sẵn sàng giao dịch → nếu đối ứng đủ thì giá ít dịch chuyển → nếu đối ứng thiếu thì lệnh phải đi tìm đối ứng ở giá xa hơn → giá di chuyển.**

## Thuật ngữ cần nắm trước

| English term | Cách gọi tiếng Việt | Định nghĩa ngắn bằng tiếng Việt |
|---|---|---|
| Liquidity | Thanh khoản | Khả năng thị trường hấp thụ lệnh mua/bán mà không làm giá dịch chuyển quá mạnh. |
| Volume | Khối lượng giao dịch | Tổng lượng đã giao dịch trong một khoảng thời gian; là dấu vết đã xảy ra, không phải lượng đang sẵn sàng hấp thụ lệnh mới. |
| Depth | Độ sâu sổ lệnh | Khối lượng đang chờ mua/bán ở nhiều mức giá trong sổ lệnh. |
| Immediacy | Khả năng khớp ngay | Khả năng giao dịch nhanh tại hoặc gần giá hiện tại. |
| Resiliency | Khả năng hồi phục thanh khoản | Tốc độ thanh khoản quay lại sau khi bị tiêu thụ hoặc rút đi. |
| Resting Liquidity | Thanh khoản đang chờ | Lệnh giới hạn đang nằm chờ trong sổ lệnh, sẵn sàng làm phía đối ứng cho lệnh chủ động. |
| Bid Liquidity | Thanh khoản chào mua | Khối lượng lệnh mua giới hạn đang chờ ở phía bid. |
| Ask Liquidity | Thanh khoản chào bán | Khối lượng lệnh bán giới hạn đang chờ ở phía ask. |
| Liquidity Pool | Vùng thanh khoản | Vùng giá có khả năng tập trung nhiều lệnh đang chờ hoặc lệnh có thể bị kích hoạt. |
| Stop-loss | Lệnh dừng lỗ | Lệnh thoát vị thế khi giá chạm mức bất lợi đã định. |
| Buy Stop | Lệnh dừng mua | Lệnh mua được kích hoạt khi giá tăng tới một mức nhất định, thường gặp ở stop-loss của người bán khống hoặc breakout order. |
| Sell Stop | Lệnh dừng bán | Lệnh bán được kích hoạt khi giá giảm tới một mức nhất định, thường gặp ở stop-loss của người mua hoặc breakdown order. |
| Triggered Order | Lệnh được kích hoạt | Lệnh chưa hoạt động cho đến khi điều kiện giá xuất hiện. |
| Liquidity Sweep | Quét thanh khoản | Giá đi qua một vùng có nhiều lệnh, kích hoạt/khớp các lệnh đó, rồi có thể tiếp diễn hoặc đảo chiều. |
| Stop Run | Chạy/quét stop | Một dạng liquidity sweep quanh vùng có nhiều stop orders; không tự động chứng minh thao túng. |
| Liquidity Vacuum | Khoảng trống thanh khoản | Tình huống sổ lệnh mỏng hoặc thiếu đối ứng khiến giá di chuyển rất nhanh qua nhiều mức. |

---

# 1. Vấn đề mở đầu: Vì sao giá có thể chạy rất nhanh sau khi vượt một vùng?

Giả sử cổ phiếu ABC đi ngang trong nhiều giờ giữa 98 và 100.

Nhiều người quan sát vùng 100:

- Người đang mua từ 98 đặt chốt lời quanh 100.
- Người bán khống đặt stop-loss nếu giá vượt 100.2.
- Người giao dịch theo phá vỡ đặt **lệnh dừng mua (buy stop)** nếu giá vượt 100.2.
- Người bán thụ động đặt limit sell quanh 100 vì cho rằng đây là kháng cự.
- Một quỹ muốn mua 300,000 cổ phiếu nhưng không muốn đẩy giá quá nhanh.

Sổ lệnh chào bán nhìn thấy quanh 100 đang như sau:

| Giá bán (Ask) | Khối lượng đang chờ bán |
|---:|---:|
| 100.0 | 50,000 |
| 100.1 | 40,000 |
| 100.2 | 30,000 |
| 100.3 | 20,000 |
| 100.8 | 25,000 |
| 101.5 | 40,000 |

Một chuỗi lệnh mua chủ động bắt đầu xuất hiện:

- 40,000 cổ phiếu mua ngay.
- 60,000 cổ phiếu mua ngay.
- 80,000 cổ phiếu mua ngay.

Trước khi đọc tiếp, hãy tự hỏi:

1. Nếu tổng lượng mua chủ động vượt lượng bán đang chờ ở 100.0-100.3, giao dịch tiếp theo phải xảy ra ở đâu?
2. Khi giá vượt 100.2, những lệnh mới nào có thể bị kích hoạt?
3. Vì sao giá có thể nhảy nhanh từ 100.3 lên 100.8 hoặc 101.5?
4. Có phải mọi cú vượt 100 rồi quay đầu đều là “cá mập quét stop” không?
5. Khối lượng giao dịch cao quanh 100 có chắc nghĩa là thanh khoản cao không?

Bây giờ phân rã:

- Ban đầu, vùng 100 có nhiều **thanh khoản chào bán (ask liquidity)** đang chờ.
- Lệnh mua chủ động tiêu thụ dần lượng chào bán tại 100.0, 100.1, 100.2, 100.3.
- Khi 100.2 bị chạm hoặc vượt, một số buy stop có thể được kích hoạt.
- Buy stop sau khi kích hoạt thường trở thành lệnh mua chủ động hoặc lệnh mua có khả năng khớp ngay.
- Những lệnh mua mới này tiếp tục tiêu thụ thanh khoản chào bán.
- Nếu giữa 100.3 và 100.8 có ít người bán, giá phải nhảy tới mức có người bán tiếp theo.
- Nếu lực mua chỉ là tạm thời và người mua mới không tiếp tục, giá có thể quay xuống dưới 100.

Hãy theo dõi toàn bộ 180,000 cổ phiếu mua chủ động đi qua từng tầng chào bán. Sơ đồ giả định ba lệnh mua đến liên tiếp và cộng lại thành một nhu cầu 180,000 cổ phiếu:

```text
TỔNG MUA CHỦ ĐỘNG: 40,000 + 60,000 + 80,000 = 180,000 CP
                                ↓
ASK 100.0 ── có 50,000 ── dùng hết 50,000 ── ✕ HẾT
                                ↓ còn 130,000
ASK 100.1 ── có 40,000 ── dùng hết 40,000 ── ✕ HẾT
                                ↓ còn 90,000
ASK 100.2 ── có 30,000 ── dùng hết 30,000 ── ✕ HẾT
                                ↓ còn 60,000 + có thể kích hoạt buy stop
ASK 100.3 ── có 20,000 ── dùng hết 20,000 ── ✕ HẾT
                                ↓ còn 40,000; vùng giữa rất mỏng
ASK 100.8 ── có 25,000 ── dùng hết 25,000 ── ✕ HẾT
                                ↓ còn 15,000
ASK 101.5 ── có 40,000 ── dùng 15,000 ────── ● CÒN 25,000
                                ↓
                             HOÀN TẤT
                 GIÁ CUỐI = 101.5
                 GIÁ TB   ≈ 100.325
```

**Cách đọc:** bắt đầu từ tổng nhu cầu mua ở trên và đi xuống. Mỗi lần một mức ask bị dùng hết mà lệnh vẫn còn, phần chưa khớp phải đi tới mức cao hơn. Mốc 100.2 còn có thể kích hoạt lệnh dừng mua, nhưng đó là khả năng cần dữ liệu xác nhận, không phải dữ kiện chắc chắn. Giá trung bình thấp hơn giá cuối vì phần lớn khối lượng đã khớp ở các mức thấp hơn.

Phát biểu nhân quả chính xác: **khi lượng mua chủ động tích lũy vượt tổng thanh khoản chào bán gần nhất, phần lệnh còn lại phải tìm người bán ở giá xa hơn; nếu vùng trung gian mỏng và lệnh mua mới được kích hoạt, giá giao dịch có thể nhảy nhanh qua nhiều mức.**

> **Ghi nhớ:** giá chạy nhanh khi lệnh còn nhiều nhưng đối ứng ở gần đã hết.

Kết quả có thể là một trong hai:

**Kịch bản 1 — Breakout tiếp diễn**

**Thanh khoản chào bán quanh 100 bị tiêu thụ → lệnh dừng mua/lệnh mua phá vỡ được kích hoạt → lệnh mua mới xuất hiện → thanh khoản bán phía trên không đủ → giá mở rộng lên vùng cao hơn → người mua tiếp tục hấp thụ nhịp lùi**

**Kịch bản 2 — Liquidity sweep thất bại**

**Giá vượt 100 → lệnh dừng/lệnh mua phá vỡ được kích hoạt → lệnh mua mới bị người bán lớn hấp thụ → không có cầu tiếp diễn → giá rơi lại dưới 100 → người mua theo phá vỡ bị kẹt**

```text
GIÁ CHẠM / VƯỢT 100.2
          ↓
BUY STOP + LỆNH MUA PHÁ VỠ CÓ THỂ KÍCH HOẠT
          ↓
LỆNH MUA CHỦ ĐỘNG MỚI ĐÁNH VÀO ASK
          │
     ┌────┴─────────────────────┐
     │                          │
ASK KHÔNG ĐỦ HẤP THỤ       SELLER BỔ SUNG / HẤP THỤ ĐỦ
     │                          │
GIÁ TÌM ASK CAO HƠN        LỰC MUA KHÔNG TẠO TIẾN TRIỂN
     │                          │
PHÁ VỠ CÓ THỂ TIẾP DIỄN    GIÁ RƠI LẠI DƯỚI VÙNG
```

**Cách đọc:** hai nhánh có cùng điểm khởi đầu và cùng xuất hiện lệnh mua mới. Điều phân biệt kết quả không phải việc stop đã kích hoạt, mà là phía bán có hấp thụ được lượng mua mới hay không và lực mua có tiếp diễn không.

Vì vậy, **việc giá vượt một mốc chỉ tạo điều kiện xuất hiện lệnh mới; hướng đi sau đó phụ thuộc vào tương quan giữa lệnh chủ động mới và thanh khoản đối ứng được duy trì hoặc bổ sung.**

> **Ghi nhớ:** cùng một điểm kích hoạt, khả năng hấp thụ quyết định tiếp diễn hay thất bại.

Cùng một hành động giá ban đầu có thể dẫn tới hai cơ chế khác nhau. Vì vậy, bài học này không dạy “thấy quét thanh khoản thì làm gì”. Nó dạy cách hỏi:

**Thanh khoản nằm ở đâu? Loại lệnh nào đang nằm ở đó? Khi giá chạm vùng đó, lệnh nào xuất hiện? Ai hấp thụ? Giá phản ứng ra sao sau khi thanh khoản bị tiêu thụ?**

---

# 2. WHY — Tại sao thanh khoản phải tồn tại?

Thị trường tồn tại vì người mua và người bán có thể gặp nhau. Nhưng không phải mọi người đều muốn giao dịch cùng lúc, cùng giá, cùng khối lượng.

Vấn đề thực tế là:

**Một người muốn giao dịch ngay cần có ai đó sẵn sàng giao dịch ngay ở phía đối diện.**

Nếu bạn muốn mua 100 cổ phiếu, thị trường thường dễ hấp thụ. Nếu bạn muốn mua 5 triệu cổ phiếu, câu hỏi trở nên khó:

- Ai đang sẵn sàng bán?
- Họ sẵn sàng bán bao nhiêu?
- Ở giá nào?
- Nếu bạn mua hết lượng bán gần nhất, người bán tiếp theo ở đâu?
- Nếu người bán thấy bạn mua mạnh, họ có giữ lệnh bán cũ không hay nâng giá/hủy lệnh?

Thanh khoản tồn tại vì thị trường cần một “lớp đệm” giữa ý định giao dịch và chuyển động giá.

Nếu không có thanh khoản:

- Một lệnh nhỏ cũng có thể làm giá nhảy rất xa.
- Spread sẽ rộng.
- Người muốn vào/thoát vị thế phải trả chi phí lớn.
- Giá khớp gần nhất dễ trở thành tín hiệu nhiễu.
- Nhà đầu tư tổ chức gần như không thể thực thi lệnh lớn mà không tự làm xấu giá.
- Khám phá giá (price discovery) trở nên hỗn loạn vì mỗi giao dịch nhỏ có thể tạo dấu vết giá quá lớn.

Thanh khoản không làm giá “đứng yên” mãi. Nó chỉ quyết định **thị trường hấp thụ được bao nhiêu áp lực mua/bán trước khi phải tìm giá mới**.

---

# 3. WHAT — Bản chất là gì?

## Tầng 1 — Trực giác (Intuition)

Hãy hình dung thanh khoản như lượng hàng sẵn có ở một cái chợ.

Nếu bạn muốn mua 5 chai nước:

- Quầy gần nhất có 100 chai.
- Bạn mua xong mà giá không cần đổi.

Nếu bạn muốn mua 5,000 chai:

- Quầy đầu tiên không đủ.
- Bạn phải mua từ quầy thứ hai, thứ ba, thứ tư.
- Mỗi quầy có thể bán giá khác nhau.
- Khi người bán thấy bạn mua quá nhiều, họ có thể nâng giá.

Trong thị trường tài chính cũng vậy. Thanh khoản là **khả năng có đủ người đứng phía bên kia để bạn giao dịch mà giá không phải chạy quá xa**.

## Tầng 2 — Định nghĩa chuẩn (Standard)

**Thanh khoản (Liquidity)** là khả năng mua hoặc bán một tài sản với khối lượng mong muốn, trong thời gian mong muốn, tại mức giá gần với giá thị trường hiện tại, với chi phí giao dịch và tác động lên giá thấp.

Thanh khoản thường có nhiều chiều:

- **Độ chặt (Tightness)**: spread hẹp hay rộng.
- **Độ sâu (Depth)**: có bao nhiêu khối lượng ở các mức giá gần.
- **Khả năng khớp ngay (Immediacy)**: có thể giao dịch nhanh không.
- **Khả năng hồi phục (Resiliency)**: sau khi bị ăn mất, thanh khoản có quay lại nhanh không.

Một thị trường thanh khoản tốt thường có chênh lệch mua-bán hẹp, độ sâu dày, giao dịch được nhanh, và sau cú lệnh lớn thanh khoản được bổ sung lại tương đối nhanh.

Bốn chiều này trả lời bốn câu hỏi khác nhau, không thể thay thế cho nhau:

```text
                         THANH KHOẢN
                              │
          ┌───────────┬───────┴───────┬────────────┐
          │           │               │            │
      ĐỘ CHẶT       ĐỘ SÂU       KHỚP NGAY      HỒI PHỤC
     Tightness      Depth        Immediacy      Resiliency
          │           │               │            │
   spread rộng?   có bao nhiêu?   nhanh với chi   bị ăn xong có
      hay hẹp?    ở giá gần?      phí bao nhiêu?  quay lại không?
```

**Cách đọc:** bắt đầu từ khái niệm thanh khoản rồi tách sang bốn thuộc tính độc lập. Một thị trường có thể tốt ở một nhánh nhưng yếu ở nhánh khác; chẳng hạn spread hẹp nhưng depth rất mỏng.

Phát biểu chính xác: **chỉ có thể đánh giá thanh khoản khi biết đồng thời khoảng giá giao dịch, khối lượng hấp thụ, chi phí khớp ngay và tốc độ bổ sung thanh khoản sau cú lệnh.**

> **Ghi nhớ:** thanh khoản tốt phải gần, đủ, nhanh và có khả năng quay lại.

## Tầng 3 — First Principles

Ở mức nền tảng, thanh khoản là quan hệ giữa:

**Nhu cầu mua/bán ngay (aggressive demand/supply) ↔ lệnh đang chờ làm đối ứng (resting liquidity) ↔ động cơ của người cung cấp thanh khoản ↔ rủi ro khi đứng phía đối diện**

Cụ thể:

- Người muốn mua ngay cần thanh khoản chào bán.
- Người muốn bán ngay cần thanh khoản chào mua.
- Người cung cấp thanh khoản chấp nhận chờ, nhưng đòi được bù đắp bằng giá tốt hơn, chênh lệch mua-bán, khoản hoàn phí (rebate), hoặc kỳ vọng có lợi.
- Nếu rủi ro tăng, họ có thể rút lệnh, giảm khối lượng, hoặc đặt giá xa hơn.
- Khi thanh khoản mỏng, cùng một lệnh chủ động tạo tác động lên giá lớn hơn.

Chuỗi nền tảng:

**Người tham gia → động cơ/rủi ro → lệnh giới hạn → thanh khoản khả dụng → lệnh chủ động tiêu thụ thanh khoản → tác động lên giá → giá mới**

## Thanh khoản không phải là gì?

- Không phải chỉ là khối lượng giao dịch cao.
- Không phải cam kết chắc chắn rằng bạn sẽ khớp được giá hiện tại.
- Không phải thứ cố định; nó có thể xuất hiện, biến mất, hoặc dời giá rất nhanh.
- Không phải mọi vùng có nhiều thanh khoản đều là “bẫy”.
- Không phải mọi cú quét qua đỉnh/đáy đều là thao túng.
- Không phải chỉ nằm trong sổ lệnh nhìn thấy được; một phần thanh khoản có thể ẩn hoặc chỉ xuất hiện khi giá tới vùng hấp dẫn.

---

# 4. MECHANISM — Thanh khoản làm giá vận động bằng cách nào?

Đây là phần quan trọng nhất của bài.

## 4.1 Công thức cơ chế tối giản

Với lệnh mua chủ động:

**Lệnh mua thị trường → tiêu thụ thanh khoản chào bán → lượng bán gần nhất giảm → nếu chưa mua đủ thì khớp lên mức chào bán cao hơn → giá giao dịch tăng → giá khớp gần nhất tăng**

Với lệnh bán chủ động:

**Lệnh bán thị trường → tiêu thụ thanh khoản chào mua → lượng mua gần nhất giảm → nếu chưa bán đủ thì khớp xuống mức chào mua thấp hơn → giá giao dịch giảm → giá khớp gần nhất giảm**

Nhưng thanh khoản không chỉ là khối lượng tại best bid/best ask. Nó còn nằm ở:

- Độ sâu nhiều mức giá.
- Lệnh đang chờ quanh vùng kỹ thuật nhiều người quan sát.
- Stop orders chưa kích hoạt.
- Lệnh ẩn hoặc lệnh của người sẽ phản ứng khi giá tới vùng họ quan tâm.
- Khả năng thanh khoản được bổ sung lại sau khi bị tiêu thụ.

## 4.2 Thanh khoản theo giá (Price liquidity)

Một tài sản có thể thanh khoản ở giá này nhưng không thanh khoản ở giá khác.

Ví dụ sổ lệnh bán:

| Ask | Khối lượng |
|---:|---:|
| 50.00 | 100,000 |
| 50.05 | 120,000 |
| 50.10 | 80,000 |
| 50.50 | 10,000 |
| 51.00 | 5,000 |

Nếu bạn mua 50,000 cổ phiếu, thanh khoản ở 50.00 đủ. Giá ít dịch chuyển.

Nếu bạn mua 350,000 cổ phiếu, bạn ăn qua 50.00, 50.05, 50.10 và còn phải đi xa hơn. Lúc này thị trường **không còn thanh khoản tốt cho size của bạn ở vùng giá gần**.

Ví dụ này còn cho thấy một giới hạn quan trọng của dữ liệu sổ lệnh đang nhìn thấy:

```text
NHU CẦU MUA: 350,000 CP
        ↓
50.00 ── dùng 100,000 ── còn 250,000
        ↓
50.05 ── dùng 120,000 ── còn 130,000
        ↓
50.10 ── dùng  80,000 ── còn  50,000
        ↓ khoảng giá rộng hơn
50.50 ── dùng  10,000 ── còn  40,000
        ↓
51.00 ── dùng   5,000 ── còn  35,000
        ↓
VƯỢT PHẠM VI SỔ LỆNH ĐANG HIỂN THỊ
→ chưa biết giá cuối và giá trung bình nếu không có thêm dữ liệu
```

**Cách đọc:** mỗi tầng trừ lượng bán có sẵn khỏi nhu cầu còn lại. Sau 51.00 vẫn thiếu 35,000 cổ phiếu, nên không được tự giả định lệnh hoàn tất hoặc tự đặt một mức giá khớp tiếp theo.

Phát biểu chính xác: **khi nhu cầu lớn hơn tổng thanh khoản hiển thị, dữ liệu hiện có chỉ cho biết lệnh sẽ quét hết các mức đang thấy; giá cuối, giá trung bình và khả năng hoàn tất vẫn chưa xác định.**

> **Ghi nhớ:** hết sổ lệnh nhìn thấy không có nghĩa là hết lệnh; nó có nghĩa là ta hết bằng chứng.

Điểm cần nhớ:

**Thanh khoản luôn phải gắn với quy mô lệnh và mức giá.**

Không có câu “cổ phiếu này thanh khoản” một cách tuyệt đối. Câu chính xác hơn là:

> Với quy mô lệnh X, trong điều kiện thị trường Y, quanh vùng giá Z, tài sản này có đủ thanh khoản hay không?

## 4.3 Độ sâu (Depth)

**Độ sâu (depth)** là lượng khối lượng đang chờ ở nhiều mức giá.

So sánh hai sổ lệnh:

### Sổ lệnh A — Dày

| Bid size | Bid | Ask | Ask size |
|---:|---:|---:|---:|
| 80,000 | 99.9 | 100.0 | 90,000 |
| 100,000 | 99.8 | 100.1 | 110,000 |
| 120,000 | 99.7 | 100.2 | 130,000 |

### Sổ lệnh B — Mỏng

| Bid size | Bid | Ask | Ask size |
|---:|---:|---:|---:|
| 5,000 | 99.9 | 100.0 | 4,000 |
| 8,000 | 99.5 | 100.5 | 6,000 |
| 10,000 | 98.5 | 101.5 | 7,000 |

Cùng một lệnh mua thị trường 50,000 cổ phiếu:

- Ở sổ lệnh A, có thể khớp toàn bộ tại 100.0.
- Ở sổ lệnh B, có thể quét qua 100.0, 100.5, 101.5 và vẫn chưa đủ.

Kết luận:

**Tác động lên giá (market impact) không chỉ phụ thuộc quy mô tuyệt đối của lệnh; nó phụ thuộc quy mô tương đối so với độ sâu khả dụng.**

```text
                    CÙNG MARKET BUY 50,000 CP
                              │
                ┌─────────────┴─────────────┐
                │                           │
          SỔ LỆNH A DÀY               SỔ LỆNH B MỎNG
       ask gần: ███████████          ask gần: ██
                │                           │
      hấp thụ tại 100.0           quét 100.0 → 100.5 → 101.5
                │                           │
        tác động nhỏ hơn              vẫn có thể chưa đủ
```

*Thanh ký tự chỉ biểu thị độ lớn tương đối, không theo tỷ lệ chính xác.*

**Cách đọc:** từ cùng một lệnh, đi theo nhánh tương ứng với độ sâu phía ask. Ở nhánh A, lượng bán gần nhất lớn hơn lệnh; ở nhánh B, tổng lượng bán đang thấy chỉ là 17,000 cổ phiếu, nên còn 33,000 chưa khớp sau mức 101.5.

Phát biểu chính xác: **với cùng một lệnh mua, sổ lệnh càng mỏng và các mức ask càng cách xa nhau thì số tầng bị quét, trượt giá và phần lệnh chưa khớp càng lớn.**

> **Ghi nhớ:** không hỏi lệnh lớn đến đâu; hãy hỏi nó lớn đến đâu so với độ sâu phía đối diện.

## 4.4 Khả năng khớp ngay (Immediacy)

Immediacy trả lời câu hỏi:

> Tôi có thể giao dịch ngay không, và cái giá của việc “ngay” là gì?

Muốn khớp ngay, bạn phải dùng lệnh chủ động hoặc lệnh giới hạn có thể khớp ngay. Chi phí thường gồm:

- Spread.
- Slippage.
- Market impact.
- Rủi ro bị khớp qua nhiều mức giá.

Trong thị trường thanh khoản cao, immediacy rẻ hơn. Trong thị trường mỏng, immediacy đắt hơn.

Chuỗi nhân quả:

**Nhu cầu khớp ngay tăng → lệnh chủ động tăng → thanh khoản đang chờ bị tiêu thụ nhanh → nếu người cung cấp thanh khoản không bổ sung kịp → giá phải di chuyển xa hơn để tìm đối ứng**

## 4.5 Khả năng hồi phục thanh khoản (Resiliency)

Thanh khoản không chỉ là “trước khi lệnh tới có bao nhiêu khối lượng”. Một câu hỏi quan trọng hơn:

> Sau khi thanh khoản bị tiêu thụ, nó có quay lại không?

Ví dụ:

- Ask 100.0 có 50,000 cổ phiếu.
- Market buy ăn hết 50,000.
- Ngay sau đó, sellers khác bổ sung thêm 80,000 tại 100.0-100.1.

Thị trường này có resiliency tốt quanh 100. Giá khó đi xa nếu phía bán liên tục quay lại.

Ngược lại:

- Ask 100.0 bị ăn hết.
- Không ai bổ sung lại.
- Ask kế tiếp nằm ở 100.8.
- Buy orders tiếp tục.

Giá có thể nhảy nhanh.

Chuỗi:

**Thanh khoản bị tiêu thụ → người cung cấp thanh khoản đánh giá lại rủi ro → nếu họ bổ sung lệnh gần giá cũ, giá ổn định hơn → nếu họ rút hoặc đặt xa hơn, giá mở rộng biên độ**

---

# 5. Thanh khoản (Liquidity) vs Khối lượng (Volume)

Đây là nhầm lẫn cực kỳ phổ biến.

**Volume là thứ đã giao dịch. Liquidity là khả năng giao dịch tiếp theo.**

```text
KHỐI LƯỢNG GIAO DỊCH (VOLUME)       THANH KHOẢN (LIQUIDITY)
              │                                  │
       giao dịch ĐÃ xảy ra              khả năng hấp thụ lệnh MỚI
              │                                  │
       dấu vết quá khứ gần              trạng thái hiện tại, có thể đổi
              │                                  │
   không cho biết chắc phía nào       phải gắn với phía, giá, size, thời điểm
```

**Cách đọc:** hai cột trả lời hai câu hỏi khác nhau. Cột trái cho biết bao nhiêu tài sản đã đổi chủ; cột phải cho biết một lệnh mới có thể giao dịch gần giá hiện tại với chi phí nào.

Phát biểu chính xác: **volume cao chỉ chứng minh nhiều giao dịch đã xảy ra; muốn kết luận thanh khoản tốt còn phải thấy spread hẹp, đủ depth đúng phía, slippage thấp và khả năng bổ sung lệnh.**

> **Ghi nhớ:** volume nhìn lại giao dịch cũ; liquidity kiểm tra sức chứa cho lệnh kế tiếp.

Volume nhìn về quá khứ gần. Liquidity liên quan đến khả năng hấp thụ lệnh ở hiện tại và tương lai rất gần.

## 5.1 Vì sao volume cao không đồng nghĩa liquidity cao?

Ví dụ ABC giao dịch 2 triệu cổ phiếu trong 10 phút quanh vùng 100. Có vẻ rất “thanh khoản”.

Nhưng có hai kịch bản khác nhau:

### Kịch bản A — Liquidity thật sự dày

- Spread hẹp.
- Bid/ask depth dày.
- Lệnh lớn khớp được gần giá hiện tại.
- Sau khi bị ăn, depth được bổ sung lại.
- Giá không bị giật quá mạnh.

Volume cao ở đây đi kèm thanh khoản tốt.

### Kịch bản B — Volume cao nhưng liquidity căng thẳng

- Nhiều lệnh thị trường hai chiều va vào nhau.
- Spread mở rộng.
- Depth mỏng.
- Giá nhảy qua nhiều mức.
- Lệnh lớn vẫn trượt giá mạnh.

Volume cao ở đây là dấu hiệu **nhiều giao dịch đã xảy ra**, không phải bằng chứng thị trường còn đủ khả năng hấp thụ lệnh mới.

## 5.2 Một thị trường nhiều giao dịch vẫn có thể trượt giá mạnh

Giả sử trong 1 phút có 500,000 cổ phiếu giao dịch. Nhưng mỗi lần bạn muốn mua 100,000 cổ phiếu, ask liquidity gần nhất chỉ có:

| Ask | Khối lượng |
|---:|---:|
| 20.00 | 10,000 |
| 20.20 | 15,000 |
| 20.60 | 20,000 |
| 21.50 | 25,000 |

Bạn vẫn trượt giá mạnh vì **volume đã xảy ra không đảm bảo có đủ resting liquidity ở phía bạn cần ngay lúc bạn giao dịch**.

Chuỗi:

**Volume quá khứ cao → không nói rõ liquidity hiện tại nằm ở phía nào → market buy cần ask liquidity ngay bây giờ → nếu ask liquidity mỏng thì vẫn quét giá → slippage cao**

## 5.3 Cách nói chính xác hơn

Thay vì nói:

> Cổ phiếu này thanh khoản vì volume cao.

Hãy nói:

> Cổ phiếu này có volume cao. Để kết luận thanh khoản tốt, cần xem spread, depth, slippage thực tế, khả năng khớp size, và liquidity có được bổ sung lại sau khi bị tiêu thụ không.

---

# 6. Thanh khoản đang chờ (Resting Liquidity)

**Thanh khoản đang chờ (resting liquidity)** là các lệnh đang nằm chờ để người khác giao dịch với chúng.

- Limit buy đang chờ tạo **bid liquidity**.
- Limit sell đang chờ tạo **ask liquidity**.

## 6.1 Bid liquidity

Bid liquidity là lượng mua đang chờ dưới hoặc tại giá hiện tại.

Nó có thể hấp thụ market sell orders.

Ví dụ:

| Bid | Khối lượng chờ mua |
|---:|---:|
| 99.9 | 50,000 |
| 99.8 | 80,000 |
| 99.7 | 100,000 |

Market sell 40,000 có thể được hấp thụ tại 99.9.

Market sell 200,000 sẽ ăn qua 99.9, 99.8, 99.7 và có thể đẩy last price xuống thấp hơn.

## 6.2 Ask liquidity

Ask liquidity là lượng bán đang chờ trên hoặc tại giá hiện tại.

Nó có thể hấp thụ market buy orders.

Ví dụ:

| Ask | Khối lượng chờ bán |
|---:|---:|
| 100.0 | 30,000 |
| 100.1 | 70,000 |
| 100.2 | 100,000 |

Market buy 25,000 có thể được hấp thụ tại 100.0.

Market buy 150,000 sẽ ăn qua 100.0, 100.1 và một phần 100.2.

## 6.3 Sự tập trung lệnh (Order concentration)

Liquidity không phân bổ đều. Nó thường tập trung tại một số vùng giá vì người tham gia có cùng cách nhìn hoặc cùng ràng buộc.

Ví dụ:

- Số tròn như 100, 1,000, 50,000.
- Đỉnh/đáy cũ.
- Biên vùng đi ngang.
- Giá tham chiếu quan trọng.
- Vùng hỗ trợ/kháng cự nhiều người nhìn.
- Mức stop-loss phổ biến.

Không được suy luận quá nhanh:

> Có nhiều lệnh quanh đỉnh cũ, vậy chắc chắn là stop của retail.

Cách nói đúng hơn:

> Đỉnh cũ là vùng có khả năng tập trung nhiều loại lệnh: limit sell chốt lời, buy stop của người bán khống, breakout orders, lệnh của người muốn thoát khi hòa vốn, và lệnh của người tìm thanh khoản để thực thi size lớn.

## 6.4 Thanh khoản nhìn thấy và thanh khoản không nhìn thấy

Một phần liquidity nằm trong sổ lệnh hiển thị. Nhưng không phải toàn bộ.

Có thể có:

- **Visible liquidity**: lệnh đang hiển thị trong order book.
- **Hidden liquidity**: lệnh ẩn hoặc iceberg, chỉ lộ một phần.
- **Latent liquidity**: người chưa đặt lệnh nhưng sẽ tham gia nếu giá tới vùng họ quan tâm.
- **Triggered liquidity**: lệnh xuất hiện sau khi điều kiện giá kích hoạt.

```text
                    THANH KHOẢN CÓ THỂ TIẾP CẬN
                              │
          ┌───────────┬───────┴────────┬────────────┐
          │           │                │            │
      NHÌN THẤY      ẨN MỘT PHẦN     TIỀM ẨN      ĐƯỢC KÍCH HOẠT
       Visible         Hidden          Latent        Triggered
          │           │                │            │
    hiện trên sổ    iceberg/lệnh ẩn  chưa đặt lệnh  chờ giá chạm điều kiện
```

**Cách đọc:** bốn nhánh phân loại theo khả năng quan sát và thời điểm lệnh xuất hiện. Chỉ nhánh “nhìn thấy” được thể hiện đầy đủ ngay trên sổ lệnh; ba nhánh còn lại chỉ có thể quan sát một phần hoặc suy luận.

Phát biểu chính xác: **độ sâu hiển thị là dữ kiện về lệnh đang công khai, nhưng tổng khả năng hấp thụ còn phụ thuộc vào lệnh ẩn, người sẽ tham gia khi giá phù hợp và lệnh chỉ xuất hiện sau kích hoạt.**

> **Ghi nhớ:** sổ lệnh là ảnh chụp phần nổi, không phải toàn bộ sức chứa của thị trường.

Vì vậy, sổ lệnh là bằng chứng quan trọng nhưng không phải bản đồ hoàn chỉnh của toàn bộ thanh khoản.

---

# 7. Vùng thanh khoản (Liquidity Pools)

**Vùng thanh khoản (liquidity pool)** là vùng giá có khả năng tập trung nhiều lệnh có thể khớp hoặc bị kích hoạt.

Từ “pool” dễ gây hiểu nhầm như thể có một cái hồ lệnh cố định. Thực tế, đây là một giả thuyết về nơi nhiều người có thể đặt lệnh vì cùng quan sát một vùng giá.

## 7.1 Đỉnh trước đó (Previous High)

Giả sử ABC có đỉnh cũ tại 100.

Quanh 100 có thể có:

- Limit sell của người đang nắm hàng muốn chốt lời.
- Lệnh bán của người tin 100 là kháng cự.
- Buy stop của người đang bán khống, vì họ phải mua lại nếu giá vượt 100.
- Breakout buy stop của người muốn mua khi giá phá đỉnh.
- Lệnh của người mua trước đó muốn thoát hòa vốn nếu từng kẹt ở vùng 100.

Khi giá tiến tới 100:

**Giá tiến gần đỉnh cũ → nhiều actor có kế hoạch hành động quanh vùng đó → resting/triggered orders tập trung → giao dịch tăng → vùng này trở thành nơi kiểm tra khả năng hấp thụ**

```text
                         VÙNG ĐỈNH CŨ 100
                                │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
LIMIT SELL / CHỐT LỜI     BUY STOP CỦA SHORT      LỆNH MUA PHÁ VỠ
   thanh khoản bán         mua lại để thoát          mua khi xác nhận
       │                        │                        │
       └─────────────── LỆNH TẬP TRUNG ────────────────┘
                                ↓ giá chạm vùng
                     KIỂM TRA KHẢ NĂNG HẤP THỤ
                         │                    │
                  seller hấp thụ        ask bị tiêu thụ
                         ↓                    ↓
                  bị từ chối/đảo chiều   có thể phá vỡ tiếp
```

**Cách đọc:** các nhánh trên cùng là những loại lệnh khác nhau có thể cùng tập trung quanh 100. Khi giá tới vùng, kết quả tách làm hai theo việc thanh khoản bán hấp thụ được lực mua mới hay bị tiêu thụ.

Phát biểu chính xác: **đỉnh cũ có thể quan trọng vì nhiều kế hoạch giao dịch hội tụ ở đó; phản ứng giá chỉ được quyết định khi các lệnh thực sự xuất hiện và tương tác, không phải vì bản thân đường giá có sức mạnh.**

> **Ghi nhớ:** vùng giá chỉ là địa chỉ; lệnh và khả năng hấp thụ mới tạo ra phản ứng.

Nếu ask liquidity tại 100 hấp thụ hết lực mua, giá có thể bị chặn.

Nếu ask liquidity bị tiêu thụ và buy stops kích hoạt thêm lực mua, giá có thể breakout.

Nếu giá phá lên nhưng bị bán mạnh hấp thụ rồi quay xuống, đó có thể là failed breakout hoặc liquidity sweep.

## 7.2 Đáy trước đó (Previous Low)

Quanh đáy cũ có thể có:

- Limit buy của người muốn bắt đáy.
- Sell stop của người đang nắm vị thế mua đặt dừng lỗ.
- Breakdown sell orders của người muốn bán khi phá đáy.
- Buy-to-cover orders của người bán khống muốn chốt lời quanh đáy.
- Lệnh của người muốn thoát hòa vốn nếu từng bán/mua quanh vùng đó.

Khi giá phá đáy:

**Sell stops kích hoạt → market sell orders tăng → bid liquidity bị tiêu thụ → nếu không đủ người mua hấp thụ, giá giảm nhanh → nếu người mua lớn hấp thụ và giá phục hồi, cú phá đáy có thể trở thành sweep**

## 7.3 Biên vùng đi ngang (Range boundary)

Một vùng đi ngang tạo ký ức thị trường.

- Gần biên trên, người mua trong range muốn chốt lời, người bán muốn short, breakout traders muốn mua nếu phá lên.
- Gần biên dưới, người bán muốn chốt lời, người mua muốn bắt nhịp hồi, breakdown traders muốn bán nếu phá xuống.

Vì vậy, range boundary thường là vùng có nhiều quyết định cùng chờ một tín hiệu giá.

Nhưng range không tự động nghĩa là “gom hàng” hay “phân phối”. Nó chỉ nói rằng trong một khoảng thời gian, thị trường chưa tìm được lý do đủ mạnh để rời vùng giá đó một cách bền vững.

## 7.4 Hỗ trợ/kháng cự (Support/Resistance)

Ở phần sau ta sẽ học sâu hơn, nhưng từ góc liquidity:

- Hỗ trợ có thể là vùng bid liquidity xuất hiện hoặc sell orders được hấp thụ.
- Kháng cự có thể là vùng ask liquidity xuất hiện hoặc buy orders bị hấp thụ.

Không nên xem support/resistance là đường kẻ thần kỳ. Hãy hỏi:

**Tại vùng này, ai muốn giao dịch? Loại lệnh nào có thể nằm ở đó? Nếu giá chạm vùng này, lệnh nào bị kích hoạt? Ai hấp thụ ai?**

## 7.5 Số tròn (Round numbers)

Số tròn tập trung lệnh vì con người thích mốc dễ nhớ và hệ thống giao dịch thường dùng ngưỡng đơn giản.

Ví dụ quanh 100:

- Chốt lời tại 100.
- Stop-loss tại 99.9 hoặc 100.1.
- Breakout order trên 100.
- Limit sell tại 100.

Số tròn không có sức mạnh tự nhiên. Nó quan trọng vì nhiều người có thể cùng dùng nó làm điểm ra quyết định.

---

# 8. Lệnh dừng và lệnh được kích hoạt (Stops and Triggered Orders)

Điểm đặc biệt của stops là chúng không nhất thiết nằm sẵn trong sổ lệnh như resting limit orders. Chúng có thể “ngủ” cho đến khi giá chạm điều kiện.

## 8.1 Stop-loss

Stop-loss là lệnh thoát vị thế khi giá đi ngược quá mức chấp nhận.

Nếu bạn đang mua ABC tại 100 và đặt stop-loss tại 97:

- Khi giá chưa chạm 97, lệnh có thể chưa tham gia thị trường.
- Khi giá chạm 97, lệnh bán được kích hoạt.
- Nếu là stop-market, nó trở thành market sell.
- Nếu bid liquidity tại 97 mỏng, bạn có thể bị khớp thấp hơn nhiều.

Chuỗi:

**Giá giảm tới stop level → stop-loss kích hoạt → sell orders mới xuất hiện → bid liquidity bị tiêu thụ → nếu bid không đủ, giá giảm tiếp → stop khác có thể kích hoạt**

```text
GIÁ HIỆN TẠI: 100
       ↓ giá giảm
════════════ STOP PRICE: 97 ════════════
       ↓ điều kiện được chạm
STOP ĐANG NGỦ → MARKET SELL / LIMIT SELL
       ↓ nếu trở thành lệnh bán chủ động
BID LIQUIDITY BỊ TIÊU THỤ
       │
       ├── BID ĐỦ       → giá được hấp thụ gần 97
       └── BID KHÔNG ĐỦ → giá giảm tiếp → có thể kích hoạt stop khác
```

**Cách đọc:** lệnh stop không tham gia trước đường ranh giới 97. Sau khi giá chạm điều kiện, loại lệnh được sinh ra và độ dày bid quyết định việc giá ổn định hay tiếp tục giảm.

Phát biểu chính xác: **stop chỉ khuếch đại chuyển động khi nó được kích hoạt thành lệnh có khả năng khớp ngay và thanh khoản phía đối diện không đủ hấp thụ; việc có stop không tự động làm giá giảm sâu.**

> **Ghi nhớ:** stop là lệnh đang ngủ; trigger đánh thức nó; thanh khoản quyết định hậu quả.

## 8.2 Buy stop

Buy stop thường xuất hiện trong hai trường hợp:

1. Người đang bán khống đặt stop-loss phía trên giá hiện tại.
2. Người muốn mua breakout đặt lệnh mua khi giá vượt một mức xác nhận.

Khi giá vượt đỉnh cũ:

**Giá chạm vùng buy stop → lệnh mua được kích hoạt → ask liquidity bị tiêu thụ → giá có thể tăng nhanh nếu phía bán không đủ hấp thụ**

## 8.3 Sell stop

Sell stop thường xuất hiện trong hai trường hợp:

1. Người đang mua đặt stop-loss phía dưới giá hiện tại.
2. Người muốn bán breakdown đặt lệnh bán khi giá phá một mức hỗ trợ.

Khi giá phá đáy cũ:

**Giá chạm vùng sell stop → lệnh bán được kích hoạt → bid liquidity bị tiêu thụ → giá có thể giảm nhanh nếu phía mua không đủ hấp thụ**

## 8.4 Forced orders

Không phải mọi lệnh kích hoạt đều là stop tự nguyện.

Có thể có:

- Margin call.
- Thanh lý bắt buộc (forced liquidation).
- Rebalancing.
- Risk limit của quỹ.
- Hedging tự động.

Các lệnh này quan trọng vì actor không còn tối ưu “giá đẹp” như bình thường. Họ cần giảm rủi ro hoặc tuân thủ ràng buộc.

Chuỗi:

**Ràng buộc bị vi phạm → bắt buộc mua/bán → nhu cầu immediacy tăng → lệnh chủ động tăng → liquidity bị tiêu thụ nhanh → giá di chuyển mạnh hơn**

---

# 9. Quét thanh khoản (Liquidity Sweep) / Stop Run

**Liquidity sweep** là hiện tượng giá đi tới hoặc đi qua vùng có nhiều lệnh, khớp/kích hoạt các lệnh đó, rồi thị trường phản ứng tiếp.

Điểm quan trọng: sweep là mô tả hiện tượng và cơ chế có thể, không phải bằng chứng tự động về ý đồ thao túng.

## 9.1 Hai giả thuyết bắt buộc phải xét

Hai giả thuyết dưới đây có thể tạo cùng một hình dạng giá nhưng khác hẳn về ý định:

```text
                    GIÁ ĐI QUA VÙNG CÓ NHIỀU STOP
                                  │
                 ┌────────────────┴────────────────┐
                 │                                 │
      CHỦ ĐỘNG TÌM THANH KHOẢN          DI CHUYỂN TỰ NHIÊN THEO LỆNH
                 │                                 │
 actor cần đối ứng để khớp size       imbalance ăn hết liquidity gần
                 │                                 │
 có yếu tố chiến lược/ý định          không cần giả định người “săn stop”
                 └────────────────┬────────────────┘
                                  ↓
                     CẦN BẰNG CHỨNG PHÂN BIỆT
```

**Cách đọc:** bắt đầu từ cùng một dữ kiện ở trên rồi tách hai cách giải thích. Nhánh trái gán hành vi chiến lược nên đòi hỏi bằng chứng mạnh hơn; nhánh phải chỉ cần cơ chế khớp lệnh và mất cân bằng.

Phát biểu chính xác: **việc giá đi qua vùng stop chứng minh vùng giá đã được giao dịch, nhưng không tự nó chứng minh một actor cố tình đưa giá tới đó.**

> **Ghi nhớ:** sweep là hiện tượng quan sát; “săn stop” là giả thuyết về ý định.

### Giả thuyết A — Có người chủ động tìm thanh khoản

Một người tham gia lớn muốn mua/bán size lớn. Họ biết rằng để khớp size, họ cần phía đối diện.

Nếu họ muốn bán lớn, họ cần người mua. Người mua có thể xuất hiện nhiều hơn:

- Khi giá breakout lên và kích hoạt buy stops.
- Khi breakout traders mua vào.
- Khi người bán khống phải mua lại.

Vì vậy, họ có thể chờ hoặc giao dịch quanh vùng có nhiều buy liquidity để bán vào.

Chuỗi:

**Actor lớn cần bán size → cần buy liquidity → vùng trên đỉnh cũ có thể kích hoạt buy stops/breakout buys → giá đẩy lên vùng đó hoặc chờ giá tới đó → buy orders xuất hiện → actor lớn bán vào → nếu mua mới bị hấp thụ hết, giá quay xuống**

### Giả thuyết B — Giá đơn giản di chuyển tới vùng có nhiều lệnh do cơ chế thị trường

Không cần giả định ai cố tình “đi săn stop”. Giá có thể đi tới vùng thanh khoản vì:

- Lực mua/bán hiện tại đủ mạnh.
- Sổ lệnh giữa các vùng mỏng.
- Nhiều người cùng phản ứng với cùng mốc giá.
- Volatility tự nhiên đưa giá qua vùng stop.
- Tin tức làm thị trường định giá lại.

Chuỗi:

**Order imbalance xuất hiện → liquidity gần nhất bị tiêu thụ → giá di chuyển tới mức tiếp theo có đối ứng → vùng stop bị chạm → triggered orders xuất hiện → giá mở rộng hoặc đảo chiều tùy khả năng hấp thụ**

## 9.2 Một cú sweep lên trên đỉnh cũ có thể là gì?

Hiện tượng:

> Giá vượt đỉnh cũ 100, lên 101, sau đó rơi lại 99.

Các giả thuyết:

| Giả thuyết | Cơ chế | Bằng chứng ủng hộ | Bằng chứng làm yếu |
|---|---|---|---|
| Breakout thất bại | Buy stops kích hoạt nhưng không có demand tiếp diễn | Giá vượt đỉnh, volume tăng, rồi đóng lại dưới vùng breakout | Giá nhanh chóng lấy lại 100 và giữ trên đó |
| Seller lớn hấp thụ | Người bán dùng buy liquidity phía trên đỉnh để bán size | Nhiều giao dịch tại ask nhưng giá không tiếp diễn; sell liquidity bổ sung dày; giá đảo chiều mạnh | Không thấy hấp thụ; giá tiếp tục tăng cùng demand mới |
| Low-liquidity spike | Sổ lệnh mỏng nên giá nhảy qua đỉnh bằng volume nhỏ | Spread rộng, depth mỏng, volume không đáng kể | Volume lớn ăn qua nhiều mức dày |
| News repricing rồi bị phủ nhận | Tin/tin đồn kéo giá vượt đỉnh, sau đó thông tin yếu đi | Có catalyst theo thời gian | Không có catalyst; chỉ riêng vùng kỹ thuật phản ứng |

## 9.3 Một cú sweep xuống dưới đáy cũ có thể là gì?

Hiện tượng:

> Giá phá đáy 95, rơi xuống 94.2, sau đó phục hồi lên 96.

Các giả thuyết:

- Sell stops của người mua bị kích hoạt rồi được buyer lớn hấp thụ.
- Breakdown traders bán theo phá đáy nhưng không có follow-through.
- Sổ lệnh dưới đáy mỏng nên giá rơi nhanh rồi hồi khi bid liquidity quay lại.
- Tin xấu ban đầu bị thị trường đánh giá lại là không nghiêm trọng.
- Forced selling tạm thời kết thúc sau khi thanh lý xong.

Không nên kết luận ngay “spring” hay “cá mập quét stop”. Cần bằng chứng:

- Volume ở vùng dưới đáy thế nào?
- Giá phục hồi nhanh hay chậm?
- Có absorption ở bid không?
- Sau phục hồi, giá có giữ được trên đáy cũ không?
- Thị trường/ngành có cùng hành vi không?

---

# 10. Khoảng trống thanh khoản (Liquidity Vacuum)

**Liquidity vacuum** xảy ra khi giữa vùng giá hiện tại và vùng đối ứng tiếp theo có rất ít thanh khoản. Giá có thể di chuyển nhanh không phải vì lệnh cực lớn, mà vì không có đủ lệnh đối diện ở các mức trung gian.

## 10.1 Sổ lệnh mỏng (Thin book)

Ví dụ phía bid:

| Bid | Khối lượng |
|---:|---:|
| 100.0 | 5,000 |
| 99.8 | 3,000 |
| 99.0 | 4,000 |
| 97.5 | 10,000 |

Market sell 15,000 có thể làm giá khớp từ 100 xuống 97.5.

```text
MARKET SELL: 15,000 CP
        ↓
BID 100.0 ── dùng 5,000 ── còn 10,000 ── ✕ HẾT
        ↓
BID  99.8 ── dùng 3,000 ── còn  7,000 ── ✕ HẾT
        ↓ khoảng giá rộng
BID  99.0 ── dùng 4,000 ── còn  3,000 ── ✕ HẾT
        ↓ khoảng giá rất rộng
BID  97.5 ── dùng 3,000 ── hoàn tất
        ↓
GIÁ CUỐI = 97.5     GIÁ TB ≈ 99.193
```

**Cách đọc:** lệnh bán đi từ bid cao nhất xuống thấp hơn. Mỗi mức bị dùng hết vì lượng bán còn lại lớn hơn lượng mua đang chờ. Khoảng cách lớn giữa 99.0 và 97.5 làm giá cuối giảm mạnh dù chỉ 3,000 cổ phiếu khớp ở mức cuối.

Phát biểu nhân quả chính xác: **khi market sell lớn hơn tổng bid gần nhất và các mức bid cách xa nhau, phần lệnh còn lại phải bán xuống giá thấp hơn, khiến last price giảm mạnh hơn giá khớp trung bình.**

> **Ghi nhớ:** vacuum không cần lực cực lớn; nó cần quá ít đối ứng ở giữa.

Không cần một “dòng tiền bán khổng lồ”. Chỉ cần lệnh bán lớn hơn lượng bid mỏng gần nhất.

## 10.2 Khoảng trống giữa các mức giá

Nếu các mức giá có thanh khoản nằm xa nhau, last price có thể nhảy.

Chuỗi:

**Ít resting orders ở vùng trung gian → lệnh chủ động không tìm thấy đối ứng gần → phải khớp ở mức xa hơn → transaction price nhảy → volatility quan sát được tăng**

## 10.3 Vì sao tin tức tạo liquidity vacuum?

Trước hoặc ngay sau tin tức, người cung cấp thanh khoản sợ bị giao dịch ở giá cũ khi giá trị hợp lý đã thay đổi.

Họ có thể:

- Hủy lệnh.
- Giảm size.
- Mở rộng spread.
- Đặt quote xa hơn.

Chuỗi:

**Bất định tăng → adverse selection risk tăng → liquidity providers rút/quote rộng hơn → order book mỏng → lệnh chủ động quét xa hơn → giá nhảy mạnh**

Vì vậy, sau tin tức giá nhảy không chỉ vì “nhiều người mua/bán”. Nó còn vì phía đối ứng cũ không muốn đứng đó nữa.

---

# 11. Thanh khoản và khám phá giá (Liquidity and Price Discovery)

Khám phá giá là quá trình thị trường tìm mức giá mà tại đó có đủ người sẵn sàng giao dịch.

Thanh khoản quyết định quá trình đó diễn ra mượt hay giật.

Chuỗi nhân quả cốt lõi của phần 2:

**Thanh khoản khả dụng (available liquidity) → lệnh tiêu thụ thanh khoản (order consumption) → mất cân bằng (imbalance) → tìm phía đối ứng (search for counterparties) → giá mới (new price)**

## 11.1 Khi liquidity dày

Nếu liquidity dày:

- Lệnh chủ động được hấp thụ gần giá hiện tại.
- Spread thường hẹp hơn.
- Giá di chuyển từng bước nhỏ hơn.
- Một cú lệnh đơn lẻ ít tạo dấu vết giá quá lớn.
- Price discovery chậm và mượt hơn.

## 11.2 Khi liquidity mỏng

Nếu liquidity mỏng:

- Cùng size lệnh tạo impact lớn hơn.
- Spread có thể rộng.
- Giá dễ nhảy qua nhiều mức.
- Volatility tăng.
- Last price dễ gây hiểu nhầm về “giá trị thật”.

## 11.3 Khi liquidity chuyển vùng

Liquidity không chỉ dày/mỏng; nó còn có thể di chuyển.

Ví dụ:

- Trước tin xấu, bid liquidity ở 100 biến mất.
- Buyers chỉ sẵn sàng mua ở 95.
- Market sell orders phải đi xuống 95 để tìm đủ đối ứng.

Chuỗi:

**Thông tin mới → người mua không còn muốn mua ở giá cũ → bid liquidity rút xuống thấp hơn → sellers cần thoát ngay → giao dịch xảy ra ở vùng bid mới → giá định lại thấp hơn**

Đây là price discovery qua thanh khoản.

---

# 12. ACTORS — Ai tham gia vào bài toán thanh khoản?

## Nhà đầu tư nhỏ lẻ (Retail trader/investor)

Retail thường tạo nhiều loại lệnh quanh vùng dễ nhìn:

- Stop-loss dưới đáy gần nhất.
- Buy stop trên đỉnh gần nhất.
- Chốt lời tại số tròn.
- Limit buy/sell quanh support/resistance.

Retail không phải lúc nào cũng “bị săn”. Nhưng vì nhiều người dùng mốc giống nhau, lệnh của họ có thể tập trung thành liquidity pool.

## Nhà đầu tư tổ chức (Institutional investor)

Tổ chức lớn quan tâm thanh khoản vì size của họ lớn so với thị trường.

Họ hỏi:

- Nếu mua ngay, impact bao nhiêu?
- Có đủ ask liquidity không?
- Có nên chia nhỏ lệnh không?
- Có vùng nào có nhiều người bán tự nhiên không?
- Nếu lộ ý định, người khác có điều chỉnh giá chống lại mình không?

Tổ chức không nhất thiết muốn “đẩy giá”. Thường họ muốn giao dịch đủ size với chi phí thấp nhất có thể.

## Nhà tạo lập thị trường (Market maker)

Market maker cung cấp bid/ask liquidity nhưng không miễn phí.

Họ quản lý:

- Spread.
- Inventory.
- Adverse selection.
- Volatility.
- Tốc độ dòng lệnh.

Khi rủi ro tăng, họ có thể rút liquidity. Đó có thể là quản trị rủi ro, không nhất thiết là thao túng.

## Thuật toán giao dịch (Algorithm)

Algorithm có thể:

- Chia nhỏ lệnh lớn.
- Tìm thời điểm có liquidity.
- Đọc biến động depth/spread.
- Rút quote khi rủi ro tăng.
- Kích hoạt giao dịch khi phá vùng.

Trong thị trường hiện đại, liquidity có thể thay đổi rất nhanh vì thuật toán phản ứng nhanh.

## Arbitrageur

Arbitrageur cung cấp hoặc tiêu thụ liquidity khi giá tài sản liên quan lệch nhau.

Họ có thể giúp thị trường hồi phục thanh khoản, nhưng khi rủi ro cao hoặc correlation đứt gãy, họ cũng có thể rút lui.

## Người bị buộc phải giao dịch

Nhóm này gồm:

- Người bị margin call.
- Quỹ phải giảm rủi ro.
- Người phải hedge.
- Người bị stop-loss.
- Dòng tiền tái cân bằng.

Họ quan trọng vì họ thường ưu tiên thực thi hơn giá đẹp. Khi họ xuất hiện cùng chiều, liquidity có thể bị tiêu thụ rất nhanh.

---

# 13. INCENTIVES — Mỗi bên muốn gì?

## Người muốn giao dịch ngay

- **Mục tiêu (objective)**: khớp nhanh.
- **Ràng buộc (constraint)**: cần có đối ứng.
- **Chi phí (cost)**: spread, slippage, market impact.
- **Rủi ro (risk)**: depth biến mất hoặc stop cascade.
- **Thông tin (information)**: có thể phản ứng với tin, tín hiệu, hoặc ràng buộc nội bộ.
- **Khung thời gian (time horizon)**: thường ngắn ở cấp thực thi.

## Người cung cấp thanh khoản

- **Mục tiêu**: bán cao hơn/mua thấp hơn, kiếm spread, thực thi vị thế ở giá mong muốn.
- **Ràng buộc**: có thể bị khớp đúng lúc bất lợi.
- **Chi phí**: cơ hội bị bỏ lỡ nếu không khớp; adverse selection nếu khớp với người biết nhiều hơn.
- **Rủi ro**: giữ inventory sai hướng.
- **Thông tin**: quan sát flow, volatility, tin tức, tương quan thị trường.
- **Khung thời gian**: từ mili-giây đến dài hạn.

## Người đặt stop

- **Mục tiêu**: giới hạn lỗ hoặc xác nhận breakout/breakdown.
- **Ràng buộc**: stop chỉ hoạt động sau khi giá chạm mức.
- **Chi phí**: có thể bị khớp xấu khi liquidity mỏng.
- **Rủi ro**: bị kích hoạt bởi cú quét tạm thời rồi giá quay lại.
- **Thông tin**: thường dùng mốc giá dễ quan sát.
- **Khung thời gian**: phụ thuộc chiến lược.

## Tổ chức cần size lớn

- **Mục tiêu**: hoàn thành giao dịch lớn.
- **Ràng buộc**: không muốn tự đẩy giá quá xa, không muốn lộ ý định.
- **Chi phí**: implementation shortfall, market impact, opportunity cost.
- **Rủi ro**: không mua/bán đủ, bị front-run, liquidity biến mất.
- **Thông tin**: có thể có thesis đầu tư hoặc nhu cầu tái cân bằng.
- **Khung thời gian**: từ trong ngày tới nhiều tuần/tháng.

---

# 14. EVIDENCE — Ta quan sát được gì?

## Dữ liệu quan sát trực tiếp

Tùy thị trường và nguồn dữ liệu, có thể quan sát:

- **Price**: giá khớp gần nhất, high/low, close.
- **Volume**: khối lượng đã giao dịch.
- **Bid/Ask**: best bid, best ask, spread.
- **Order Book / Depth of Market**: thanh khoản đang chờ ở nhiều mức giá.
- **Time & Sales**: giao dịch đã khớp theo thời gian.
- **Footprint / Bid-Ask volume**: nếu có, xem giao dịch xảy ra tại bid hay ask.
- **Volatility**: biên độ và tốc độ di chuyển.
- **Context**: tin tức, thị trường chung, ngành, thời điểm mở/đóng cửa, đáo hạn phái sinh, tái cân bằng.

## Dữ liệu không quan sát trực tiếp hoặc chỉ quan sát một phần

- Stop orders chưa kích hoạt.
- Hidden liquidity.
- Iceberg orders.
- Ý định thật của tổ chức lớn.
- Lý do actor hủy lệnh.
- Toàn bộ vị thế đang nắm giữ của người tham gia.

Vì vậy, khi nói về liquidity pools hoặc stops, nhiều phần là suy luận. Suy luận có thể hữu ích, nhưng phải tách khỏi dữ kiện.

## Nếu giả thuyết liquidity sweep đúng, kỳ vọng thấy gì?

Ví dụ giả thuyết:

> Giá vượt đỉnh cũ để kích hoạt buy stops, sau đó lực mua mới bị hấp thụ và giá đảo chiều.

Kỳ vọng quan sát:

- Giá vượt nhẹ hoặc nhanh qua đỉnh cũ.
- Volume tăng quanh vùng vượt.
- Có dấu hiệu lệnh mua chủ động xuất hiện.
- Giá không tiếp tục mở rộng sau khi stops bị kích hoạt.
- Người bán bổ sung/duy trì ask liquidity.
- Giá quay lại dưới đỉnh cũ và không lấy lại được vùng đó.

Bằng chứng làm yếu:

- Giá giữ trên đỉnh cũ.
- Nhịp lùi nông, volume bán yếu.
- Người mua tiếp tục hấp thụ người bán.
- Market/sector context ủng hộ breakout thật.
- Không có dấu hiệu thất bại sau khi kích hoạt lệnh.

---

# 15. ALTERNATIVE EXPLANATIONS — Cùng hiện tượng, nhiều cách giải thích

Hiện tượng:

> Giá ABC vượt đỉnh 100, lên 101.2 rất nhanh, volume tăng mạnh, rồi đóng cửa ở 100.8.

```text
                   VƯỢT 100 → 101.2 → ĐÓNG 100.8
                                  │
       ┌──────────────┬───────────┼───────────┬──────────────┐
       │              │           │           │              │
 PHÁ VỠ THẬT      SWEEP THẤT BẠI  SỔ MỎNG   ĐỊNH GIÁ LẠI   SHORT COVERING
       │              │           │           │              │
 cần tiếp diễn     cần dấu hấp thụ  cần depth  cần catalyst  cần bối cảnh vị thế
```

**Cách đọc:** hiện tượng trên cùng chỉ là dữ kiện đầu vào. Mỗi nhánh là một cơ chế cạnh tranh và có một loại bằng chứng phân biệt riêng; các nhánh cũng có thể cùng đóng góp.

Phát biểu chính xác: **một đường giá không xác định duy nhất nguyên nhân; ta chỉ tăng xác suất cho một giả thuyết khi quan sát được dấu vết mà giả thuyết đó dự báo tốt hơn các nhánh còn lại.**

> **Ghi nhớ:** đừng chọn câu chuyện trước; hãy chọn bằng chứng có sức phân biệt.

| Giả thuyết (Hypothesis) | Cơ chế (Mechanism) | Bằng chứng ủng hộ | Bằng chứng chống lại | Nếu đúng, tiếp theo nên thấy gì? |
|---|---|---|---|---|
| Breakout thật | Ask liquidity quanh 100 bị hấp thụ; buy stops và demand mới tiếp tục đẩy giá | Giá giữ trên 100; nhịp lùi nông; volume tiếp diễn; sector ủng hộ | Giá quay lại dưới 100 nhanh; không có follow-through | Giá tiếp tục tìm vùng đối ứng cao hơn hoặc retest 100 thành công |
| Liquidity sweep / failed breakout | Buy stops bị kích hoạt nhưng bị seller hấp thụ | Vượt 100 rồi rơi lại; volume lớn nhưng không mở rộng; ask/sell pressure bổ sung | Giá giữ trên 101 và hấp thụ bán tốt | Người mua breakout bị kẹt; giá dễ quay về range |
| Low-liquidity move | Sổ lệnh mỏng, ít ask phía trên nên giá nhảy nhanh | Spread rộng; depth mỏng; volume không đủ lớn so với biến động | Depth dày vẫn bị ăn liên tục | Giá có thể quay về khi liquidity trở lại |
| News repricing | Thông tin mới làm người bán rút quote và người mua nâng giá | Có catalyst rõ; nhiều mã cùng ngành phản ứng | Không có tin; chỉ phản ứng quanh mốc kỹ thuật | Giá giữ vùng mới nếu tin được chấp nhận |
| Short covering | Người bán khống phải mua lại khi giá vượt mốc | Tăng nhanh qua vùng stop; bối cảnh short interest phù hợp | Không có dấu hiệu short/derivatives; lực mua mới tiếp diễn dài | Tăng mạnh nhưng có thể yếu khi covering kết thúc |

Mục tiêu không phải gọi tên đúng ngay. Mục tiêu là biết **dữ liệu nào sẽ phân biệt các giả thuyết**.

---

# 16. FALSIFICATION — Điều gì chứng minh giả thuyết yếu hoặc sai?

Giả thuyết chính của bài:

> Giá chạy nhanh sau khi vượt một vùng vì vùng đó chứa thanh khoản/lệnh kích hoạt; khi lệnh được kích hoạt và liquidity gần nhất không đủ hấp thụ, giá phải đi tìm đối ứng ở mức xa hơn.

Bằng chứng ủng hộ:

- Giá đi qua vùng nhiều người quan sát như đỉnh/đáy cũ, range boundary, số tròn.
- Volume tăng tại hoặc ngay sau vùng đó.
- Time & Sales cho thấy lệnh chủ động cùng chiều tăng.
- Depth phía đối diện bị tiêu thụ hoặc rút đi.
- Spread mở rộng hoặc giá nhảy qua vùng mỏng.
- Sau khi chạm vùng, giá phản ứng mạnh vì lệnh mới xuất hiện.

Bằng chứng làm giả thuyết yếu:

- Không có volume hoặc activity đáng kể quanh vùng bị cho là liquidity pool.
- Giá di chuyển do tin tức rõ ràng và toàn thị trường cùng định giá lại.
- Sổ lệnh cho thấy liquidity không bị tiêu thụ mà chỉ quote được cập nhật.
- Giá vượt vùng nhưng giao dịch rất nhỏ, spread quá rộng, không đủ nói về sweep có ý nghĩa.
- Bối cảnh ngành/thị trường giải thích tốt hơn.
- Sau cú sweep giả định, giá không có phản ứng phù hợp với giả thuyết.

Thiên kiến dễ mắc:

- Thấy giá vượt đỉnh rồi quay đầu là gọi ngay “stop run”.
- Thấy phá đáy rồi hồi là gọi ngay “spring”.
- Gán mọi liquidity pool cho retail stop-loss.
- Quên rằng tổ chức lớn cũng có thể là người bị buộc giao dịch.
- Nhầm volume cao với liquidity cao.
- Nhầm câu chuyện hợp lý với bằng chứng.

---

# 17. APPLICATION — Dùng khái niệm này như thế nào?

Quy trình:

**Quan sát (Observe) → diễn giải (Interpret) → lập giả thuyết (Hypothesize) → dự đoán (Predict) → kiểm tra (Test) → cập nhật xác suất → quyết định**

## Ví dụ ứng dụng

### Quan sát (Observe)

ABC đi ngang giữa 98 và 100 trong cả buổi sáng. Cuối phiên, giá vượt 100.2, lên 101.0 trong 3 phút, volume tăng mạnh, sau đó quay về 99.8.

### Diễn giải (Interpret)

Vùng trên 100 có thể chứa buy stops, breakout orders và limit sell. Khi giá vượt 100.2, lệnh mua mới có thể được kích hoạt nhưng sau đó không đủ tiếp diễn.

### Lập giả thuyết (Hypothesize)

Giả thuyết A: liquidity sweep/failed breakout.

Giả thuyết B: breakout thật nhưng đang retest thất bại tạm thời.

Giả thuyết C: cú nhảy do liquidity mỏng cuối phiên.

Giả thuyết D: tin tức hoặc dòng tiền ngành gây biến động.

### Dự đoán (Predict)

Nếu A đúng, giá khó lấy lại 100.2, người mua breakout có thể bị kẹt, và giá dễ quay về giữa range hoặc biên dưới.

Nếu B đúng, giá nên nhanh chóng lấy lại 100.2, giữ trên vùng breakout, và nhịp lùi có bán yếu.

Nếu C đúng, khi thanh khoản bình thường trở lại, giá có thể quay về vùng cũ và volume không tiếp diễn.

Nếu D đúng, cần thấy catalyst hoặc phản ứng đồng pha ở các mã liên quan.

### Kiểm tra (Test)

Xem:

- Time & Sales quanh 100.2.
- Volume so với trung bình.
- Spread và depth tại thời điểm phá.
- Giá đóng ở đâu so với vùng 100.
- Ngày/phiên sau có giữ hay lấy lại vùng 100.2 không.
- Market/sector context.

### Cập nhật xác suất

Không kết luận nhị phân. Nếu giá tiếp tục bị từ chối ở 100.2, xác suất A tăng. Nếu giá lấy lại 100.2 và hấp thụ bán tốt, xác suất B tăng.

### Quyết định (Decide)

Quyết định có thể là:

- Không giao dịch vì dữ liệu không đủ.
- Chỉ ghi lại case study.
- Chờ retest để quan sát absorption.
- Nếu giao dịch, xác định trước điều kiện vô hiệu hóa giả thuyết.

Khái niệm liquidity hữu ích để hiểu nơi giá có thể phản ứng mạnh. Nó không tự động tạo tín hiệu mua/bán.

---

# 18. FACT → INFERENCE → STORY

```text
FACT / DỮ KIỆN              INFERENCE / SUY LUẬN             STORY / CÂU CHUYỆN
       │                              │                               │
giá, volume, spread, depth    stop có thể đã kích hoạt       “cá mập cố tình quét”
       │                      seller có thể đã hấp thụ        “MM đang điều khiển giá”
       │                              │                               │
QUAN SÁT TRỰC TIẾP              CẦN KIỂM CHỨNG                 GÁN Ý ĐỊNH SÂU
```

**Cách đọc:** đi từ trái sang phải, độ chắc chắn giảm khi khoảng cách với dữ liệu tăng. Một suy luận hợp lý vẫn chưa phải dữ kiện; câu chuyện về danh tính hoặc ý định cần bằng chứng mạnh hơn nữa.

Phát biểu chính xác: **dữ liệu có thể xác nhận giá đã vượt vùng và thanh khoản hiển thị đã đổi, nhưng việc stop tồn tại, ai hấp thụ và họ có chủ ý gì thường chỉ được suy luận với xác suất.**

> **Ghi nhớ:** thấy gì nói nấy; suy thêm phải gắn nhãn; gán ý định phải có bằng chứng.

## Ví dụ 1

> Giá phá lên trên 100, volume tăng mạnh, rồi quay lại 99.7.

- **Dữ kiện (Fact)**: Giá vượt 100, volume tăng, sau đó đóng/khớp lại dưới 100.
- **Suy luận (Inference)**: Cú vượt 100 có thể đã kích hoạt lệnh mua nhưng không có follow-through.
- **Câu chuyện (Story)**: “Cá mập quét stop để xả hàng.” Có thể đúng, nhưng chưa phải dữ kiện.

## Ví dụ 2

> Giá phá đáy 95, giao dịch lớn tại 94.8, rồi phục hồi lên 96.

- **Dữ kiện**: Giá đi dưới đáy cũ, volume lớn ở vùng thấp, sau đó phục hồi.
- **Suy luận**: Sell stops có thể bị kích hoạt và được hấp thụ bởi người mua.
- **Câu chuyện**: “Composite operator tạo spring.” Đây là giả thuyết nâng cao, cần bằng chứng thêm.

## Ví dụ 3

> Volume hôm nay cao gấp 5 lần trung bình nhưng spread cũng rộng hơn và giá giật mạnh.

- **Dữ kiện**: Volume cao, spread rộng, volatility cao.
- **Suy luận**: Có nhiều giao dịch nhưng liquidity có thể đang căng thẳng hoặc không ổn định.
- **Câu chuyện**: “Dòng tiền lớn vào nên thanh khoản tốt.” Câu này có thể sai vì volume cao không đồng nghĩa liquidity cao.

## Ví dụ 4

> Trước tin tức, order book hai bên mỏng đi rõ rệt.

- **Dữ kiện**: Depth giảm, spread có thể rộng hơn.
- **Suy luận**: Liquidity providers có thể rút bớt lệnh vì adverse selection risk tăng.
- **Câu chuyện**: “Market maker cố tình làm giá biến động.” Chưa đủ bằng chứng.

---

# 19. Nghiên cứu tình huống (Case studies)

## Case A — Breakout thật qua vùng thanh khoản

### Dữ kiện quan sát được (Facts)

- ABC có đỉnh cũ 100.
- Giá nhiều lần bị chặn ở 100.
- Lần này giá vượt 100 với volume cao.
- Sau khi vượt, giá giữ trên 100.2.
- Nhịp lùi về 100.1 có volume thấp hơn và bật lên.

### Cơ chế (Mechanism)

**Ask liquidity tại kháng cự bị tiêu thụ → buy stops/breakout orders kích hoạt → người bán không bổ sung đủ liquidity → giá mở rộng lên vùng cao hơn → nhịp lùi được người mua hấp thụ**

### Actors

- Aggressive buyers.
- Breakout traders.
- Short sellers bị stop.
- Passive sellers quanh 100.
- Có thể có tổ chức hoặc algo tham gia.

### Incentives

Buyers muốn vào khi thấy phá đỉnh. Short sellers phải giảm rủi ro. Sellers quanh 100 muốn chốt lời hoặc bán kháng cự nhưng bị hấp thụ.

### Evidence

- Volume tăng tại breakout.
- Giá giữ trên vùng phá.
- Pullback không xuyên lại range.
- Nếu có footprint, có thể thấy buying at ask và absorption ở vùng retest.

### Falsification

Nếu giá nhanh chóng rơi lại dưới 100 và không lấy lại được, giả thuyết breakout thật yếu đi.

### Kết luận

Breakout thật không phải vì “đường kháng cự bị phá” theo nghĩa hình học. Nó xảy ra khi nguồn cung/liquidity phía trên bị hấp thụ và lệnh mua mới đủ mạnh để tìm giá cao hơn.

## Case B — Liquidity sweep trên đỉnh cũ

### Dữ kiện quan sát được (Facts)

- ABC có đỉnh cũ 100.
- Giá vượt lên 100.8 trong thời gian ngắn.
- Volume tăng mạnh tại 100.3-100.8.
- Sau đó giá rơi về 99.5.
- Không có tin tức rõ ràng.

### Cơ chế (Mechanism)

**Giá vượt đỉnh cũ → buy stops/breakout buys kích hoạt → lệnh mua mới cung cấp đối ứng cho người bán → người bán hấp thụ hết lực mua → không còn demand tiếp diễn → giá rơi lại dưới đỉnh**

### Actors

- Breakout buyers.
- Short sellers bị stop.
- Passive/large sellers hấp thụ.
- Short-term traders đảo chiều khi breakout thất bại.

### Incentives

Buyers muốn mua xác nhận phá đỉnh. Người bán lớn có thể cần liquidity để bán size. Traders ngắn hạn bán khi thấy failure.

### Evidence

- Vượt đỉnh nhưng không giữ được.
- Volume lớn ở vùng trên đỉnh.
- Giá đóng lại dưới đỉnh cũ.
- Nếu có dữ liệu chi tiết, buying chủ động tăng nhưng không tạo progress tương xứng.

### Falsification

Nếu sau cú rơi ngắn giá nhanh chóng lấy lại 100.8 và tiếp tục tăng, giả thuyết sweep thất bại yếu đi.

### Kết luận

Có thể gọi đây là liquidity sweep theo hiện tượng. Nhưng kết luận “có người cố tình quét stop” cần bằng chứng mạnh hơn về hành vi chủ động, lặp lại, bối cảnh và dấu vết thực thi.

## Case C — Volume cao nhưng liquidity thấp

### Dữ kiện quan sát được (Facts)

- XYZ giao dịch volume cao hơn trung bình 4 lần.
- Spread mở rộng từ 0.1 lên 0.8.
- Depth best bid/ask giảm mạnh.
- Giá nhảy nhiều đoạn, khớp không liên tục.

### Cơ chế (Mechanism)

**Bất định tăng → liquidity providers rút depth/quote rộng hơn → nhiều lệnh chủ động vẫn giao dịch → volume cao nhưng resting liquidity thấp → giá giật mạnh và slippage lớn**

### Actors

- Traders phản ứng với tin.
- Market makers giảm rủi ro.
- Momentum traders.
- Người bị stop hoặc forced liquidation.

### Incentives

Người cần thoát/vào ưu tiên immediacy. Liquidity providers không muốn đứng đối diện ở giá cũ.

### Evidence

- Volume cao đi cùng spread rộng.
- Order book mỏng.
- Giá nhảy qua nhiều mức.
- Slippage thực tế lớn.

### Falsification

Nếu spread hẹp, depth dày và lệnh lớn vẫn khớp tốt quanh giá hiện tại, giả thuyết liquidity thấp yếu đi.

### Kết luận

Volume cao không đủ để nói thanh khoản tốt. Trong stress, volume và illiquidity có thể cùng tăng.

---

# 20. Câu hỏi Socratic

Hãy trả lời trước khi xem phần đáp án.

1. Vì sao một người muốn mua size lớn cần quan tâm “ai ở phía đối diện” hơn là chỉ nhìn giá hiện tại?
2. Thanh khoản khác khối lượng giao dịch ở điểm nào?
3. Vì sao volume cao vẫn có thể đi cùng slippage lớn?
4. Bid liquidity hấp thụ loại lệnh nào?
5. Ask liquidity hấp thụ loại lệnh nào?
6. Vì sao đỉnh cũ có thể trở thành liquidity pool?
7. Vì sao đáy cũ có thể trở thành liquidity pool?
8. Khi buy stops bị kích hoạt, loại áp lực lệnh nào có thể xuất hiện?
9. Khi sell stops bị kích hoạt, loại áp lực lệnh nào có thể xuất hiện?
10. Một cú giá vượt đỉnh rồi quay đầu có nhất thiết là thao túng không?
11. Liquidity vacuum làm giá chạy nhanh bằng cơ chế nào?
12. Vì sao market maker có thể rút quote trước tin tức?
13. Điều gì phân biệt breakout thật với liquidity sweep thất bại?
14. Vì sao support/resistance nên được hiểu như vùng tương tác của lệnh thay vì đường kẻ?
15. Nếu giả thuyết của bạn là “giá phá đáy do sell stops bị kích hoạt”, bằng chứng nào sẽ làm giả thuyết yếu đi?

## Đáp án và chuỗi suy luận

1. Vì giá hiện tại chỉ là giao dịch gần nhất. Size lớn cần đủ người bán/mua ở phía đối diện; nếu không đủ, lệnh phải khớp qua nhiều mức giá và tự làm giá xấu đi.
2. Volume là lượng đã giao dịch. Liquidity là khả năng hấp thụ lệnh mới tại hoặc gần giá hiện tại.
3. Vì volume cao có thể xảy ra trong lúc spread rộng, depth mỏng và nhiều lệnh chủ động va vào nhau. Khi bạn cần giao dịch ngay, resting liquidity vẫn có thể không đủ.
4. Bid liquidity hấp thụ market sell orders hoặc lệnh bán có khả năng khớp ngay.
5. Ask liquidity hấp thụ market buy orders hoặc lệnh mua có khả năng khớp ngay.
6. Vì quanh đỉnh cũ có thể có limit sell, take-profit, buy stops của short sellers, breakout orders và lệnh thoát hòa vốn.
7. Vì quanh đáy cũ có thể có limit buy, stop-loss của người mua, breakdown sell orders và buy-to-cover của short sellers.
8. Buy stops kích hoạt có thể tạo thêm market buy hoặc lệnh mua chủ động, tiêu thụ ask liquidity.
9. Sell stops kích hoạt có thể tạo thêm market sell hoặc lệnh bán chủ động, tiêu thụ bid liquidity.
10. Không. Nó có thể là breakout thất bại tự nhiên, low-liquidity move, news repricing, short covering kết thúc, hoặc sweep có chủ ý. Cần bằng chứng.
11. Khi vùng trung gian thiếu resting liquidity, lệnh chủ động phải khớp ở mức giá xa hơn để tìm đối ứng, khiến transaction price nhảy nhanh.
12. Vì adverse selection risk tăng. Họ không muốn bị giao dịch ở giá cũ khi thông tin mới có thể làm giá trị hợp lý thay đổi.
13. Breakout thật thường giữ được vùng phá, có follow-through hoặc retest thành công. Sweep thất bại thường vượt vùng rồi quay lại nhanh, cho thấy lệnh kích hoạt bị hấp thụ.
14. Vì vùng đó quan trọng không do đường kẻ, mà do nhiều actor có thể đặt hoặc kích hoạt lệnh quanh đó.
15. Nếu không có volume/activity quanh đáy, thị trường chung/tin tức giải thích tốt hơn, hoặc giá phá đáy nhưng không có dấu hiệu sell pressure/stop cascade, giả thuyết yếu đi.

---

# 21. Kiểm tra “thực sự hiểu”

## Suy luận ngược (Reverse reasoning)

Kết quả: giá tăng từ 100 lên 103 rất nhanh sau khi vượt 100.

Hãy nêu ít nhất 5 cơ chế có thể:

- Ask liquidity quanh 100 bị tiêu thụ.
- Buy stops của short sellers bị kích hoạt.
- Breakout orders tạo thêm market buys.
- Sổ lệnh phía trên mỏng, tạo liquidity vacuum.
- Tin tức khiến sellers rút quote và đặt ask cao hơn.
- Forced buying hoặc hedging tạo áp lực mua bắt buộc.

## Nếu thay đổi một biến thì sao? (What-if)

Nếu ask liquidity tại 100 tăng từ 30,000 lên 3,000,000 cổ phiếu, cùng lượng market buy 100,000 sẽ thế nào?

Suy luận: lệnh có thể được hấp thụ gần 100 hơn, breakout khó xảy ra hơn nếu không có demand lớn hơn hoặc sellers rút liquidity.

## Phản ví dụ (Counterexample)

“Volume cao nghĩa là thanh khoản cao.”

Phản ví dụ: trong một phiên tin xấu, volume rất cao vì nhiều người bán tháo, nhưng bid liquidity rút mạnh, spread rộng, giá rơi qua nhiều mức. Volume cao nhưng khả năng bán thêm ở giá gần hiện tại rất kém.

## Điều kiện bác bỏ (Falsification)

Nhận định:

> Giá vượt đỉnh cũ rồi quay đầu vì buy stops bị quét và seller lớn hấp thụ.

Bằng chứng làm yếu:

- Giá nhanh chóng lấy lại đỉnh và tiếp tục tăng.
- Không có volume đáng kể tại vùng trên đỉnh.
- Dữ liệu cho thấy seller không hấp thụ; chỉ là spread rộng và giao dịch nhỏ.
- Tin tức hoặc sector movement giải thích tốt hơn.
- Không có dấu hiệu buyers bị kẹt sau cú vượt.

## Tự giảng lại (Teach-back)

Hãy giải thích không dùng thuật ngữ chuyên môn:

> Giá chạy nhanh khi người muốn mua hoặc bán ngay không tìm đủ người đứng phía bên kia ở giá gần hiện tại. Họ phải đi tới các mức giá xa hơn để tìm người chấp nhận giao dịch. Những vùng như đỉnh cũ, đáy cũ hoặc số tròn quan trọng vì nhiều người hay đặt kế hoạch giao dịch quanh đó. Khi giá chạm vùng này, nhiều lệnh mới có thể xuất hiện, làm giá đi nhanh hơn hoặc đảo chiều nếu phía đối diện hấp thụ hết.

---

# 22. Bản đồ liên kết (Connection Map)

## Kiến thức cần có trước (Prerequisite)

Từ Phần 1, cần hiểu:

- Lệnh thị trường tiêu thụ thanh khoản.
- Lệnh giới hạn cung cấp thanh khoản.
- Giá giao dịch thay đổi khi lệnh khớp ở mức giá mới.
- Sổ lệnh có depth và có thể bị hủy/bổ sung.
- Market impact phụ thuộc size so với liquidity.

## Phần nằm trước trong chuỗi nhân quả (Upstream)

Liquidity xuất hiện từ:

**Participants → incentives/risk → limit orders/stops/hidden interest → available or potential counterparties**

## Khái niệm hiện tại (Current concept)

Liquidity giải thích:

**Available liquidity → order consumption → imbalance → search for counterparties → price movement**

## Phần được giải thích tiếp theo (Downstream)

Từ liquidity, ta chuyển sang **dòng lệnh (Order Flow)**:

> Ai đang chủ động yêu cầu giao dịch ngay, và phía đối diện có hấp thụ được không?

Liquidity nói về **khả năng hấp thụ**. Order flow nói về **ai đang tấn công thanh khoản và kết quả ra sao**.

## Map nối với toàn khóa

**Participants → Incentives → Orders → Order Book → Liquidity → Order Flow → Imbalance/Absorption → Price Discovery → Price & Volume → Market Structure → Wyckoff/Market Hypotheses**

```text
PHẦN TRƯỚC / UPSTREAM           PHẦN 2 — THANH KHOẢN           PHẦN SAU / DOWNSTREAM

NGƯỜI THAM GIA                   LỆNH ĐANG CHỜ                 DÒNG LỆNH CHỦ ĐỘNG
       ↓                              +                               ↓
ĐỘNG CƠ / RỦI RO                LỆNH ĐƯỢC KÍCH HOẠT           MẤT CÂN BẰNG / HẤP THỤ
       ↓                              ↓                               ↓
LỆNH → SỔ LỆNH              KHẢ NĂNG HẤP THỤ THEO GIÁ        KHÁM PHÁ GIÁ → PRICE ACTION
```

**Cách đọc:** cột trái giải thích thanh khoản được tạo ra từ đâu; cột giữa là nội dung Phần 2; cột phải cho thấy thanh khoản trở thành vật cản mà dòng lệnh ở Phần 3 sẽ tác động vào.

Phát biểu nhân quả: **động cơ và rủi ro tạo ra lệnh đang chờ hoặc lệnh có điều kiện; chúng quyết định khả năng hấp thụ; dòng lệnh chủ động tương tác với khả năng đó để tạo mất cân bằng, khám phá giá và hành động giá quan sát được.**

> **Ghi nhớ:** Phần 2 học sức chứa của thị trường; Phần 3 học dòng lệnh đang sử dụng sức chứa đó.

Phần 2 chủ yếu bao phủ:

**Orders → Resting/Triggered Liquidity → Liquidity Pools → Sweeps/Vacuums → Price Discovery**

---

# 23. Gate 2 — Vì sao giá thường tăng/giảm rất nhanh sau khi vượt một vùng nhất định?

Câu trả lời đạt chuẩn:

> Giá thường chạy nhanh sau khi vượt một vùng nhất định vì vùng đó có thể tập trung nhiều lệnh đang chờ hoặc lệnh được kích hoạt. Ví dụ trên đỉnh cũ có thể có buy stops của người bán khống, breakout orders của người mua, limit sell của người chốt lời và lệnh của người cần liquidity để bán size lớn. Khi giá chạm vùng này, một số lệnh mới xuất hiện và lệnh chủ động tiêu thụ thanh khoản phía đối diện. Nếu liquidity gần nhất không đủ hoặc bị rút đi, giao dịch phải xảy ra ở mức giá xa hơn, nên giá tăng/giảm nhanh. Tuy nhiên, cùng hiện tượng có thể là breakout thật, liquidity sweep thất bại, low-liquidity move, news repricing hoặc forced orders; cần kiểm tra volume, depth, spread, Time & Sales, bối cảnh và khả năng giá giữ vùng sau đó.

Câu trả lời chưa đạt:

- “Vì cá mập quét stop.”
- “Vì breakout nên giá tăng.”
- “Vì thủng hỗ trợ nên bán mạnh.”
- “Vì nhiều người mua hơn người bán.”

Các câu này thiếu cơ chế. Để đạt chuẩn, phải nói rõ:

**Vùng đó chứa loại liquidity nào? Lệnh nào được kích hoạt? Lệnh chủ động tiêu thụ phía nào? Liquidity có đủ hấp thụ không? Giá có giữ được vùng mới không? Có giải thích cạnh tranh nào tốt hơn không?**

---

# 24. Kết thúc bài

## 1. Tóm tắt theo First Principles

1. Thanh khoản là khả năng hấp thụ một lệnh cụ thể, ở một phía và thời điểm cụ thể, mà không làm giá dịch chuyển quá mạnh.
2. Khối lượng giao dịch (volume) là lượng đã khớp; thanh khoản là sức chứa cho lệnh tiếp theo, gồm độ chặt, độ sâu, khả năng khớp ngay và khả năng hồi phục.
3. Thanh khoản đang chờ nằm trong lệnh giới hạn; thanh khoản được kích hoạt xuất hiện khi điều kiện giá làm lệnh dừng hoặc lệnh có điều kiện trở nên hoạt động.
4. Giá chạy nhanh khi lệnh chủ động vượt khả năng hấp thụ gần nhất, đặc biệt trong khoảng trống thanh khoản hoặc khi nhiều lệnh cùng được kích hoạt.
5. Quét thanh khoản mô tả hiện tượng giá đi qua vùng tập trung lệnh; mọi kết luận về ý định phải được kiểm chứng bằng dữ kiện, giả thuyết cạnh tranh và điều kiện bác bỏ.

## 2. Mô hình tư duy (Mental Model)

Hãy nghĩ về giá như quá trình đi tìm người đứng phía bên kia:

**Nếu người muốn giao dịch ngay không tìm đủ đối ứng ở gần giá hiện tại, họ phải đi xa hơn. Những vùng có nhiều người đặt kế hoạch giao dịch sẽ làm quá trình đó tăng tốc hoặc đảo chiều.**

## 3. Không được nhầm

- Không nhầm **volume cao** với **liquidity cao**.
- Không nhầm **vùng liquidity pool** với **bằng chứng thao túng**.
- Không nhầm **giá vượt đỉnh** với **breakout thật**.
- Không nhầm **giá phá đáy rồi hồi** với **spring chắc chắn**.
- Không nhầm **sổ lệnh nhìn thấy** với **toàn bộ liquidity**.
- Không nhầm **last price nhảy mạnh** với **demand/supply bền**.

## 4. Tôi đã hiểu nếu...

Bạn đã hiểu phần 2 nếu có thể:

- Giải thích liquidity bằng cơ chế hấp thụ lệnh.
- Phân biệt liquidity và volume.
- Giải thích vì sao cùng một lệnh có impact khác nhau trong sổ lệnh dày và mỏng.
- Nêu các chiều của liquidity: spread, depth, immediacy, resiliency.
- Giải thích bid liquidity và ask liquidity hấp thụ loại lệnh nào.
- Phân tích vì sao đỉnh cũ, đáy cũ, range boundary và số tròn có thể tập trung lệnh.
- Giải thích buy stop, sell stop và triggered orders làm giá chạy nhanh ra sao.
- Đưa ra ít nhất 3 giả thuyết cho một cú vượt đỉnh rồi quay đầu.
- Nêu bằng chứng làm yếu giả thuyết “liquidity sweep”.
- Trả lời Gate 2 mà không dùng mô hình giá hoặc câu chuyện “cá mập” làm điểm tựa chính.

## 5. Cầu nối sang bài tiếp theo

Sau khi hiểu liquidity là khả năng hấp thụ, câu hỏi kế tiếp là:

> **Ai đang chủ động tiêu thụ thanh khoản, và phía đối diện có hấp thụ được không?**

Đó là Phần 3 — **Dòng lệnh (Order Flow)**.

Nếu Phần 2 giải thích **thanh khoản nằm ở đâu và vì sao giá chạy nhanh khi liquidity bị tiêu thụ**, thì Phần 3 sẽ giải thích **dòng lệnh chủ động và sự hấp thụ để lại dấu vết gì trong price, volume, bid/ask, delta và footprint**.
