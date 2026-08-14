# Giáo trình: Hiểu cơ chế vận động giá & dòng tiền lớn từ First Principles

**Mã tham chiếu:** MKT-CURR03

## 0. Mục tiêu của giáo trình

Giáo trình này không bắt đầu từ việc học thuộc mô hình nến, chart
pattern hay tín hiệu mua/bán.

Mục tiêu là xây dựng khả năng suy luận:

**Quan sát → Cơ chế → Actors → Incentives → Hypothesis → Evidence →
Alternatives → Falsification → Probability → Decision**

Sau khi hoàn thành, người học cần có khả năng:

-   Giải thích vì sao giá thay đổi từ cơ chế khớp lệnh và cung--cầu thực
    tế.
-   Hiểu vai trò của thanh khoản và vì sao dòng tiền lớn cần thanh
    khoản.
-   Phân biệt điều quan sát được với suy luận và câu chuyện kể về thị
    trường.
-   Hiểu cách tổ chức lớn thực thi lệnh và market impact phát sinh.
-   Giải thích Support, Resistance, Breakout, Pullback từ cơ chế thị
    trường.
-   Đọc Price + Volume + Structure như dấu vết của hành vi thị trường.
-   Hiểu Wyckoff từ cơ chế thay vì học thuộc schematic.
-   Không mặc định mọi biến động bất thường là do "cá mập thao túng".
-   Xây dựng và kiểm chứng nhiều hypothesis cạnh tranh trước khi ra
    quyết định.

------------------------------------------------------------------------

# I. Phương pháp học cố định cho mỗi concept

Mỗi **Core Concept** phải được học qua 9 lớp.

## 1. WHY --- Tại sao concept này phải tồn tại?

-   Vấn đề thực tế nào khiến concept xuất hiện?
-   Nếu concept này không tồn tại, thị trường sẽ vận hành ra sao?
-   Ta có thể tự suy ra concept từ một tình huống đơn giản hay không?

## 2. WHAT --- Bản chất là gì?

Học theo ba tầng:

1.  Giải thích cực kỳ đơn giản.
2.  Định nghĩa tài chính chuẩn.
3.  Định nghĩa theo First Principles.

Luôn trả lời thêm:

> Concept này **không phải** là gì?

## 3. MECHANISM --- Cơ chế nhân quả

Không chấp nhận kết luận kiểu:

> Dòng tiền lớn vào → giá tăng.

Phải phân rã:

**A → B → C → D → Price**

và giải thích từng mắt xích.

## 4. ACTORS --- Ai tham gia?

Xác định những người có thể đứng ở hai phía giao dịch:

-   Retail
-   Institutional investor
-   Mutual/Pension fund
-   Hedge fund
-   Proprietary trader
-   Market maker
-   Algorithm
-   Arbitrageur
-   Major shareholder

Luôn hỏi:

> **Who's on the other side?**

## 5. INCENTIVES --- Họ muốn gì?

Phân tích:

-   Objective
-   Constraint
-   Cost
-   Risk
-   Information
-   Time horizon

Không suy đoán ý đồ chỉ từ hình dạng chart.

## 6. EVIDENCE --- Ta quan sát được gì?

Phân biệt dữ liệu có thể quan sát:

-   Price
-   Volume
-   Bid/Ask
-   Order Book
-   Time & Sales
-   Delta
-   Footprint
-   Volume Profile
-   Volatility
-   Market/sector context

Từ mechanism phải suy ra được prediction có thể kiểm tra.

## 7. ALTERNATIVE EXPLANATIONS --- Còn cách giải thích nào khác?

Với mỗi hiện tượng phải có ít nhất 2--3 hypothesis cạnh tranh khi hợp
lý.

Ví dụ một cú phá đáy có thể liên quan tới:

-   Liquidity event
-   News
-   Large market order
-   Forced liquidation
-   Index rebalance
-   Low liquidity
-   Broad-market movement
-   Random volatility

