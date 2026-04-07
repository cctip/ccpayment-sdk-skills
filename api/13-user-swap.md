# User Swap Module

## 13.1 User Swap

**Interface:** `POST /userSwap`

**Description:** Execute a swap operation for a user (user-level swap).

**Request Parameters:**

| Field | Type | Required | Description | Validation Rules |
|------|------|------|------|----------|
| orderId | string | Yes | Order ID | Length 3-64 |
| userId | string | No | User ID | Length 3-64 |
| coinIdIn | uint64 | Yes | From token ID | ≥1 |
| amountIn | string | Yes | From amount | Length ≥1 |
| coinIdOut | uint64 | Yes | To token ID | ≥1 |
| amountOutMinimum | string | No | Minimum output amount | - |
| extraFeeRate | string | No | Extra fee rate | - |

**Response Data:**

| Field | Type | Description |
|------|------|------|
| recordId | string | Swap record ID |
| orderId | string | Order ID |
| coinIdIn | uint64 | From token ID |
| coinIdOut | uint64 | To token ID |
| amountOut | string | Output amount |
| amountIn | string | Input amount |
| amountOutMinimum | string | Minimum output amount |
| swapRate | string | Exchange rate |
| fee | string | Fee amount |
| feeRate | string | Fee rate |
| extraFee | string | Extra fee |
| netAmountOut | string | Net amount to receive |
| to_coin_price | string | To coin price |
| from_coin_price | string | From coin price |

## 13.2 Get User Swap Record

**Interface:** `POST /getUserSwapRecord`

**Description:** Query details of a single user swap record.

**Request Parameters:**

| Field | Type | Required | Description |
|------|------|------|------|
| recordId | string | No | Record ID |
| orderId | string | No | Order ID |
| ty | int32 | No | Type |

**Response Data:**

| Field | Type | Description |
|------|------|------|
| record | Object | Swap record |
| record.recordId | string | Record ID |
| record.orderId | string | Order ID |
| record.coinInSymbol | string | From coin symbol |
| record.coinIdIn | uint64 | From coin ID |
| record.coinOutSymbol | string | To coin symbol |
| record.coinIdOut | uint64 | To coin ID |
| record.amountOut | string | Output amount |
| record.amountIn | string | Input amount |
| record.amountOutMinimum | string | Minimum output amount |
| record.netAmountOut | string | Net amount received |
| record.userId | string | User ID |
| record.extraFee | string | Extra fee |
| record.extraFeeRate | string | Extra fee rate |
| record.swapRate | string | Exchange rate |
| record.feeRate | string | Fee rate |
| record.fee | string | Fee amount |
| record.status | string | Status |
| record.createdAt | int64 | Creation timestamp |
| record.arrivedAt | int64 | Arrival timestamp |

## 13.3 Get User Swap Record List

**Interface:** `POST /getUserSwapRecordList`

**Description:** Query user swap record list.

**Request Parameters:**

| Field | Type | Required | Description |
|------|------|------|------|
| recordIds | Array<string> | No | Record ID list (max 20) |
| orderIds | Array<string> | No | Order ID list (max 20) |
| userId | string | No | User ID |
| coinIdIn | uint64 | No | From token ID |
| coinIdOut | uint64 | No | To token ID |
| startAt | int64 | No | Start time (default 90 days) |
| endAt | int64 | No | End time |
| nextId | string | No | Next page ID |

**Response Data:**

| Field | Type | Description |
|------|------|------|
| records | Array | Swap record list (same structure as getUserSwapRecord) |
| nextId | string | Next page ID (optional) |
