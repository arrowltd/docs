# Giới thiệu
Những gì viết ở coding convention là bắt buộc phải theo. Mỗi mục sẽ có link tới trang giải thích riêng, có thể đọc nếu muốn, nhưng cái trọng tâm là những gì được ghi lại ở đây. Khi review code, khi chuẩn bị commit, push, sẽ cần đảm bảo phần code đó đã theo đúng chuẩn của coding convention. (chú ý là có 1 cái checklist riêng cho review code, commit push code, ở [đây](review_checklist.md)).

Khi cần làm khác đi coding convention, sẽ luôn cần hỏi người quản lý của bạn.
# Error

Không được giấu error. Sẽ luôn phải đọc, xử lý error.

#### Ví dụ 1:

```go
err := bsql.Update(trx, "commission", map[string]any{
    "end_date": date.MaxDate(),
    "product_id": productId,
    "brand_id": brandId,
})

err = trx.Exec(`
	DELETE FROM
		commission
	WHERE
		end_date < start_date
		AND product_id = $1
		AND brand_id = $2
`)
if err != nil {
	panic(err)
}
```
ở ví dụ trên, chúng ta sẽ thấy được là err trả về của ```bsql.Update``` ko được check hay xử lý, mà code đi tới dòng tiếp theo là ```trx.Exec``` luôn. Nếu ```bsql.Update``` lỗi, chúng ta chỉ thấy được lỗi là `transaction already aborted` ở dòng panic dưới, mà ko biết được nó bị ```aborted``` khi nào


#### Ví dụ 2:
```go
row := company.DB.QueryRow(fmt.Sprintf(`
	SELECT
		sum(temp.sum_writer_referral) as sum_all_writer_referral,
		sum(temp.sum_winloss) as sum_all_winloss
	FROM
		(%s) as temp;
	`, queryString), values...)

var grandTotalReferral, grandTotalWinloss decimal.NullDecimal
row.Scan(&grandTotalReferral, &grandTotalWinloss)

results := &pagination.PaginationResp{
	TotalSize: totalCount,
	Total: Dict{
		"grand_total_referral": grandTotalReferral.Decimal,
		"grand_total_winloss":  grandTotalWinloss.Decimal,
	},
	List: list,
}
```
ở dòng ```row.Scan```, function này có trả về error, nhưng ko được xử lý, làm cho query này nếu lỗi, sẽ ko có một thông báo gì mà nó chỉ trả ```grandTotalReferral, grandTotalWinloss``` là 0


#### Ví dụ 3:
```go
balanceAfter, _ := createTransaction(trx, player.Id, bet.Id, bet.BetAmount, TxTypeSabaBet)
// err is supposed to panic inside.
```
toàn bộ phần code này sai (kể cả comment). Vì nếu ở function này error sẽ luôn panic ở trong func, thì nó ko nên return error. Khi mà function có trả error, nó sẽ cần được kiểm tra.

## Database

### 1. Tạo table, datatype
**Không dùng foreign key.**

**Luôn phải có `PRIMARY KEY`**

 - đại đa số thời gian nó sẽ là `id BIGSERIAL PRIMARY KEY`

Tất cả string được lưu vào database sẽ ở cột có `datatype` là `TEXT`, **không bao giờ dùng `VARCHAR`**

Tất cả numeric data (data dạng số, ví dụ 543253425), nếu ko cần tính toán (ko cần cộng trừ nhân chia, thao tác trên database ở dạng cộng trừ nhân chia trung bình, etc.) thì cũng sẽ lưu ở dạng `TEXT`

Tất cả timestamp sẽ dùng `TIMESTAMPTZ` (có timezone)

Tất cả số liệu sẽ dùng `BIGINT` hoặc `NUMERIC(22, 4)`, tuỳ vào việc số này có phải là số nguyên hay ko. **Không bao giờ dùng `INT`**

 - Số điện thoại => `TEXT`
 - Id chỉ bao gồm chữ số, ko có chữ cái => `TEXT`
 - Id gồm chữ số và chứ cái => `TEXT`
 - mã vùng, mã miền, các loại mã pin code được quy định là ko có chữ cái => `TEXT`
 - số tiền của member => `NUMERIC(22, 4)`
 - số lượng vé của member => `BIGINT`
 - số lần member được sai password (chỉ có giá trị từ 0-3) => `BIGINT`

### 2. Query
Luôn `defer rows.Close()` sau khi dùng `db.Query`
* Bọc query code ở trong `func() {}()` nếu query code ở trong loop (defer sẽ chạy sau khi function kết thúc chứ ko phải sau 1 lần loop)