## 8. FALSIFICATION --- Điều gì chứng minh hypothesis yếu hoặc sai?

Không chỉ tìm bằng chứng ủng hộ.

Luôn hỏi:

> Nếu nhận định của tôi sai, thị trường có thể biểu hiện như thế nào?

## 9. APPLICATION --- Dùng concept như thế nào?

Quy trình:

**Observe → Interpret → Hypothesize → Test → Update probability →
Decide**

Ứng dụng không nhất thiết phải kết thúc bằng Buy/Sell.

------------------------------------------------------------------------

# II. Ba loại kiến thức

## Core Concept

Phải hiểu sâu cả 9 lớp và có khả năng tự suy luận.

## Supporting Concept

Hiểu mechanism, mối quan hệ với Core Concept và cách sử dụng.

## Tool / Measurement

Hiểu:

-   Nó đo cái gì?
-   Dữ liệu đầu vào là gì?
-   Nó không đo được gì?
-   Khi nào dễ gây hiểu nhầm?

Không biến indicator thành "chân lý".

------------------------------------------------------------------------

# III. Nguyên tắc FACT → INFERENCE → STORY

Trong mọi case study phải tách ba tầng.

### FACT

Những gì dữ liệu thực sự cho thấy.

Ví dụ:

> Giá vượt 100 và volume bằng 2 lần trung bình.

### INFERENCE

Suy luận hợp lý từ dữ liệu.

> Buying pressure có thể đang mạnh hơn.

### STORY

Câu chuyện về ý đồ người tham gia.

> "Cá mập kéo qua 100 để kích hoạt FOMO."

Story có thể đúng nhưng không được trình bày như Fact nếu không có đủ
bằng chứng.

------------------------------------------------------------------------

# PHẦN 1 --- MARKET MICROSTRUCTURE

## Câu hỏi trung tâm

> **Một lệnh mua/bán thực sự biến thành chuyển động giá bằng cách nào?**

## 1.1 Các thành phần của thị trường

**Supporting**

-   Exchange
-   Broker
-   Clearing
-   Custody
-   Market maker
-   Buyer/Seller
-   Matching engine

**Cần trả lời được:** Ai làm gì và luồng một lệnh đi qua hệ thống như
thế nào?

## 1.2 Orders

**Core**

-   Market Order
-   Limit Order
-   Stop Order
-   Stop-limit
-   Resting order
-   Aggressive vs passive order

**Investigation:** Một người muốn mua ngay khác gì một người chỉ mua nếu
giá đủ thấp?

## 1.3 Bid -- Ask -- Spread

**Core**

-   Best Bid
-   Best Ask
-   Spread
-   Mid-price

**Câu hỏi trọng tâm:** Tại sao luôn tồn tại chênh lệch giữa giá muốn mua
và muốn bán?

## 1.4 Limit Order Book

**Core**

-   Price levels
-   Queue
-   Depth
-   Price-time priority
-   Order cancellation

**Thought experiment:** Nếu Ask tại 100 chỉ có 1.000 cổ phiếu nhưng tôi
Market Buy 10.000 thì chuyện gì xảy ra?

## 1.5 Matching & Price Formation

**Core**

-   Matching
-   Transaction price
-   Last price
-   Price discovery

**Cần tự suy ra:** Giá không "tự chạy"; orders tương tác khiến mức giá
giao dịch thay đổi.

## 1.6 Market Impact

**Core**

-   Temporary impact
-   Permanent impact
-   Size vs liquidity
-   Slippage

**Case:** Hai người cùng mua 1 tỷ đồng nhưng một người giao dịch VNM,
người kia giao dịch cổ phiếu cực kém thanh khoản. Tại sao tác động khác
nhau?

## 1.7 Volatility

**Supporting**

-   Volatility hình thành từ đâu?
-   Liquidity và volatility
-   Information và volatility
-   Order imbalance và volatility

