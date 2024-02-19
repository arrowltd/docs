# Giới thiệu
Những gì viết ở coding convention là bắt buộc phải theo. Mỗi mục sẽ có link tới trang giải thích riêng, có thể đọc nếu muốn, nhưng cái trọng tâm là những gì được ghi lại ở đây. Khi review code, khi chuẩn bị commit, push, sẽ cần đảm bảo phần code đó đã theo đúng chuẩn của coding convention. (chú ý là có 1 cái checklist riêng cho review code, commit push code, ở [đây](review_checklist.md)).

Khi cần làm khác đi coding convention, sẽ luôn cần hỏi người quản lý của bạn.
# Error

Không được giấu error, ko xử lý error.

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

## Do not reuse struct that are not related just because need a field in it

Example this struct here is for create

![image-20211027142715059](image-20211027142715059.png)

Should not do this (reuse the struct for params in list api, because need `PoolId` field)

![image-20211027142741061](image-20211027142741061.png)

## API

For API that need validation (username cannot be blank, no special character, or logic validation), API will always have to validate, not just FE
	* Because if only FE validate, anyone can just call the API themself and bypass FE validation

## Duplication check

For field, data that need duplication check, will have to check with a regex to prevent special character also

* Normally this will be `username`, `code` bla bla, these always have `a-z0-9` or the like, this will prevent special character

* without special character prevention, data can have extra invisible character that cause 2 same strings to be not equal but display the same

* in this example "douglas" and "douglas" are not equal (https://play.golang.org/p/djhmTffdvvr)

  ![image-20211104155140854](image-20211104155140854.png)

# Timezone

For database, use `timestamptz` data type

In Code, should always use `date` package
* will use `date.Now()` instead of `time.Now()` always
* any code about format datetime to string, string to datetime object should use funcs inside `date` package
* if need more func then add new func to `date` package



## HTTP Request

Remember to add `defer resp.Body.Close()` after send request

![image-20211109104207041](image-20211109104207041.png)

## Git

Commit message will have to be meaningful

Please dont:

![telegram-cloud-photo-size-5-6075848371614101317-y](telegram-cloud-photo-size-5-6075848371614101317-y.png)

another to do this is squash all commit into 1, then amend the commit message to be meaningful

https://www.internalpointers.com/post/squash-commits-into-one-git



## Int/Number for type

Type should be `string`, a self explain text for that type, eg:

* `promo_win`
* `withdraw`

If integrate 3rd party, and they are using integer/number for type, have to create const for those

Example this is very hard to read, no idea what 20 21 or 111 mean

![image-20211111102804707](image-20211111102804707.png)

Updated:

![image-20211111102832004](image-20211111102832004.png)

![image-20211111102933883](image-20211111102933883.png)


## Error

return when user needs to see the error (input error, validation error)

should not return error when user does not need to see the error (it will be returned to user as `err:internal_server_error`), so most of api call to 3rd party, database query error etc

panic only if need Tech Lead to check (in production, every panic will notify Tech Lead). So another way to think if should panic or not is to ask yourself is this error need check by Tech Lead in production server?

Some panic cases:
* API call to 3rd party
* Most errors from db

If user does not need to see, but also no need to ask Tech Lead to check, then log it to file (using LogSerious)


## HTTP Response when success

Should not response empty or JSON `{}` when success. Can be `{"success": true}` or the like
* This is because sometime when server failure or http problem, empty response can also be sent

# Float64

Should just never use float64. It is not an exact value

The way our golang project structure, some value, field, object will get cached and use a lot, if a field is in float64, the more that field is used to compute, the easier it will get incorrect value (3 = 2.9999999)

Another problem with float64 is when unmarshal json data in golang. If the json field is number json type, should use decimal.Decimal, or a custom wrapper for decimal.Decimal to read/write json. This is because there are case when reading a number json type into float64 cause it to just off by a bit (3 = 2.999999)

If a JSON response required a number field with decimal value (example {"value": 3.14}, instead of normally {"value": "3.14"}). We will use DecimalNumberJSON struct to do it, instead of decimalValue.Float64()
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

TODO: pending example needed

# JSON marshal/unmarshal

Will always use struct, or try as much as you can to use struct, instead of Dict or map[string]interface{}. This will prevent various problem with strange json format like number become scientific number, cannot read numeric field from map[string]interface{}

# Define a function.
Any functions defined would be better if they are attached to structs, except the functions defined in help packages (shared/utils).

Format for the function:

type struct_name struct { }

func (m *struct_name) function_name() int {

 //code
    
}

Example:

We have a ticket to identify the payment gateways, they have the same features and only the **payment code** is different.

To do this:

- We define an Utilities struct

![Screen Shot 2023-11-20 at 17 12 48](https://github.com/arrowltd/docs/assets/17697751/66d68f15-cd81-4300-a0fc-7c03bd429867)

- Init 2 Utilities (2 payment gateways) struct with the payment code is different: 
 
  ![Screen Shot 2023-11-20 at 17 28 50](https://github.com/arrowltd/docs/assets/17697751/cbf5822e-319c-4758-9ca5-0dbeed247e0f)

- After that we define function callAPIWithUserAgent and attached to Utilities struct

![Screen Shot 2023-11-20 at 17 15 13](https://github.com/arrowltd/docs/assets/17697751/8efd318e-d46f-4f20-b4df-2bfe007c2e74)

- For each payment gateway we only need to define a unique payment code and based on that code we can perform the corresponding logic





 


