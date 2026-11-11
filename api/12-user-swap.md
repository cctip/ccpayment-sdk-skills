# User Swap Module

Last Updated: 2026-11-10

All HTTP interfaces in this file are synchronized from the CCPayment API v2 definitions. Every route uses `POST` and the base URL is `https://ccpayment.com/ccpayment/v2/`.

User Swap V1 routes remain callable but are deprecated. V2 order requests use `SwapReqV2`; migrate coin, amount, fee, and slippage fields explicitly.

## Endpoint List

- `/getUserSwapRecord` -> `GetUserSwapRecord` (Deprecated; use GetUserSwapRecordV2. No proto comment.)
- `/getUserSwapRecordList` -> `GetUserSwapRecordList` (Deprecated; use GetUserSwapRecordListV2. No proto comment.)
- `/userSwap` -> `UserSwap` (Deprecated; use UserSwapV2. No proto comment.)
- `/swapv2/userSwap` -> `UserSwapV2` (钱包模式创建 V2 Swap订单。)
- `/swapv2/getUserSwapRecord` -> `GetUserSwapRecordV2` (钱包模式获取用户 V2 Swap记录。)
- `/swapv2/getUserSwapRecordList` -> `GetUserSwapRecordListV2` (钱包模式获取用户 V2 Swap记录列表。)

## Interface Details

### GetUserSwapRecord

- HTTP: `POST /getUserSwapRecord`
- Request Type: `GetSwapRecordReq`
- Response Type: `GetSwapRecordReply`
- **Deprecated:** Use `GetUserSwapRecordV2` instead. The V1 route remains available.

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `ty` | `int32` | No |  |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `record` | `SwapRecordEntity` | No |  |

### GetUserSwapRecordList

- HTTP: `POST /getUserSwapRecordList`
- Request Type: `GetSwapRecordListReq`
- Response Type: `GetSwapRecordListReply`
- **Deprecated:** Use `GetUserSwapRecordListV2` instead. The V1 route remains available.

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `orderIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `userId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No |  |
| `startAt` | `int64` | No | Defaults to the last 90 days |
| `endAt` | `int64` | No |  |
| `nextId` | `string` | No |  |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `records` | `SwapRecordEntity[]` | No |  |
| `nextId` | `string` | No | Pagination cursor; intended to be hidden from end users later. |

### UserSwap

- HTTP: `POST /userSwap`
- Request Type: `SwapReq`
- Response Type: `SwapReply`
- **Deprecated:** Use `UserSwapV2` instead. The V1 route remains available.

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `orderId` | `string` | No | [(validate.rules).string = {min_len: 3, max_len:64}]; |
| `userId` | `string` | No | [(validate.rules).string = {ignore_empty: true, min_len: 3, max_len:64}]; //{ignore_empty: true, max_len:120}] |
| `coinIdIn` | `uint64` | Yes | Token ID ; validation: `(validate.rules).uint64.gte = 1` |
| `amountIn` | `string` | Yes | Input amount ; validation: `(validate.rules).string.min_len = 1` |
| `coinIdOut` | `uint64` | Yes | Token ID ; validation: `(validate.rules).uint64.gte = 1` |
| `amountOutMinimum` | `string` | No |  |
| `extraFeeRate` | `string` | No |  |
| `appId` | `string` | No | Reserved for business-side use |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `amountOutMinimum` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `to_coin_price` | `string` | No | Price |
| `from_coin_price` | `string` | No | Price |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | Original total payment, including input-side fees |
| `netFromAmount` | `string` | No | Successful payment minus visible input-side fees; includes Hide Fee |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `swapType` | `string` | No | swapv2Swap类型，如flash，Standard |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，等同于extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外费用，等同于extraFee |
| `successFromAmount` | `string` | No | Successful payment including input-side fees; zero before settlement |
| `refundAmount` | `string` | No | Returned input amount for a partially successful swap |
| `userId` | `string` | No |  |

### UserSwapV2

- HTTP: `POST /swapv2/userSwap`
- Request Type: `SwapReqV2`
- Response Type: `SwapReply`
- Description: 钱包模式创建 V2 Swap订单。

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `orderId` | `string` | No | [(validate.rules).string = {min_len: 3, max_len:64}]; |
| `userId` | `string` | No | [(validate.rules).string = {ignore_empty: true, min_len: 3, max_len:64}]; //{ignore_empty: true, max_len:120}] |
| `fromCoinId` | `uint64` | Yes | SwapV2 支付币 ID ; validation: `(validate.rules).uint64.gte = 1` |
| `fromAmount` | `string` | Yes | SwapV2 支付数量 ; validation: `(validate.rules).string.min_len = 1` |
| `toCoinId` | `uint64` | Yes | SwapV2 买入币 ID ; validation: `(validate.rules).uint64.gte = 1` |
| `markupFeeRate` | `string` | No |  |
| `appId` | `string` | No | Reserved for business-side use |
| `slippageRate` | `string` | No | Decimal slippage in [0.001, 0.5], truncated to 3 decimal places; defaults to 0.005. |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `amountOutMinimum` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `to_coin_price` | `string` | No | Price |
| `from_coin_price` | `string` | No | Price |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | Original total payment, including input-side fees |
| `netFromAmount` | `string` | No | Successful payment minus visible input-side fees; includes Hide Fee |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `swapType` | `string` | No | swapv2Swap类型，如flash，Standard |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，等同于extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外费用，等同于extraFee |
| `successFromAmount` | `string` | No | Successful payment including input-side fees; zero before settlement |
| `refundAmount` | `string` | No | Returned input amount for a partially successful swap |
| `userId` | `string` | No |  |