### Gate 1

Phải giải thích được:

> "Tại sao giá tăng?" mà không sử dụng chart pattern hoặc indicator.

------------------------------------------------------------------------

# PHẦN 2 --- LIQUIDITY

## Câu hỏi trung tâm

> **Nếu tôi muốn giao dịch một lượng rất lớn, ai sẽ đứng phía bên kia?**

## 2.1 Liquidity

**Core**

-   Khả năng hấp thụ giao dịch
-   Price liquidity
-   Depth
-   Immediacy
-   Resiliency

## 2.2 Liquidity vs Volume

**Core**

-   Vì sao volume cao không đồng nghĩa liquidity cao?
-   Vì sao một thị trường có nhiều giao dịch vẫn có thể trượt giá mạnh?

## 2.3 Resting Liquidity

**Core**

-   Limit orders
-   Bid liquidity
-   Ask liquidity
-   Order concentration

## 2.4 Liquidity Pools

**Core**

Nghiên cứu tại sao orders có thể tập trung quanh:

-   Previous High
-   Previous Low
-   Range boundary
-   Support/Resistance
-   Round numbers

Không mặc định mọi vùng này đều chứa cùng một loại order.

## 2.5 Stops và Triggered Orders

**Core**

-   Stop-loss
-   Buy stop
-   Sell stop
-   Breakout order
-   Forced order

**Mechanism:** Khi mức giá bị chạm, những orders mới nào có thể xuất
hiện?

## 2.6 Liquidity Sweep / Stop Run

**Core**

Phải nghiên cứu cả hai hướng:

**Hypothesis A:** Có actor chủ động tìm liquidity.

**Hypothesis B:** Giá đơn giản di chuyển tới vùng có nhiều orders do cơ
chế thị trường.

Không gán intentional manipulation nếu evidence không đủ.

## 2.7 Liquidity Vacuum

**Supporting**

-   Thin book
-   Gap between price levels
-   Rapid price movement

## 2.8 Liquidity và Price Discovery

**Core**

Xây causal chain:

**Available liquidity → Order consumption → Imbalance → Search for
counterparties → New price**

### Gate 2

Giải thích được:

> Vì sao giá thường tăng/giảm rất nhanh sau khi vượt một vùng nhất định?

------------------------------------------------------------------------

# PHẦN 3 --- ORDER FLOW

## Câu hỏi trung tâm

> **Ai đang chủ động yêu cầu giao dịch ngay, và phía đối diện có hấp thụ
> được không?**

## 3.1 Passive vs Aggressive Participants

**Core**

-   Passive buyer
-   Passive seller
-   Aggressive buyer
-   Aggressive seller

## 3.2 Bid/Ask Transactions

**Core**

-   Trade at Bid
-   Trade at Ask
-   Buyer initiated
-   Seller initiated

## 3.3 Order Imbalance

**Core**

-   Buy imbalance
-   Sell imbalance
-   Persistence
-   Local vs broader imbalance

## 3.4 Absorption

**Core**

Case trung tâm:

> Selling volume rất lớn nhưng giá không giảm đáng kể.

Phải xây nhiều hypothesis và evidence cần thiết trước khi kết luận
absorption.

## 3.5 Exhaustion

**Core**

-   Aggression suy yếu
-   Failure to continue
-   Absorption vs exhaustion

## 3.6 Initiative vs Responsive Activity

**Supporting**

Ai đang:

-   Chủ động tìm mức giá mới?
-   Phản ứng khi giá trở nên hấp dẫn?

## 3.7 Volume

**Core**

Phải nghiên cứu ít nhất bốn trường hợp:

-   Price ↑ + Volume ↑
-   Price ↑ + Volume ↓
-   Price ↓ + Volume ↑
-   Price ↓ + Volume ↓

Không học chúng như bốn quy tắc cứng.

## 3.8 Delta / Cumulative Delta

**Tool**

