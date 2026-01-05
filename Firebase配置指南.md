# 佳妮 & 宇轩 - 情侣网站

一个精美的情侣专属网站，支持相册、未来计划和存钱罐功能。

## 🌟 功能特点

- **📷 相册** - 上传、查看、删除照片，自动压缩优化
- **📝 未来计划** - 添加和管理长期计划，支持分类
- **💰 存钱罐** - 虚拟存款账户，记录存取款历史
- **🔐 账号系统** - 三个账号，访客只能浏览
- **📱 移动端适配** - 完美支持手机浏览
- **☁️ 云端同步** - 数据实时同步，多设备访问

## 📱 账号信息

| 用户 | 账号 | 密码 | 权限 |
|------|------|------|------|
| 赵佳妮 | `zjn` | `jianiloveyuxuan` | 完全权限 |
| 李宇轩 | `lyx` | `yuxuanlovejianni` | 完全权限 |
| 访客 | `guest` | `welcome2025` | 仅浏览 |

> 💡 可以在 `index.html` 中的 `USERS` 对象修改账号和密码

---

## 🔧 Firebase 配置指南

### 第一步：创建 Firebase 项目

1. 访问 [Firebase Console](https://console.firebase.google.com/)
2. 点击「添加项目」或「Add project」
3. 输入项目名称（例如：`zjn-lyx-love`）
4. 可以关闭 Google Analytics（个人项目不需要）
5. 点击「创建项目」

### 第二步：启用 Realtime Database

1. 在左侧菜单选择「构建」→「Realtime Database」
2. 点击「创建数据库」
3. 选择数据库位置（推荐选择离你最近的区域）
4. **重要**：选择「以测试模式启动」（稍后会配置安全规则）
5. 点击「启用」

### 第三步：启用 Storage（存储照片）

1. 在左侧菜单选择「构建」→「Storage」
2. 点击「开始使用」
3. **重要**：选择「以测试模式启动」
4. 选择存储位置（推荐与数据库相同区域）
5. 点击「完成」

### 第四步：获取配置信息

1. 点击左上角的⚙️齿轮图标 →「项目设置」
2. 向下滚动到「您的应用」部分
3. 点击 Web 图标 `</>`
4. 输入应用昵称（例如：`couple-web`）
5. 不需要勾选 Firebase Hosting
6. 点击「注册应用」
7. 复制显示的 `firebaseConfig` 配置

配置示例：
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyB.....................",
    authDomain: "zjn-lyx-love.firebaseapp.com",
    databaseURL: "https://zjn-lyx-love-default-rtdb.firebaseio.com",
    projectId: "zjn-lyx-love",
    storageBucket: "zjn-lyx-love.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abcdef123456"
};
```

### 第五步：配置网站

1. 打开 `index.html` 文件
2. 找到以下代码（大约在第 650 行附近）：

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    ...
};
```

3. 将其替换为你在第四步复制的配置

### 第六步：配置安全规则

#### Realtime Database 规则

1. 在 Firebase Console 中，进入「Realtime Database」→「规则」
2. 将规则替换为：

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ 注意：这是简化规则，允许任何人读写。因为网站有前端登录验证，这样配置是可行的。如果需要更安全的配置，可以联系我。

#### Storage 规则

1. 进入「Storage」→「Rules」
2. 将规则替换为：

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```

3. 点击「发布」

---

## 🚀 部署到 Netlify

### 方法一：通过 GitHub（推荐）

1. 将代码推送到 GitHub 仓库
2. 登录 [Netlify](https://www.netlify.com/)
3. 点击「Add new site」→「Import an existing project」
4. 选择 GitHub 并授权
5. 选择你的仓库
6. 构建设置保持默认（不需要构建命令）
7. 点击「Deploy site」
8. 部署完成后，在「Domain settings」中添加自定义域名 `zjn-lyx.com`

### 方法二：直接拖拽

1. 登录 Netlify
2. 将包含 `index.html` 的文件夹直接拖到 Netlify 的部署区域
3. 等待部署完成
4. 配置自定义域名

### 配置自定义域名

1. 在 Netlify 站点设置中，点击「Domain settings」
2. 点击「Add custom domain」
3. 输入 `zjn-lyx.com`
4. 按照提示在你的域名注册商处配置 DNS：
   - 添加 CNAME 记录：`www` → `你的netlify域名.netlify.app`
   - 或添加 A 记录指向 Netlify IP

---

## 📁 文件结构

```
couple-website/
├── index.html      # 主页面（包含所有代码）
└── README.md       # 说明文档
```

## 🔒 安全建议

1. **修改默认密码**：部署后建议修改 `USERS` 中的密码
2. **定期备份**：虽然 Firebase 会自动保存数据，建议定期导出备份
3. **监控用量**：Firebase 免费额度足够个人使用，但建议关注用量

## 💡 常见问题

### Q: 照片上传失败？
A: 检查 Firebase Storage 规则是否正确配置，以及是否超出免费额度（5GB存储/月）

### Q: 数据不同步？
A: 检查 Firebase Realtime Database 规则，确保 `.read` 和 `.write` 都为 `true`

### Q: 如何修改目标存款金额？
A: 在 `index.html` 中找到 `const GOAL_AMOUNT = 100000;`，修改数字即可

### Q: 如何添加更多账号？
A: 在 `USERS` 对象中添加新的账号配置

---

## 📞 技术支持

如有问题，请检查浏览器控制台（F12）中的错误信息。

祝你们幸福！💕
