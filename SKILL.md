---
name: ccpayment.sdk.codegen
version: 2.1.0
description: CCPayment SDK Code Generator - Generate strongly-typed SDK code from API documentation
homepage: https://ccpayment.com
metadata:
  {
    "ccpayment":
      {
        "category": "sdk-generator",
        "api_base": "https://ccpayment.com/ccpayment/v2/",
        "api_docs": "api/",
        "supported_languages": ["golang", "typescript", "python", "java", "php", "shell"]
      }
  }
---

# CCPayment SDK Code Generator

## Usage

### Prerequisites: Import and Learn

Before using the code generator, you need to import this file into your AI Agent:

**Step 1: Import the Skill File**

Import this file in your AI Agent using the following method:

```
@(ccpayment-sdk-skills/SKILL.md)
```

**Step 2: Let AI Agent Learn**

After importing, the AI Agent will automatically read and learn the code generation rules, type mappings, validation rules, etc. from this document.

---

### Generate Code Command

After learning is complete, use the following command to generate SDK code:

```bash
ccpayment.sdk.codegen <language> [module] [path]
```

**Parameter Description:**
- `<language>` (required): Target language, supports `golang`, `typescript`, `python`, `java`, `php`, `ruby`, `javascript`, `shell`
- `[module]` (optional): Specify the module to generate, generate all modules if not specified
- `[path]` (optional): Specify the output path for generated code, use default path if not specified

**Usage Examples:**
```bash
# Generate all modules to default path
ccpayment.sdk.codegen golang

# Generate only merchant-assets module to default path
ccpayment.sdk.codegen golang merchant-assets

# Generate all modules to specified path
ccpayment.sdk.codegen golang /custom/output/path

# Generate only merchant-assets module to specified path
ccpayment.sdk.codegen golang merchant-assets /custom/output/path

# Generate Shell scripts for all modules
ccpayment.sdk.codegen shell

# Generate Shell script for merchant-assets module
ccpayment.sdk.codegen shell merchant-assets

# Generate to specified path
ccpayment.sdk.codegen shell merchant-assets ./shell-examples
```

**Default Output Paths:** (relative to working directory)
- Golang: `generated/golang/`
- Python: `generated/python/`
- TypeScript: `generated/typescript/`
- Java: `generated/java/`
- PHP: `generated/php/`
- Shell: `generated/shell/`

**Supported Modules:**
- `basic-info` - Basic Info Module (Coins, Fiat, Chains)
- `merchant-assets` - Merchant Assets Module
- `merchant-deposit` - Merchant Deposit Module
- `merchant-withdraw` - Merchant Withdraw Module
- `merchant-batch-withdraw` - Merchant Batch Withdraw Module
- `user-assets` - User Assets Module
- `user-deposit` - User Deposit Module
- `user-withdraw` - User Withdraw Module
- `user-transfer` - User Transfer Module
- `orders` - Orders Module
- `swap` - Swap Module
- `user-swap` - User Swap Module
- `utilities` - Utilities

## Working Principles

When the AI assistant receives a command, it executes different generation strategies based on the parameters:

### Full Generation Mode
**Command**: `ccpayment.sdk.codegen golang` or `ccpayment.sdk.codegen golang . /custom/path`

Generate a complete SDK for all 13 modules, including all core components and services.

### Modular Generation Mode
**Command**: `ccpayment.sdk.codegen golang merchant-assets` or `ccpayment.sdk.codegen golang merchant-assets /custom/path`

Only generate code for the specified module, including:
- Core Components (Client, Signature, Models, Errors)
- Service for the specified module
- Data models related to this module
- Test program (only tests this module)

### Path Parameter Handling
- If `[path]` parameter is provided, code will be generated to the specified path
- If `[path]` parameter is not provided, code will be generated to the default path `generated/<language>/`
- Path can be relative or absolute
- If the target path does not exist, it will be created automatically

### Execution Steps

#### Step 1: Read API Documentation

**Base Files (always required):**
- `api/README.md` - Authentication, Base URL
- `api/appendix.md` - Data types, error codes, validation rules

**Module File Mapping Table:**