-   Đo cái gì?
-   Không đo cái gì?
-   Divergence có thể xuất hiện vì sao?

## 3.9 Footprint Chart

**Tool**

-   Bid/Ask volume
-   Imbalance
-   Absorption clues

## 3.10 Volume Profile

**Tool**

-   POC
-   HVN
-   LVN
-   Value area

### Gate 3

Phân tích được:

> Volume tăng đột biến nhưng giá gần như đứng yên.

và đưa ra nhiều explanation thay vì một kết luận duy nhất.

------------------------------------------------------------------------

# PHẦN 4 --- INSTITUTIONAL EXECUTION

## Câu hỏi trung tâm

> **Nếu tôi quản lý một quỹ rất lớn, làm thế nào mua/bán mà không tự làm
> xấu giá của mình?**

## 4.1 Institutional Constraints

**Core**

-   Position size
-   Liquidity constraint
-   Time
-   Benchmark
-   Risk
-   Information leakage

## 4.2 Execution Cost

**Core**

-   Explicit cost
-   Spread
-   Slippage
-   Market impact
-   Opportunity cost

## 4.3 Implementation Shortfall

**Core**

Hiểu khoảng cách giữa quyết định đầu tư và kết quả execution thực tế.

## 4.4 Order Slicing

**Core**

Tự suy ra vì sao large order thường phải chia nhỏ.

## 4.5 VWAP

**Supporting / Tool**

-   Benchmark
-   Execution objective
-   Limitations

## 4.6 TWAP

**Supporting / Tool**

So sánh với VWAP từ objective và constraint.

## 4.7 POV

**Supporting**

Participation rate và mối quan hệ với volume thị trường.

## 4.8 Iceberg / Hidden Liquidity

**Supporting**

Hiểu tại sao actor có thể không muốn công khai toàn bộ size.

## 4.9 Algorithmic Execution

**Core**

Không học tên thuật toán trước; bắt đầu từ bài toán tối ưu:

**Market Impact ↔ Execution Risk ↔ Information Leakage ↔ Time**

## 4.10 Accumulation / Distribution dưới góc execution

**Core**

Không mặc định sideways = accumulation.

Phải hỏi:

-   Ai có thể đang mua?
-   Ai đang bán?
-   Evidence nào hỗ trợ?
-   Explanation cạnh tranh là gì?

## 4.11 Institutional Behavior vs Manipulation

**Core**

Phân biệt:

-   Large legitimate execution
-   Market making
-   Hedging
-   Arbitrage
-   Rebalancing
-   Manipulative behavior

### Gate 4

Giải bài toán:

> Một quỹ cần mua lượng cổ phiếu tương đương nhiều ngày thanh khoản bình
> thường. Họ phải đối mặt với những trade-off nào và hành vi nào có thể
> xuất hiện trên thị trường?

------------------------------------------------------------------------

# PHẦN 5 --- MARKET STRUCTURE & PRICE ACTION

## Câu hỏi trung tâm

> **Những cơ chế đã học để lại dấu vết gì trên chart?**

Đây là lúc mới chính thức quay lại chart.

## 5.1 Market Structure

**Core**

-   HH
-   HL
-   LH
-   LL
-   Trend
-   Range

## 5.2 Support

**Core**

Không học "đường hỗ trợ".

Hỏi:

> Tại sao tại một vùng giá, phản ứng mua có thể lặp lại?

Phân tích từ:

**Orders + Positioning + Memory + Incentives + Liquidity**

## 5.3 Resistance

**Core**

Tương tự Support nhưng không giả định trước actor.

## 5.4 Breakout

**Core**

Phân tích:

**Resistance → Available supply → Absorption/imbalance → Triggered
orders → Price expansion**

và các alternative explanations.

## 5.5 Breakdown

**Core**

Mirror mechanism của breakout nhưng phải xét asymmetry do panic,
leverage và forced selling.

