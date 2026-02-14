# Apple Sign In 登录功能实现

## 后端接口

**服务地址：** `http://localhost:8888`

**接口文档：** `/Users/jiangshen/go/src/moodlet-backend/doc/moodlet.json`

### POST /api/app/auth/apple

Apple 登录认证接口

**请求：**
```json
{
  "identityToken": "Apple 返回的 identityToken (JWT 格式)"
}
```

**响应：**
```json
{
  "code": 0,
  "msg": "",
  "err": "",
  "data": {
    "token": "JWT token，后续请求需带在 Authorization header",
    "user": {
      "id": 123,
      "nickname": "用户昵称",
      "avatar": "头像URL"
    }
  }
}
```

**错误处理：**
- `code = 0` 表示成功
- `code != 0` 时，`msg` 为用户提示信息，`err` 为开发者调试信息

## 实现步骤

### 步骤 1：配置 Xcode 项目

在 Xcode 中添加 "Sign in with Apple" capability：

1. 打开 `moodlet-app-ios.xcodeproj`
2. 选择项目 target → "Signing & Capabilities"
3. 点击 "+ Capability"
4. 搜索并添加 "Sign in with Apple"

### 步骤 2：创建网络层

创建 `Services/APIService.swift`：
- 基础 HTTP 请求封装
- 处理 JSON 编解码
- 统一错误处理
- Token 自动附加

### 步骤 3：创建认证管理器

创建 `Services/AuthManager.swift`：
- 管理登录状态（@Published isLoggedIn）
- Token 存储（Keychain）
- 用户信息缓存
- 登出功能

### 步骤 4：创建登录页面

创建 `Views/LoginView.swift`：
- Apple Sign In 按钮（ASAuthorizationAppleIDButton）
- Tagline: "今天的你，还好吗？"
- 处理登录回调
- 调用后端接口

### 步骤 5：更新 App 入口

修改 `moodlet_app_iosApp.swift`：
- 注入 AuthManager 为环境对象
- 根据登录状态切换 LoginView / ContentView

## 关键代码参考

### Apple Sign In 核心流程

```swift
import AuthenticationServices

// 1. 创建请求
let request = ASAuthorizationAppleIDProvider().createRequest()
request.requestedScopes = [.fullName, .email]

// 2. 执行授权
let controller = ASAuthorizationController(authorizationRequests: [request])
controller.delegate = self
controller.performRequests()

// 3. 处理回调
func authorizationController(controller: ASAuthorizationController,
                            didCompleteWithAuthorization authorization: ASAuthorization) {
    if let credential = authorization.credential as? ASAuthorizationAppleIDCredential,
       let identityToken = credential.identityToken,
       let tokenString = String(data: identityToken, encoding: .utf8) {
        // 发送 tokenString 到后端 /api/app/auth/apple
    }
}
```

### 项目文件结构（建议）

```
moodlet-app-ios/
├── moodlet_app_iosApp.swift
├── ContentView.swift
├── Models/
│   └── User.swift              # 用户模型
├── Services/
│   ├── APIService.swift        # 网络请求
│   └── AuthManager.swift       # 认证管理
└── Views/
    └── LoginView.swift         # 登录页面
```

## 注意事项

1. **真机调试**：Sign in with Apple 需要真机或模拟器测试，且需要有效的 Apple Developer 账号
2. **Keychain 存储**：Token 应存储在 Keychain 而非 UserDefaults
3. **Token 刷新**：考虑 JWT 过期后的刷新机制
4. **首次登录**：Apple 只在首次授权时返回用户邮箱和姓名

## 相关资源

- 设计稿：`/Users/jiangshen/创意/Moodlet/页面/moodlet-login.html`
- 后端项目：`/Users/jiangshen/go/src/moodlet-backend/`