| Module Name | Documentation File | Generated Service Class |
|:------|:---------|:------------|
| `basic-info` | `api/01-basic-info.md` | `BasicInfoService` |
| `merchant-assets` | `api/02-merchant-assets.md` | `MerchantAssetsService` |
| `merchant-deposit` | `api/03-merchant-deposit.md` | `MerchantDepositService` |
| `merchant-withdraw` | `api/04-merchant-withdraw.md` | `MerchantWithdrawService` |
| `merchant-batch-withdraw` | `api/05-merchant-batch-withdraw.md` | `MerchantBatchWithdrawService` |
| `user-assets` | `api/06-user-assets.md` | `UserAssetsService` |
| `user-deposit` | `api/07-user-deposit.md` | `UserDepositService` |
| `user-withdraw` | `api/08-user-withdraw.md` | `UserWithdrawService` |
| `user-transfer` | `api/09-user-transfer.md` | `UserTransferService` |
| `orders` | `api/10-orders.md` | `OrdersService` |
| `swap` | `api/11-swap.md` | `SwapService` |
| `user-swap` | `api/12-user-swap.md` | `UserSwapService` |
| `utilities` | `api/13-utilities.md` | `UtilitiesService` |

**Reading Strategy:**
- Full generation: Read all 13 module files
- Modular generation: Only read the specified module file + base files

#### Step 2: Parse API Definitions

Extract from each module document:
- **API Path**: e.g., `POST /getCoinList`
- **Request Parameters Table**: field name, type, required, description, validation rules
- **Response Data Table**: field name, type, description

#### Step 3: Generate Strongly-Typed Code

#### Golang Code Structure

**Full Generation** (`ccpayment.sdk.codegen golang` or `ccpayment.sdk.codegen golang /output/path`):
```
<output_path>/  (default: generated/golang/)
├── client.go              # Main client (contains all service methods)
├── signature.go           # Signature generation
├── models.go              # All data models
├── errors.go              # Error handling
├── basic_info.go          # Basic info service
├── merchant_assets.go     # Merchant assets service
├── merchant_deposit.go    # Merchant deposit service
├── ... (other 10 services)
├── test_main.go           # Complete test program
├── go.mod                 # Go module configuration
└── README.md              # Usage documentation
```

**Modular Generation** (`ccpayment.sdk.codegen golang merchant-assets` or `ccpayment.sdk.codegen golang merchant-assets /output/path`):
```
<output_path>/  (default: generated/golang/)
├── client.go              # Main client (only contains MerchantAssets method)
├── signature.go           # Signature generation
├── models.go              # Only contains merchant-assets related data models
├── errors.go              # Error handling
├── merchant_assets.go     # Merchant assets service
├── test_main.go           # merchant-assets test program
├── go.mod                 # Go module configuration
└── README.md              # Usage documentation (for this module)
```

**Custom Path Examples:**
```bash
# Generate to project's sdk directory
ccpayment.sdk.codegen golang merchant-assets ./sdk/golang

# Generate to absolute path
ccpayment.sdk.codegen python /Users/username/projects/my-sdk/python
```

#### Core Component Implementation