Suy nghĩ về `db.Query` và `for rows.Next()` dưới dạng là cách để lấy data từ database, chứ ko phải để tính toán
* Sẽ ko được có logic code, gọi function ko liên quan tới việc dịch chuyển sửa đổi data, query thêm một lần nữa, ở trong `for rows.Next()` 
* Chỉ nên có code để lấy data ra từ `rows` (`rows.Scan` code) ở trong `for rows.Next()` 
* Tất cả logic, gọi function khác, sẽ nằm ngoài `for rows.Next()` loop (cứ loop qua cái list đã tạo sau khi đọc data từ `for rows.Next()` một lần nữa ở ngoài)

Khi query cần chú ý ko query trên 10000 rows (table, query nào trả nhiều hơn phải có limit). Có thể query nhiều lần khi logic cần phải lấy nhiều hơn 10000 rows, nhưng 1 query chỉ trả về tối đa 10000 rows.

### 3. Thay đổi về database
Tất cả mọi thay đổi về database (datatype, cột, table, default value etc), đều cần tạo migration

### 4. Số lượng query trong 1 api
Nếu API request này cần query nhiều hơn 50 database queries, cần suy nghĩ về logic, và nói chuyện với quản lý của bạn.

Nếu phải loop qua một list data, và gọi query cho từng dòng của list data đó, cần suy nghĩ về logic, và nói chuyện với quản lý của bạn.



When creating new database, have to have `primary key`
* Normally it will be `id BIGSERIAL PRIMARY KEY`
* Please dont use `SERIAL`, or forget to add `PRIMARY KEY`

### 5. Cách viết query trong code

```go
queryString := `
	SELECT
		r.id, r.request_type, r.free_bet_event_id,
		r.date as date,
		m.account_id, m.username,
		by_acc.username,
		m.deposit_count,
		m.first_deposit_date, m.last_deposit_date, m.last_deposit_bank_account_name, m.last_deposit_amount,
		m.last_withdraw_date, m.last_withdraw_bank_account_name, m.last_withdraw_amount
	FROM
		dw_requests r
	LEFT JOIN
		account by_acc ON by_acc.id = r.by_account_id
	LEFT JOIN
		member m ON m.account_id = r.member_account_id
	WHERE
		r.date >= $1
		AND r.date <= $2
	ORDER BY
		r.date DESC
	LIMIT
		3000
`
rows, err := db.Query(queryString, startDate, endDate)
if err != nil {
	panic(err)
}
defer rows.Close()
for rows.Next() {
	var id, freeBetEventId, accountId, depositCount int
	var requestType, username string
	var byAccountUsername, lastDepositBankAccountName, lastWithdrawBankAccountName sql.NullString
	var lastDepositAmount, lastWithdrawAmount decimal.NullDecimal
	var date, firstDepositDate, lastDeposit time.Time
	var date, firstDepositDate, lastDepositDate time.Time

	err := rows.Scan(&id, &requestType,&freeBetEventId,
		&date,
		&accountId, &username,
		&byAccountUsername,
		&depositCount,
		&firstDepositDate,&lastDepositDate,&lastDepositBankAccountName,&lastDepositAmount,
		&lastWithdrawDate,&lastWithdrawBankAccountName,&lastWithdrawAmount)
	if err != nil {
		panic(err)
	}
}
```
* SQL Keywords sẽ được viết hoa
* Xuống hàng ở mỗi SELECT, FROM, WHERE, LEFT JOIN, ORDER BY, LIMIT etc
* ngắt hàng nếu quá nhiều cột
* cột ở cùng 1 dòng sẽ thuộc về 1 cụm logic chung (chứ ko phải mỗi cột nằm 1 dòng)
* vị trí ngắt cột ở mỗi dòng ở SELECT cũng sẽ là vị trí ngắt cột ở mỗi dòng khi Scan()
* dùng $1 $2 khi thêm params (không được dùng String Format, nếu cần phải dùng, hỏi lead)

Một ví dụ về query viết sai:
```golang
queryString := fmt.Sprintf(`
	SELECT
		r.id, r.request_type, r.free_bet_event_id,
		r.date as date,
		m.account_id, m.username,
		by_acc.username,
		m.deposit_count,
		m.first_deposit_date, m.last_deposit_date, m.last_deposit_bank_account_name, m.last_deposit_amount,
		m.last_withdraw_date, m.last_withdraw_bank_account_name, m.last_withdraw_amount
	FROM
		dw_requests r
	LEFT JOIN
		account by_acc ON by_acc.id = r.by_account_id
	LEFT JOIN
		member m ON m.account_id = r.member_account_id
	WHERE
		r.date >= %s
		AND r.date <= %s
	ORDER BY
		r.date DESC
	LIMIT
		3000