## 5.6 Pullback / Retracement

**Core**

-   Profit taking
-   Reduced aggression
-   New counter-orders
-   Retest
-   Continuation vs reversal

## 5.7 Failed Breakout

**Core**

Một bài học trọng tâm về falsification.

## 5.8 Consolidation / Range

**Core**

Không được mặc định:

> Range = cá mập gom hàng.

Phải kiểm tra competing hypotheses.

## 5.9 Reversal vs Continuation

**Core**

Evidence nào cho thấy market mechanism thực sự thay đổi?

## 5.10 Chart Patterns

**Supporting**

Chỉ sau khi hiểu toàn bộ phần trên mới nghiên cứu:

-   Cup & Handle
-   Double Bottom
-   Head & Shoulders
-   Triangle
-   Flag

Với mỗi pattern phải **reverse engineer** về:

**Orders → Liquidity → Order Flow → Positioning → Price Structure**

### Gate 5

Nhìn Cup & Handle và giải thích được toàn bộ mô hình mà không cần dùng
tên "Cup & Handle".

------------------------------------------------------------------------

# PHẦN 6 --- WYCKOFF

## Câu hỏi trung tâm

> **Có thể suy luận quá trình chuyển giao cổ phiếu giữa các nhóm người
> tham gia từ Price + Volume + Structure đến mức nào?**

## 6.1 Wyckoff Philosophy

**Supporting**

-   Supply & Demand
-   Cause & Effect
-   Effort vs Result

Không xem đây là định luật vật lý.

## 6.2 Composite Operator

**Core**

Hiểu đây là **mental model**, không mặc định tồn tại một "cá mập duy
nhất" điều khiển toàn bộ thị trường.

## 6.3 Accumulation

**Core**

-   Range formation
-   Supply absorption
-   Potential transfer of inventory

## 6.4 Spring

**Core**

Reverse engineer bằng:

**Liquidity + Stops + Order Flow + Recovery**

## 6.5 Test

**Core**

Tại sao thị trường có thể quay lại một vùng và ta muốn quan sát điều gì?

## 6.6 Sign of Strength

**Supporting**

Đánh giá bằng Price + Volume + Structure, không chỉ hình dạng.

## 6.7 Distribution

**Core**

Đảo bài toán institutional execution:

> Một holder lớn muốn thoát vị thế mà không làm giá sụp ngay.

## 6.8 Upthrust / UTAD

**Core**

So sánh:

-   Genuine breakout
-   Failed breakout
-   Liquidity event
-   Distribution hypothesis

## 6.9 Markup / Markdown

**Supporting**

Liên kết với liquidity và price discovery.

## 6.10 Wyckoff vs Alternative Explanations

**Core**

Mỗi schematic phải được kiểm tra chống lại:

-   News
-   Fundamental repricing
-   Sector movement
-   Index flows
-   Short covering
-   Forced liquidation
-   Random range behavior

### Gate 6

Có khả năng dùng Wyckoff như **hypothesis framework**, không biến
schematic thành công cụ "đọc ý đồ cá mập".

------------------------------------------------------------------------

# IV. Lớp tích hợp --- Từ dữ liệu đến quyết định

Sau 6 phần, mọi case thị trường sử dụng cùng một protocol.

## Step 1 --- FACT

Ghi những gì thực sự quan sát được.

## Step 2 --- CONTEXT

-   Market
-   Sector
-   Trend
-   Liquidity
-   News/catalyst
-   Timeframe

## Step 3 --- MECHANISM

Điều gì có thể tạo ra các Fact trên?

## Step 4 --- ACTORS

Những actor nào có thể tạo ra mechanism đó?

## Step 5 --- INCENTIVES

Mỗi actor có objective và constraint gì?

## Step 6 --- HYPOTHESES

Lập 2--4 explanation cạnh tranh.

## Step 7 --- PREDICTIONS