**1. Client (client.go)**
```go
package main

import (
    "bytes"
    "context"
    "encoding/json"
    "fmt"
    "io"
    "net/http"
)

const (
    DefaultBaseURL = "https://ccpayment.com/ccpayment/v2"
)

type Client struct {
    appID      string
    appSecret  string
    baseURL    string
    httpClient *http.Client
}

func NewClient(appID, appSecret string) *Client {
    return &Client{
        appID:      appID,
        appSecret:  appSecret,
        baseURL:    DefaultBaseURL,
        httpClient: &http.Client{},
    }
}

func (c *Client) SetHTTPClient(httpClient *http.Client) {
    c.httpClient = httpClient
}

// Returns each service (dynamically added based on generated modules)
// If only generating merchant-assets module, only this method is included
func (c *Client) MerchantAssets() *MerchantAssetsService {
    return &MerchantAssetsService{client: c}
}

// When fully generated, includes all service methods:
// func (c *Client) BasicInfo() *BasicInfoService
// func (c *Client) MerchantDeposit() *MerchantDepositService
// func (c *Client) UserAssets() *UserAssetsService
// ... etc.

// post internal method
func (c *Client) post(ctx context.Context, path string, req, resp interface{}) error {
    var bodyBytes []byte
    var err error

    if req != nil {
        bodyBytes, err = json.Marshal(req)
        if err != nil {
            return fmt.Errorf("marshal request: %w", err)
        }
    }

    sign, timestamp, err := c.generateSign(bodyBytes)
    if err != nil {
        return fmt.Errorf("generate sign: %w", err)
    }

    httpReq, err := http.NewRequestWithContext(ctx, "POST", c.baseURL+path, bytes.NewReader(bodyBytes))
    if err != nil {
        return fmt.Errorf("create request: %w", err)
    }

    httpReq.Header.Set("Content-Type", "application/json")
    httpReq.Header.Set("Appid", c.appID)
    httpReq.Header.Set("Sign", sign)
    httpReq.Header.Set("Timestamp", timestamp)

    httpResp, err := c.httpClient.Do(httpReq)
    if err != nil {
        return fmt.Errorf("do request: %w", err)
    }
    defer httpResp.Body.Close()

    respBody, err := io.ReadAll(httpResp.Body)
    if err != nil {
        return fmt.Errorf("read response: %w", err)
    }

    var apiResp Response
    if err := json.Unmarshal(respBody, &apiResp); err != nil {
        return fmt.Errorf("unmarshal response: %w", err)
    }

    if apiResp.Code != 10000 {
        return &APIError{
            Code:    apiResp.Code,
            Message: apiResp.Msg,
        }
    }

    if resp != nil {
        dataBytes, err := json.Marshal(apiResp.Data)
        if err != nil {
            return fmt.Errorf("marshal data: %w", err)
        }
        if err := json.Unmarshal(dataBytes, resp); err != nil {
            return fmt.Errorf("unmarshal data: %w", err)
        }
    }

    return nil
}
```

**2. Signature (signature.go)**
```go
package main

import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/hex"
    "strconv"
    "time"
)

func (c *Client) generateSign(bodyBytes []byte) (sign, timestamp string, err error) {
    timestamp = strconv.FormatInt(time.Now().Unix(), 10)
    
    signText := c.appID + timestamp
    if len(bodyBytes) > 0 {
        signText += string(bodyBytes)
    }
    
    mac := hmac.New(sha256.New, []byte(c.appSecret))
    mac.Write([]byte(signText))
    sign = hex.EncodeToString(mac.Sum(nil))
    
    return sign, timestamp, nil
}
```

**3. Models (models.go)**

Generate request and response structures for each API:

```go
package main

type Response struct {
    Code int         `json:"code"`
    Msg  string      `json:"msg"`
    Data interface{} `json:"data"`
}

type APIError struct {
    Code    int
    Message string
}

func (e *APIError) Error() string {
    return e.Message
}

// Asset-related models
type Asset struct {
    CoinID     uint64 `json:"coinId"`
    CoinSymbol string `json:"coinSymbol"`
    Available  string `json:"available"`
}

type GetAppCoinAssetListResponse struct {
    Assets []Asset `json:"assets"`
}

type GetAppCoinAssetRequest struct {
    CoinID uint64 `json:"coinId"`
}

type GetAppCoinAssetResponse struct {
    Asset Asset `json:"asset"`
}
```

**4. Services (merchant_assets.go)**

```go
package main

import "context"

type MerchantAssetsService struct {
    client *Client
}

func (s *MerchantAssetsService) GetAppCoinAssetList(ctx context.Context) (*GetAppCoinAssetListResponse, error) {
    var result GetAppCoinAssetListResponse
    err := s.client.post(ctx, "/getAppCoinAssetList", nil, &result)
    if err != nil {
        return nil, err
    }
    return &result, nil
}

func (s *MerchantAssetsService) GetAppCoinAsset(ctx context.Context, coinID uint64) (*GetAppCoinAssetResponse, error) {
    req := &GetAppCoinAssetRequest{CoinID: coinID}
    var result GetAppCoinAssetResponse
    err := s.client.post(ctx, "/getAppCoinAsset", req, &result)
    if err != nil {
        return nil, err
    }
    return &result, nil
}
```