`, startDateStr, endDateStr)
rows, err := db.Query(queryString)
if err != nil {
	panic(err)
}
defer rows.Close()
for rows.Next() {
	var id, freeBetEventId, accountId, depositCount int
	var requestType, username string
	var byAccountUsername, lastDepositBankAccountName, lastWithdrawBankAccountName sql.NullString
	var lastDepositAmount, lastWithdrawAmount decimal.NullDecimal
	var date, firstDepositDate, lastDeposit time.Time
	var date, firstDepositDate, lastDepositDate time.Time

	err := rows.Scan(&id, 
		&requestType,
		&freeBetEventId,
		&date,
		&accountId, 
		&username,
		&byAccountUsername,
		&depositCount,
		&firstDepositDate,&lastDepositDate,
		&lastDepositBankAccountName,
		&lastDepositAmount,
		&lastWithdrawDate,
		&lastWithdrawBankAccountName,
		&lastWithdrawAmount)
	if err != nil {
		panic(err)
	}
}
```
* ngắt cột ở SELECT sai
* lâu lâu thích thì 2 cột nằm 1 dòng ở Scan sai
* dùng String Format thay vì $ để truyền params vào query

### 6. Cách viết Scan()
- Không ghi thẳng vào field của object, luôn tạo var rồi dùng nó.
  * Ví dụ đúng
```go
var id, freeBetEventId, accountId, depositCount int
var requestType, username string
var byAccountUsername, lastDepositBankAccountName, lastWithdrawBankAccountName sql.NullString
var lastDepositAmount, lastWithdrawAmount decimal.NullDecimal
var date, firstDepositDate, lastDeposit time.Time
var date, firstDepositDate, lastDepositDate time.Time

err := rows.Scan(&id, &requestType,&freeBetEventId,
	&date,
	&accountId, &username,
	&byAccountUsername,
	&depositCount,
	&firstDepositDate,&lastDepositDate,&lastDepositBankAccountName,&lastDepositAmount,
	&lastWithdrawDate,&lastWithdrawBankAccountName,&lastWithdrawAmount)
if err != nil {
	panic(err)
}
```
  * Ví dụ sai
```go
err := row.Scan(&accountObjc.Username, &accountObjc.DisplayName, &accountObjc.WhitelistIPs, &accountObjc.Permission)
	if err != nil {
		panic(err)
	}
```

# Cách đặt tên

- Nếu là list/slice/array thì đuôi phải có `List` hoặc `s` hoặc `es`. Ví dụ
  * nameList := []string{}
  * members := []*Member{}
  * listName => sai
  * sliceName => sai
- Nếu là map thì đuôi phải có `Map`, đầu phải có tên key và tên value
  * companyAccountIdBalanceMap := map[int]decimal.Decimal{}
  * memberUsernameMemberMap := map[string]*Member{}
  * mapCompanyCache => sai
- tên function sẽ bắt đầu bằng động từ
  * getBetList()
  * getBets()
  * queryMemberInfo()
  * getUsername()
  * Username() => sai
- Nếu function không cần public, thì viết chữ cái đầu là viết thường
  * Nên là lúc mới viết thì cứ viết thường hết, khi nào cần public cái function thì viết hoa lên

# Pointer/Value

**- Đây là cái Tech Lead kị nhất. Đừng bao giờ sai.**
- Nếu đang là object (sinh ra từ struct), thì luôn luôn xài pointer, ko bao giờ pass by value 
- Ví dụ đúng
```go
member := &Member{
	Username : "steve",
	Age: 2,
}
```

```go
memberList := []*Member{}
```

```go
func getMemberWithId(id int) *Member {
	// function content
}
```

- Ví dụ sai
```go
member := Member{
	Username : "steve",
	Age: 2,
}
```

```go
memberList := []Member{}
```

```go
func getMemberWithId(id int) Member {
	// function content
}
```

# Xài lại struct