### GetUserSwapRecordV2

- HTTP: `POST /swapv2/getUserSwapRecord`
- Request Type: `GetSwapRecordReq`
- Response Type: `GetSwapRecordReply`
- Description: 钱包模式获取用户 V2 Swap记录。

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `ty` | `int32` | No |  |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `record` | `SwapRecordEntity` | No |  |

### GetUserSwapRecordListV2

- HTTP: `POST /swapv2/getUserSwapRecordList`
- Request Type: `GetSwapRecordListReq`
- Response Type: `GetSwapRecordListReply`
- Description: 钱包模式获取用户 V2 Swap记录列表。

#### Request Parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `orderIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `userId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No |  |
| `startAt` | `int64` | No | Defaults to the last 90 days |
| `endAt` | `int64` | No |  |
| `nextId` | `string` | No |  |

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `records` | `SwapRecordEntity[]` | No |  |
| `nextId` | `string` | No | Pagination cursor; intended to be hidden from end users later. |

## Data Models

### GetSwapRecordReq

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `ty` | `int32` | No |  |

### GetSwapRecordReply

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `record` | `SwapRecordEntity` | No |  |

### SwapRecordEntity

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `coinInSymbol` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinOutSymbol` | `string` | No |  |
| `coinIdOut` | `uint64` | No |  |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `amountOutMinimum` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `userId` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `extraFeeRate` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `status` | `string` | No |  |
| `createdAt` | `int64` | No |  |
| `arrivedAt` | `int64` | No |  |
| `fromCoinSymbol` | `string` | No | swapv2 付款代币symbol |
| `fromCoinId` | `uint64` | No | swapv2 付款代币id |
| `fromAmount` | `string` | No | Original total payment, including input-side fees |
| `netFromAmount` | `string` | No | Successful payment minus visible input-side fees; includes Hide Fee |
| `toCoinSymbol` | `string` | No | swapv2 收到代币symbol |
| `toCoinId` | `uint64` | No | swapv2 收到代币id |
| `toAmount` | `string` | No | 得到代币数量 |
| `netToAmount` | `string` | No | 实际得到代币数量 |
| `swapType` | `string` | No | Swap类型 |
| `markupFeeRate` | `string` | No | 商家额外Service fee率 |
| `markupFee` | `string` | No | 商家额外Service fee |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `successFromAmount` | `string` | No | Successful payment including input-side fees; zero before settlement |
| `refundAmount` | `string` | No | Returned input amount for a partially successful swap |
| `slippageRate` | `string` | No | Slippage fraction saved with the order; for example, "0.03" means 3%. |

### GetSwapRecordListReq

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `orderIds` | `string[]` | No | validation: `(validate.rules).repeated.max_items = 20` |
| `userId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No |  |
| `startAt` | `int64` | No | Defaults to the last 90 days |
| `endAt` | `int64` | No |  |
| `nextId` | `string` | No |  |

### GetSwapRecordListReply

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `records` | `SwapRecordEntity[]` | No |  |
| `nextId` | `string` | No | Pagination cursor; intended to be hidden from end users later. |

### SwapReq

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `orderId` | `string` | No | [(validate.rules).string = {min_len: 3, max_len:64}]; |
| `userId` | `string` | No | [(validate.rules).string = {ignore_empty: true, min_len: 3, max_len:64}]; //{ignore_empty: true, max_len:120}] |
| `coinIdIn` | `uint64` | Yes | Token ID ; validation: `(validate.rules).uint64.gte = 1` |
| `amountIn` | `string` | Yes | Input amount ; validation: `(validate.rules).string.min_len = 1` |
| `coinIdOut` | `uint64` | Yes | Token ID ; validation: `(validate.rules).uint64.gte = 1` |
| `amountOutMinimum` | `string` | No |  |
| `extraFeeRate` | `string` | No |  |
| `appId` | `string` | No | Reserved for business-side use |

### SwapReply

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `recordId` | `string` | No |  |
| `orderId` | `string` | No |  |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `amountOutMinimum` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `to_coin_price` | `string` | No | Price |
| `from_coin_price` | `string` | No | Price |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | Original total payment, including input-side fees |
| `netFromAmount` | `string` | No | Successful payment minus visible input-side fees; includes Hide Fee |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `swapType` | `string` | No | swapv2Swap类型，如flash，Standard |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，等同于extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外费用，等同于extraFee |
| `successFromAmount` | `string` | No | Successful payment including input-side fees; zero before settlement |
| `refundAmount` | `string` | No | Returned input amount for a partially successful swap |
| `userId` | `string` | No |  |

### SwapReqV2

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `orderId` | `string` | No | [(validate.rules).string = {min_len: 3, max_len:64}]; |
| `userId` | `string` | No | [(validate.rules).string = {ignore_empty: true, min_len: 3, max_len:64}]; //{ignore_empty: true, max_len:120}] |
| `fromCoinId` | `uint64` | Yes | SwapV2 支付币 ID ; validation: `(validate.rules).uint64.gte = 1` |
| `fromAmount` | `string` | Yes | SwapV2 支付数量 ; validation: `(validate.rules).string.min_len = 1` |
| `toCoinId` | `uint64` | Yes | SwapV2 买入币 ID ; validation: `(validate.rules).uint64.gte = 1` |
| `markupFeeRate` | `string` | No |  |
| `appId` | `string` | No | Reserved for business-side use |
| `slippageRate` | `string` | No | Decimal slippage in [0.001, 0.5], truncated to 3 decimal places; defaults to 0.005. |