**5. Test Program (test_main.go)**

The generated test program automatically adjusts test content based on the specified module.

**Example: merchant-assets module test**
```go
package main

import (
    "context"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "time"
)

func main() {
    // API credentials
    appID := "your_app_id"
    appSecret := "your_app_secret"

    // Create client
    client := NewClient(appID, appSecret)

    // Optional: Configure HTTP proxy
    // proxyURL, _ := url.Parse("http://127.0.0.1:10808")
    // httpClient := &http.Client{
    //     Transport: &http.Transport{Proxy: http.ProxyURL(proxyURL)},
    //     Timeout: 30 * time.Second,
    // }
    // client.SetHTTPClient(httpClient)

    fmt.Println("=== Merchant Assets Module Test ===")
    
    // Test 1: Get all assets
    fmt.Println("\n[Test 1] Get all assets...")
    resp, err := client.MerchantAssets().GetAppCoinAssetList(context.Background())
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("✓ Successfully retrieved asset list, total %d tokens\n", len(resp.Assets))
    
    // Display first 10
    for i, asset := range resp.Assets {
        if i >= 10 {
            break
        }
        fmt.Printf("%d. %s: %s\n", i+1, asset.CoinSymbol, asset.Available)
    }

    // Test 2: Get single coin asset
    if len(resp.Assets) > 0 {
        fmt.Println("\n[Test 2] Get single coin asset...")
        coinID := resp.Assets[0].CoinID
        assetResp, err := client.MerchantAssets().GetAppCoinAsset(context.Background(), coinID)
        if err != nil {
            log.Printf("Failed to get: %v", err)
        } else {
            fmt.Printf("✓ %s available balance: %s\n", assetResp.Asset.CoinSymbol, assetResp.Asset.Available)
        }
    }
    
    fmt.Println("\n=== Test completed ===")
}
```

**Different modules will generate different test content:**
- `basic-info`: Test getting coin list, fiat list, chain list
- `merchant-deposit`: Test creating deposit address, querying deposit records
- `user-transfer`: Test user transfer, batch transfer
- etc...

#### Shell Script Generation Mode

**Full Generation** (`ccpayment.sdk.codegen shell` or `ccpayment.sdk.codegen shell /output/path`):

Generate Shell script files for all 13 modules:

```
<output_path>/  (default: generated/shell/)
├── basic_info.sh          # Basic info module Shell script
├── merchant_assets.sh     # Merchant assets module Shell script
├── merchant_deposit.sh    # Merchant deposit module Shell script
├── merchant_withdraw.sh   # Merchant withdraw module Shell script
├── ... (other modules)
├── all_apis.sh            # All APIs Shell script collection
├── README.md              # Usage documentation
└── env.sh                 # Environment variables configuration template
```

**Modular Generation** (`ccpayment.sdk.codegen shell merchant-assets`):

Only generate Shell script for the specified module:

```
<output_path>/  (default: generated/shell/)
├── merchant_assets.sh     # Merchant assets module Shell script
├── README.md              # Usage documentation
└── env.sh                 # Environment variables configuration template
```

**Generated Shell Script Example:**

```bash
#!/bin/bash
# Merchant assets module - Shell script example
# Usage: source env.sh && ./merchant_assets.sh

# Get all assets
# POST /getAppCoinAssetList
# Request parameters: None
curl -X POST "${BASE_URL}/getAppCoinAssetList" \
  -H "Content-Type: application/json" \
  -H "Appid: ${APP_ID}" \
  -H "Sign: $(generate_sign '{}')" \
  -H "Timestamp: $(date +%s)" \
  -d '{}'

echo ""
echo "---"

# Get single coin asset
# POST /getAppCoinAsset
# Request parameters: coinId (uint64, required)
curl -X POST "${BASE_URL}/getAppCoinAsset" \
  -H "Content-Type: application/json" \
  -H "Appid: ${APP_ID}" \
  -H "Sign: $(generate_sign '{"coinId":1280}')" \
  -H "Timestamp: $(date +%s)" \
  -d '{"coinId":1280}'
```

**env.sh Environment Variables Template:**