Không xài lại struct vì ở đó cần field của struct đó, dù là logic không liên quan gì tới nhau.
- Ví dụ sai
```go
type Company struct {
	Username string `json:"username"`
	JoinedDate time.Time `json:"joined_date"`
}

func queryCompanyList(db *bsql.DB) []*Company {
	list := []*Company{}
	rows, err := db.Query(`
			SELECT
				username, joined_date
			FROM
				company
			ORDER BY
				joined_date DESC
		`)
	if err != nil {
		panic(err)
	}
	defer rows.Close()
	for rows.Next() {
		var username string
		var joinedDate time.Time
		err := rows.Scan(&username, &joinedDate)
		if err != nil {
			panic(err)
		}
		list = append(list, &Company{
			Username:   username,
			JoinedDate: joinedDate,
		})
	}
	return list
}

func queryMemberList(db *bsql.DB) []*Company {
	list := []*Company{}
	rows, err := db.Query(`
			SELECT
				username, joined_date
			FROM
				member
			ORDER BY
				joined_date DESC
		`)
	if err != nil {
		panic(err)
	}
	defer rows.Close()
	for rows.Next() {
		var username string
		var joinedDate time.Time
		err := rows.Scan(&username, &joinedDate)
		if err != nil {
			panic(err)
		}
		list = append(list, &Company{
			Username:   username,
			JoinedDate: joinedDate,
		})
	}
	return list
}
```
  * Sai là vì function queryMemberList, mặc dù cũng trả về list object mà mỗi object có Username và JoinedDate, thì logic của `queryMemberList` là để query lấy member list, chứ ko phải là để query lấy company list, nên phải trả về một list gồm member object.

## Độ dài của một dòng code

Độ dài tối đa của một dòng code nên dao động trong khoảng 150 ký tự.

## Git

- Commit message sẽ có thêm tên branch.
  * <tên branch> <commit message>
- Phần không phải tên branch của commit message nên có ý nghĩ
  * Không nên "refactor", "fix bug", "fix bug 2"
  * Có thể commit với message như vậy, nhưng khi đưa Lead review, thì squash mớ đó lại thành 1 commit, và sửa commit message

## Int/Number cho type

- Ví dụ

```go
// Validate existing bet
if reqParams.Type == 20 || reqParams.Type == 21｛
	 // handle
} else if reqParams.Type == 22 {
   // handle	
}
```

- Ở ví dụ trên sẽ rất khó nhớ/biết được là 20 21 là gì, 22 là gì.
- Những trường hợp như vầy chúng ta sẽ tạo const

```go
const ReqParamsTypeBet int = 20
const ReqParamsTypeWin int = 21
const ReqParamsTypeRefund int = 22

// Validate existing bet
if reqParams.Type == ReqParamsTypeBet || reqParams.Type == ReqParamsTypeWin｛
	 // handle
} else if reqParams.Type == ReqParamsTypeRefund {
   // handle	
}
```

## Error

- Return error khi player/member/user cần phải thấy error đó là gì (nhập sai username, chưa nhập đủ ký tự)
- Panic(error) khi player/member/user không cần phải thấy error đó là gì (Database error, gọi api tới bên thứ ba lỗi)
  * khi panic, chỗ handle nó sẽ tự động gửi cho player/member/user lỗi `err:internal_server_error`


## HTTP Response khi gọi API thành công

- Không nên trả về rỗng, JSON rỗng ("" hoặc "{}") khi thành công.
  * Chỉ cần trả về `{"success": true}` là được
	* Đây là vì một số server setup khi lỗi họ cũng trả về rỗng. Làm FE, hay bên nào gọi API request ko biết được đang gọi thành công hay server đang lỗi

# Float64

**- Đây là cái Tech Lead kị thứ 3. Đừng bao giờ sai.**

**Không bao giờ được dùng float, float64, double. Số được chứa bởi những datatype đó không phải là giá trị chính xác.**

- Khi `Unmarshal` JSON data, nếu cần đọc Number JSON Type, chúng ta dùng decimal.Decimal
  * Khi gọi API cần JSON có số thập phân (ví dụ cần gửi lên {"value": 3.14}, thay vì {"value": "3.14"}), sẽ dùng DecimalNumberJSON struct để làm việc đó, thay vì decimalValue.Float64()
```
type DecimalNumberJSON struct {
	DecimalNumber decimal.Decimal
}

func (di *DecimalNumberJSON) UnmarshalJSON(input []byte) error {
	decimalValue, err := decimal.NewFromString(string(input))
	di.DecimalNumber = decimalValue
	if err != nil {
		return err
	}
	return nil
}

func (di DecimalNumberJSON) MarshalJSON() ([]byte, error) {
	raw := di.DecimalNumber.String()
	return []byte(raw), nil
}
```

# JSON marshal/unmarshal

**- Đây là cái Tech Lead kị thứ 2. Đừng bao giờ sai.**

Luôn dùng struct, thay vì `map[string]interface{}` hay `map[string]any`.


 