Nếu mỗi hypothesis đúng, tiếp theo ta kỳ vọng quan sát thấy gì?

## Step 8 --- FALSIFICATION

Evidence nào khiến mỗi hypothesis bị loại hoặc giảm xác suất?

## Step 9 --- UPDATE

Cập nhật mức độ tin tưởng khi có dữ liệu mới.

## Step 10 --- DECISION

Chỉ sau cùng mới hỏi:

-   Có edge không?
-   Risk/reward?
-   Entry?
-   Invalidation?
-   Position size?
-   Không giao dịch có phải lựa chọn tốt hơn không?

------------------------------------------------------------------------

# V. Causal Map toàn khóa

``` text
MARKET PARTICIPANTS
        ↓
OBJECTIVES + CONSTRAINTS
        ↓
ORDERS
        ↓
ORDER BOOK
        ↓
ORDER FLOW
        ↕
LIQUIDITY
        ↓
ABSORPTION / EXHAUSTION / IMBALANCE
        ↓
PRICE DISCOVERY
        ↓
PRICE + VOLUME
        ↓
MARKET STRUCTURE
        ↓
SUPPORT / RESISTANCE
        ↓
BREAKOUT / FAILURE / PULLBACK
        ↓
CHART PATTERNS
        ↓
WYCKOFF / MARKET HYPOTHESES
        ↓
PROBABILISTIC DECISION
```

------------------------------------------------------------------------

# VI. Những câu hỏi bắt buộc phải trở thành thói quen

1.  **Tôi thực sự quan sát được gì?**
2.  **Phần nào là Fact, phần nào là Inference, phần nào chỉ là Story?**
3.  **Cơ chế nào có thể tạo ra hiện tượng này?**
4.  **Who's on the other side?**
5.  **Mỗi actor có incentive gì?**
6.  **Liquidity nằm ở đâu và tại sao?**
7.  **Ai đang aggressive, ai đang passive?**
8.  **Nếu hypothesis của tôi đúng, tiếp theo phải thấy gì?**
9.  **Còn explanation nào khác?**
10. **Điều gì sẽ chứng minh tôi sai?**
11. **Tôi biết điều này từ dữ liệu hay chỉ đang kể một câu chuyện hợp
    lý?**
12. **Thông tin mới làm xác suất hypothesis tăng hay giảm?**

------------------------------------------------------------------------

# VII. Chuẩn "đã hiểu" một concept

Không đánh giá bằng khả năng nhớ định nghĩa.

Một concept chỉ được xem là đã hiểu khi người học có thể:

-   Giải thích bằng ngôn ngữ đời thường.
-   Giải thích bằng cơ chế First Principles.
-   Vẽ causal chain.
-   Chỉ ra actors và incentives.
-   Nêu evidence có thể quan sát.
-   Đưa ra alternative explanations.
-   Nêu điều kiện falsification.
-   Giải thích một case thực tế.
-   Giải thích một counterexample.
-   Kết nối nó với các concept trước và sau.
-   Giải thích mà không cần dựa vào thuật ngữ chuyên môn.

------------------------------------------------------------------------

# VIII. Thứ tự học

``` text
PHASE 1
Market Microstructure
        ↓
PHASE 2
Liquidity
        ↓
PHASE 3
Order Flow
        ↓
PHASE 4
Institutional Execution
        ↓
PHASE 5
Market Structure & Price Action
        ↓
PHASE 6
Wyckoff
        ↓
INTEGRATION
Real Market Case Studies
```

**Không nhảy thẳng sang Wyckoff hoặc Smart Money Concepts trước khi hiểu
bốn phase đầu.**

Mục tiêu cuối cùng không phải trở thành người "nhìn chart đoán cá mập",
mà là người có thể nhìn thị trường như một hệ thống gồm **participants →
incentives → orders → liquidity → transactions → price**, sau đó xây
dựng nhận định có thể kiểm chứng và cập nhật theo bằng chứng.