```bash
#!/bin/bash
# Environment variables configuration

export BASE_URL="https://ccpayment.com/ccpayment/v2"
export APP_ID="your_app_id"
export APP_SECRET="your_app_secret"

# Signature generation function
generate_sign() {
    local body="$1"
    local timestamp=$(date +%s)
    local sign_text="${APP_ID}${timestamp}${body}"
    echo -n "$sign_text" | openssl dgst -sha256 -hmac "$APP_SECRET" | sed 's/^.* //'
}

export -f generate_sign
```

#### Step 4: Type Mapping

Map API documentation types to target language types:

| Doc Type | Golang | TypeScript | Python | Java |
|:--------|:-------|:-----------|:-------|:-----|
| `uint64` | `uint64` | `number` | `int` | `Long` |
| `int64` | `int64` | `number` | `int` | `Long` |
| `int32` | `int32` | `number` | `int` | `Integer` |
| `uint32` | `uint32` | `number` | `int` | `Integer` |
| `string` | `string` | `string` | `str` | `String` |
| `bool` | `bool` | `boolean` | `bool` | `Boolean` |
| `Array` | `[]T` | `T[]` | `List[T]` | `List<T>` |
| `Object` | `struct` | `interface` | `dict` | `Map` |

#### Step 5: Validation Rules Mapping

Generate code validation logic from API documentation validation rules:

| Doc Rule | Golang validate tag |
|:---------|:-------------------|
| Required | `validate:"required"` |
| Length ≥1 | `validate:"min=1"` |
| Length 3-64 | `validate:"min=3,max=64"` |
| ≥1 | `validate:"gte=1"` |
| ≤100 | `validate:"lte=100"` |
| Email format | `validate:"email"` |
| URI format | `validate:"uri"` |
| Max 50 items | `validate:"max=50"`

## Important Notes

### Path Parameter Handling

⚠️ **Path Parameter Rules**:

**Path Format:**
- Relative path: `./sdk/golang`, `../output/python`
- Absolute path: `/Users/username/projects/sdk`, `C:/projects/sdk`

**Path Processing:**
- If target path does not exist, directory will be created automatically
- If target path already contains files, existing files will be overwritten
- Recommended to use relative paths for project portability

**Examples:**
```bash
# Relative path (recommended)
ccpayment.sdk.codegen golang merchant-assets ./sdk/golang

# Absolute path
ccpayment.sdk.codegen python . /Users/oks/projects/ccpayment-sdk/python

# Default path (without path parameter)
ccpayment.sdk.codegen golang merchant-assets
```

### Package Name Handling

⚠️ **Key Issue**: When generating test code, all files must use the same package name.

**Correct Approach:**
- If generating standalone test program, all files use `package main`
- If generating SDK library, all files use `package ccpayment`, test files use `package ccpayment_test`

**Incorrect Example** (will cause compilation failure):
```
client.go: package main
models.go: package main
example_test.go: package ccpayment  // ❌ Package name inconsistent
```

### Go Workspace Configuration

If you encounter "module is not one of the workspace modules" error:

```bash
cd generated/golang
go work use .
```

### HTTP Proxy Configuration

Support for proxy configuration via custom HTTP client:

```go
proxyURL, _ := url.Parse("http://127.0.0.1:10808")
httpClient := &http.Client{
    Transport: &http.Transport{
        Proxy: http.ProxyURL(proxyURL),
    },
    Timeout: 30 * time.Second,
}
client.SetHTTPClient(httpClient)
```

### IP Whitelist

⚠️ **Must configure before first test**

API calls require IP whitelist configuration in CCPayment developer console:

1. Log in to https://console.ccpayment.com/developer/config
2. Find "IP Whitelist" settings
3. Add your public IP address
4. Save configuration

If not configured, you will receive error:
```
ip not in whitelist, please check the ip whitelist settings on the developer page
```

## Generate README.md

Create usage documentation for the generated SDK:

```markdown
# CCPayment Go SDK

## Installation

\`\`\`bash
go get github.com/yourusername/ccpayment-sdk/generated/golang
\`\`\`

## Quick Start

\`\`\`go
package main

import (
    "context"
    "log"
)

func main() {
    client := NewClient("your_app_id", "your_app_secret")
    
    // Get all assets
    resp, err := client.MerchantAssets().GetAppCoinAssetList(context.Background())
    if err != nil {
        log.Fatal(err)
    }
    
    for _, asset := range resp.Assets {
        log.Printf("%s: %s\n", asset.CoinSymbol, asset.Available)
    }
}
\`\`\`

## Configure HTTP Proxy

\`\`\`go
import (
    "net/http"
    "net/url"
    "time"
)

proxyURL, _ := url.Parse("http://127.0.0.1:10808")
httpClient := &http.Client{
    Transport: &http.Transport{Proxy: http.ProxyURL(proxyURL)},
    Timeout: 30 * time.Second,
}
client.SetHTTPClient(httpClient)
\`\`\`

## API Authentication

All API requests must include in Header:
- \`Appid\`: Application ID
- \`Timestamp\`: Current timestamp (seconds)
- \`Sign\`: HMAC-SHA256 signature

SDK automatically handles signature generation.

## Error Handling

\`\`\`go
resp, err := client.MerchantAssets().GetAppCoinAssetList(ctx)
if err != nil {
    if apiErr, ok := err.(*APIError); ok {
        log.Printf("API Error: code=%d, message=%s", apiErr.Code, apiErr.Message)
    } else {
        log.Printf("Other Error: %v", err)
    }
    return
}
\`\`\`
```

## Testing Verification Checklist

After generating code, verify with the following steps:

1. ✅ **Check Output Path**
   ```bash
   # Confirm files are generated to the specified path
   ls -la <output_path>
   
   # Default path example
   ls -la generated/golang
   
   # Custom path example
   ls -la ./sdk/python
   ```

2. ✅ **Compilation Check**
   ```bash
   # Golang
   cd <output_path>
   go build .
   
   # Python
   cd <output_path>
   python3 -m py_compile *.py
   ```

3. ✅ **Configure IP Whitelist**
   - Log in to developer console
   - Add public IP to whitelist

4. ✅ **Run Tests**
   ```bash
   # Golang
   cd <output_path>
   go run .
   
   # Python
   cd <output_path>
   python3 test_main.py
   ```

5. ✅ **Verify Output**
   
   **Full Generation**:
   - Test main interfaces of all 13 modules
   - Verify all data types are parsed correctly
   
   **Modular Generation** (e.g., `merchant-assets`):
   - ✓ Successfully retrieved asset list, total X tokens
   - ✓ Successfully retrieved single coin asset info
   - Confirm data types are parsed correctly
   - Verify error handling works normally

6. ✅ **Code Quality Check**
   ```bash
   # Golang
   go fmt .
   go vet .
   
   # Python
   python3 -m pylint *.py  # Requires pylint
   python3 -m black *.py   # Requires black
   ```

## FAQ

### 1. Package Name Conflict
**Problem**: `found packages main and ccpayment`
**Solution**: Delete or rename conflicting test files, ensure all files use the same package name

### 2. IP Whitelist Error
**Problem**: `ip not in whitelist`
**Solution**: Configure IP whitelist in developer console

### 3. Workspace Error
**Problem**: `module is not one of the workspace modules`
**Solution**: Run `go work use .`

### 4. Proxy Connection Failure
**Problem**: Cannot access API through proxy
**Solution**: Check proxy address and port, ensure proxy service is running normally

### 5. Python SDK Service Accessor Issue
**Problem**: `'function' object has no attribute 'get_coin_list'`
**Cause**: Python client service accessors defined as methods instead of properties
**Solution**: Use `@property` decorator for service accessors:
```python
@property
def basic_info(self):
    from .services.basic_info import BasicInfoService
    if not hasattr(self, '_basic_info'):
        self._basic_info = BasicInfoService(self)
    return self._basic_info
```
Usage: `client.basic_info.get_coin_list()` (not `client.basic_info().get_coin_list()`)

### 6. Python SDK API Response Type Handling
**Problem**: `'list' object has no attribute 'get'`
**Cause**: Some API endpoints return list directly instead of dict with key
**Solution**: Check response type in service methods:
```python
def get_app_coin_asset_list(self) -> List[dict]:
    result = self.client._post("/getAppCoinAssetList", {})
    if isinstance(result, list):
        return result
    return result.get("assets", [])
```

