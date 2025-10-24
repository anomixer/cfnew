### 感覺跳轉加群有點流氓行為 改成

<img width="1708" height="884" alt="image" src="https://github.com/user-attachments/assets/ca35ae39-6971-4291-b182-28cb292c0353" />


想加群的自己點選新增吧 tg交流群 https://t.me/+ft-zI76oovgwNmRh 

###  Snippets

<img width="1128" height="801" alt="image" src="https://github.com/user-attachments/assets/ae108dd2-c543-4a63-b448-d56d4d520e1d" />

#### 加入多客戶端支援 域名/你的uuid即可看見

###  配套工具

| 型別 | 描述 | 連結 |
| :--- | :--- | :--- |
|  **文字教程** | 詳細的部署與使用說明部落格文章 | [https://joeyblog.net/yuanchuang/1146.html](https://joeyblog.net/yuanchuang/1146.html) |
|  **Workers影片教程** | 直觀的操作演示和功能講解 | https://youtu.be/Rlypv_iswD8 |
|  **Snippets影片教程** | 直觀的操作演示和功能講解 | https://www.youtube.com/watch?v=xeFeH3Akcu8 |

###  部署
	
加入了千呼萬喚的訂閱每15分鐘自動優選一次

#### 🔧 基礎配置
| 變數名 | 值 | 說明 |
| :--- | :--- | :--- |
| `u` | `你的 UUID` | **必需**。用於訪問訂閱和配置管理介面 |
| `p` | `proxyip` | **可選**。自定義ProxyIP地址和埠 |
| `s` | `你的SOCKS5地址` | **可選**。用於將所有出站流量透過 SOCKS5 代理轉發，格式為 `user:pass@host:port` 或 `host:port` |
| `d` | `你的訂閱地址` | **可選**。不填就是/你的uuid |
| `wk` | `地區程式碼` | **可選**。手動指定Worker地區，如：`SG`、`HK`、`US`、`JP`等 |

#### 🎯 圖形化配置（推薦）
- **KV儲存配置**：在Workers中建立KV名稱空間，繫結環境變數 `C`
- **訪問介面**：部署後訪問 `/{你的UUID}` 即可使用圖形化配置管理
- **實時生效**：透過介面修改配置無需重新部署，立即生效

#### 🔧 高階控制
| 變數名 | 值 | 說明 |
| :--- | :--- | :--- |
| `yx` | `自定義優選IP/域名` | **可選**。支援節點命名，格式：`1.1.1.1:443#香港節點,8.8.8.8:53#Google DNS` |
| `yxURL` | `優選IP來源URL` | **可選**。自定義優選IP列表來源URL，留空則使用預設地址 |
| `qj` | `no` | **可選**。降級控制，設定為`no`時啟用降級模式：CF直連失敗→SOCKS5連線→fallback地址 |
| `dkby` | `yes` | **可選**。TLS控制，設定為`yes`時只生成TLS節點，不生成非TLS節點（如80埠） |
| `yxby` | `yes` | **可選**。優選控制，設定為`yes`時關閉所有優選功能，只使用原生地址，不生成優選IP和域名節點 |
| `rm` | `no` | **可選**。地區匹配控制，設定為`no`時關閉地區智慧匹配 |
| `apiEnabled` | `yes` | **可選**。API管理開關，設定為`yes`時允許透過API動態管理優選IP（預設關閉） |

#### 📦 KV儲存設定（可選但推薦）
1. 在Cloudflare Workers中建立KV名稱空間
2. 在Workers設定中繫結KV名稱空間，變數名設為 `C`
3. 重新部署Workers
4. 訪問 `/{你的UUID}` 即可使用圖形化配置管理

#### 🔑 API快速開始
1. https://github.com/byJoey/yx-tools/releases 優選軟體
2. **開啟API功能**：訪問 `/{UUID}` → 找到"允許API管理"→ 選擇"開啟API管理"→ 儲存
3. **新增單個IP**：
```bash
curl -X POST "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '{"ip": "1.2.3.4", "port": 443, "name": "香港節點"}'
```
3. **批次新增IP**：
```bash
curl -X POST "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '[
    {"ip": "1.2.3.4", "port": 443, "name": "節點1"},
    {"ip": "5.6.7.8", "port": 8443, "name": "節點2"}
  ]'
```
4. **一鍵清空**：
```bash
curl -X DELETE "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '{"all": true}'
```

###  新功能

#### 🎯 圖形化配置管理
- **KV儲存支援**：使用Cloudflare KV儲存持久化配置
- **圖形化介面**：訪問 `/{你的UUID}` 即可使用配置管理介面
- **實時配置**：無需重新部署，配置立即生效
- **配置優先順序**：KV配置 > 環境變數 > 預設值

#### 🚀 API動態管理（新增）
- **API管理**：透過RESTful API動態管理優選IP，無需修改程式碼
- **批次上報**：支援一次性批次新增多個優選IP
- **一鍵清空**：支援清空所有優選IP，快速更新列表
- **安全開關**：預設關閉，需在圖形介面手動開啟API功能
- **自動合併**：API新增的IP與手動配置的yx變數自動合併
- **實時同步**：API新增的IP立即在配置頁面顯示
- **API端點**：
  - `GET /{UUID}/api/preferred-ips` - 查詢優選IP列表
  - `POST /{UUID}/api/preferred-ips` - 新增優選IP（支援單個/批次）
  - `DELETE /{UUID}/api/preferred-ips` - 刪除優選IP（支援單個/全部）


#### 🌍 手動指定地區
- **地區選擇**：支援手動指定Worker地區，覆蓋自動檢測
- **設定方式**：`wk=SG` 或透過圖形化介面選擇
- **支援地區**：US、SG、JP、HK、KR、DE、SE、NL、FI、GB 
- **智慧顯示**：系統狀態會顯示"手動指定地區"而非"自動檢測"

#### 🏷️ 優選節點命名
- **自定義名稱**：支援為優選節點設定自定義名稱
- **格式支援**：`IP:埠#節點名稱` 或 `IP:埠`（使用預設名稱）
- **示例**：`1.1.1.1:443#香港節點,8.8.8.8:53#Google DNS`
- **預設格式**：未設定名稱時自動生成 `自定義優選-IP:埠`

#### 📊 系統狀態監控
- **實時檢測**：顯示Worker地區、檢測方式、ProxyIP狀態
- **智慧匹配**：同地區 → 鄰近地區 → 其他地區的選擇邏輯
- **狀態指示**：視覺化顯示系統執行狀態和配置資訊

#### 🔧 高階控制選項
- **地區匹配控制**：`rm=no` 關閉地區智慧匹配
- **降級控制**：`qj=no` 啟用降級模式（CF直連失敗→SOCKS5→fallback）
- **TLS控制**：`dkby=yes` 只生成TLS節點，不生成非TLS節點
- **優選控制**：`yxby=yes` 關閉所有優選功能

#### 🎨 多客戶端支援
- **訂閱格式**：支援Clash、Surge、Sing-box、Loon、V2Ray等
- **自動轉換**：根據客戶端型別自動生成對應配置
- **一鍵獲取**：圖形化介面一鍵生成訂閱連結

#### ⚡ 效能最佳化
- **智慧優選**：每15分鐘自動優選一次，保持最佳效能
- **容錯機制**：多重備用方案，確保服務穩定性
- **快取最佳化**：智慧快取機制，減少重複計算

###  致謝

  * 本專案基於 [zizifn/edgetunnel](https://github.com/zizifn/edgetunnel) 修改，感謝原作者的貢獻。
  * 本專案內建ProxyIP 來自CM [[cmliu](https://github.com/cmliu)) ，感謝作者的貢獻。
  * 本專案反代IP來著前端獨苗kejiland  [[qwer-search](https://github.com/qwer-search)) ，感謝作者的貢獻。
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=byJoey/cfnew&type=Timeline)](https://www.star-history.com/#byJoey/cfnew&Timeline&LogScale)
