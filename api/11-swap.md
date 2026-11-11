# Swap Module

Last Updated: 2026-11-10

All HTTP interfaces in this file are synchronized from the CCPayment API v2 definitions. Every route uses `POST` and the base URL is `https://ccpayment.com/ccpayment/v2/`.

Swap V1 routes below remain callable but are deprecated. `GetSwapPairList` lists enabled pairs; its response is different from `GetSwapCoinList`. Migrate V1 request bodies explicitly: V2 uses `fromCoinId`, `fromAmount`, `toCoinId`, optional `markupFeeRate`, and optional `slippageRate`.

## Endpoint List

- `/getSwapCoinList` -> `GetSwapCoinList` (Deprecated; use GetSwapPairList. ====================================Swap========= Swap token list)
- `/estimate` -> `SwapEstimate` (Deprecated; use SwapEstimateV2. No proto comment.)
- `/getSwapRecord` -> `GetSwapRecord` (Deprecated; use GetSwapRecordV2. No proto comment.)
- `/getSwapRecordList` -> `GetSwapRecordList` (Deprecated; use GetSwapRecordListV2. No proto comment.)
- `/swap` -> `Swap` (Deprecated; use SwapV2. No proto comment.)
- `/swapv2/getSwapPairList` -> `GetSwapPairList` (====================================Swap V2========= GetSwapPairList returns all enabled swap pairs with no business parameters.)
- `/swapv2/estimate` -> `SwapEstimateV2` (V2 Swap报价；非钱包模式不传 userId，钱包模式传用户 ID。)
- `/swapv2/swap` -> `SwapV2` (非钱包模式创建 V2 Swap订单。)
- `/swapv2/getSwapRecord` -> `GetSwapRecordV2` (非钱包模式获取 V2 Swap记录。)
- `/swapv2/getSwapRecordList` -> `GetSwapRecordListV2` (非钱包模式获取 V2 Swap记录列表。)

## Interface Details

### GetSwapCoinList

- HTTP: `POST /getSwapCoinList`
- Request Type: `Empty`
- Response Type: `GetSwapCoinListReply`
- Description: ====================================Swap========= Swap token list
- **Deprecated:** Use `GetSwapPairList` instead. The V1 route remains available.

#### Request Parameters

No body.

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `coins` | `SwapCoinEntity[]` | No |  |

### SwapEstimate

- HTTP: `POST /estimate`
- Request Type: `SwapReq`
- Response Type: `SwapEstimateReply`
- **Deprecated:** Use `SwapEstimateV2` instead. The V1 route remains available.

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
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | 买入币种数量 |
| `netFromAmount` | `string` | No | 实际上参与兑换的fromCoin |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，相当于之前的extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外手续费，等同于extraFee |
| `swapType` | `string` | No | Swap类型，如 flash、Standard |

### GetSwapRecord

- HTTP: `POST /getSwapRecord`
- Request Type: `GetSwapRecordReq`
- Response Type: `GetSwapRecordReply`
- **Deprecated:** Use `GetSwapRecordV2` instead. The V1 route remains available.

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

### GetSwapRecordList

- HTTP: `POST /getSwapRecordList`
- Request Type: `GetSwapRecordListReq`
- Response Type: `GetSwapRecordListReply`
- **Deprecated:** Use `GetSwapRecordListV2` instead. The V1 route remains available.

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

### Swap

- HTTP: `POST /swap`
- Request Type: `SwapReq`
- Response Type: `SwapReply`
- **Deprecated:** Use `SwapV2` instead. The V1 route remains available.

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

### GetSwapPairList

- HTTP: `POST /swapv2/getSwapPairList`
- Request Type: `Empty`
- Response Type: `GetSwapPairListReply`
- Description: ====================================Swap V2========= GetSwapPairList returns all enabled swap pairs with no business parameters.

#### Request Parameters

No body.

#### Response Data

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `pairs` | `SwapPairEntity[]` | No |  |

### SwapEstimateV2

- HTTP: `POST /swapv2/estimate`
- Request Type: `SwapReqV2`
- Response Type: `SwapEstimateReply`
- Description: V2 Swap报价；非钱包模式不传 userId，钱包模式传用户 ID。

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
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | 买入币种数量 |
| `netFromAmount` | `string` | No | 实际上参与兑换的fromCoin |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，相当于之前的extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外手续费，等同于extraFee |
| `swapType` | `string` | No | Swap类型，如 flash、Standard |

### SwapV2

- HTTP: `POST /swapv2/swap`
- Request Type: `SwapReqV2`
- Response Type: `SwapReply`
- Description: 非钱包模式创建 V2 Swap订单。

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

### GetSwapRecordV2

- HTTP: `POST /swapv2/getSwapRecord`
- Request Type: `GetSwapRecordReq`
- Response Type: `GetSwapRecordReply`
- Description: 非钱包模式获取 V2 Swap记录。

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

### GetSwapRecordListV2

- HTTP: `POST /swapv2/getSwapRecordList`
- Request Type: `GetSwapRecordListReq`
- Response Type: `GetSwapRecordListReply`
- Description: 非钱包模式获取 V2 Swap记录列表。

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

### GetSwapCoinListReply

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `coins` | `SwapCoinEntity[]` | No |  |

### SwapCoinEntity

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `coinId` | `uint64` | No |  |
| `symbol` | `string` | No |  |
| `logoUrl` | `string` | No |  |

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

### SwapEstimateReply

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `coinIdIn` | `uint64` | No |  |
| `coinIdOut` | `uint64` | No | Token ID |
| `amountOut` | `string` | No |  |
| `amountIn` | `string` | No |  |
| `swapRate` | `string` | No |  |
| `feeRate` | `string` | No |  |
| `fee` | `string` | No |  |
| `extraFee` | `string` | No |  |
| `netAmountOut` | `string` | No | Net amount received |
| `fromCoinId` | `uint64` | No | swapv2 入账币种(买入币种) |
| `toCoinId` | `uint64` | No | swapv2 得到币种 |
| `fromAmount` | `string` | No | 买入币种数量 |
| `netFromAmount` | `string` | No | 实际上参与兑换的fromCoin |
| `toAmount` | `string` | No | 买入币种数量 |
| `netToAmount` | `string` | No | 实际上买入币种的数量 |
| `feeCoinId` | `uint64` | No | swapv2 手续费币种 |
| `feeDeductSide` | `string` | No | swapv2 扣除手续费的方向，如：from，to |
| `markupFeeRate` | `string` | No | swapv2商家额外费率，相当于之前的extraFeeRate |
| `markupFee` | `string` | No | swapv2商家额外手续费，等同于extraFee |
| `swapType` | `string` | No | Swap类型，如 flash、Standard |

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

### GetSwapPairListReply

GetSwapPairListReply contains all enabled swap pairs without pagination.

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `pairs` | `SwapPairEntity[]` | No |  |

### SwapPairEntity

SwapPairEntity exposes the configured base/quote pair and its fee coin.

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `pairSymbol` | `string` | No | Base/quote symbols, for example BTC/USDT. |
| `coinAId` | `uint64` | No | Base coin ID. ; validation: `(openapi.v3.property) = {type: "integer"}` |
| `coinASymbol` | `string` | No |  |
| `coinBId` | `uint64` | No | Quote coin ID. ; validation: `(openapi.v3.property) = {type: "integer"}` |
| `coinBSymbol` | `string` | No |  |
| `feeCoinId` | `uint64` | No | Configured fee coin ID; 0 retains legacy output-coin fees. ; validation: `(openapi.v3.property) = {type: "integer"}` |

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