## API模块详细说明

CCPayment API v2共包含13个功能模块，61个HTTP端点：

### 商户模块 (Merchant Modules)

| 模块名称 | 文档文件 | API端点数量 | 主要功能 |
|:------|:---------|:----------|:---------|
| `basic-info` | `api/01-basic-info.md` | 7 | 获取代币列表、法币列表、链信息、代币信息、法币价格等基础信息 |
| `merchant-assets` | `api/02-merchant-assets.md` | 3 | 查询商户资产列表、单币种资产详情、资产余额 |
| `merchant-deposit` | `api/03-merchant-deposit.md` | 4 | 创建商户充值地址、查询充值记录、查询单条充值详情 |
| `merchant-withdraw` | `api/04-merchant-withdraw.md` | 8 | 商户提币申请、查询提币记录、取消提币、查询单条提币详情 |
| `merchant-batch-withdraw` | `api/05-merchant-batch-withdraw.md` | 7 | 批量提币申请、查询批量提币记录、批量取消提币 |

### 用户模块 (User Modules)

| 模块名称 | 文档文件 | API端点数量 | 主要功能 |
|:------|:---------|:----------|:---------|
| `user-assets` | `api/06-user-assets.md` | 2 | 查询用户资产列表、单币种资产详情 |
| `user-deposit` | `api/07-user-deposit.md` | 3 | 创建用户充值地址、查询用户充值记录 |
| `user-withdraw` | `api/08-user-withdraw.md` | 4 | 用户提币申请、查询提币记录、取消提币 |
| `user-transfer` | `api/09-user-transfer.md` | 7 | 用户间转账、批量转账、查询转账记录 |
| `user-swap` | `api/12-user-swap.md` | 3 | 用户闪兑、查询闪兑记录、获取闪兑支持的币种 |

### 其他模块 (Other Modules)

| 模块名称 | 文档文件 | API端点数量 | 主要功能 |
|:------|:---------|:----------|:---------|
| `orders` | `api/10-orders.md` | 3 | 创建订单、查询订单列表、查询订单详情 |
| `swap` | `api/11-swap.md` | 5 | 闪兑下单、查询闪兑记录、获取闪兑汇率 |
| `utilities` | `api/13-utilities.md` | 5 | 网络检测、获取服务器时间、IP白名单检测等工具接口 |

### 认证与请求规范

**Base URL:** `https://ccpayment.com/ccpayment/v2/`

**请求头 (Headers):**
- `Appid`: 应用标识
- `Timestamp`: 毫秒级时间戳
- `Sign`: 请求体签名 (HMAC-SHA256)
- `Content-Type`: `application/json`

**通用响应格式:**
```json
{
  "code": 10000,
  "msg": "success",
  "data": {}
}
```

**状态码说明:**
- `10000`: 请求成功
- 其他代码请参考 `api/appendix.md` 中的错误码定义

---

## 参考文档

### 项目文档
- **项目说明文档**: `README.md` - 包含项目特性、快速开始、使用示例、支持的AI Agent列表
- **代码生成器文档**: `SKILL.md` (本文档) - 代码生成规则、类型映射、验证规则

### API文档
- **API文档入口**: `api/README.md` - API模块索引、认证说明
- **数据类型附录**: `api/appendix.md` - 数据类型定义、错误码列表、验证规则
- **模块详细文档**: `api/01-basic-info.md` ~ `api/13-utilities.md`

### 在线资源
- **官方网站**: https://ccpayment.com
- **开发者控制台**: https://console.ccpayment.com
- **API文档**: https://ccpayment.com/api/doc

### 支持的AI Agent
根据 `README.md`，本SDK生成器支持以下AI Agent：
- **Windsurf** - Codeium的AI编程助手
- **Cursor** - AI代码编辑器
- **Cline** - VS Code的Claude扩展
- **Continue.dev** - 开源AI编程助手
- **Aider** - 命令行AI编程工具
- **Gemini** - Google的AI代码助手
- **Codex** - GitHub Copilot/OpenAI Codex
- **DeepSeek** - DeepSeek Coder AI助手
