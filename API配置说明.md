# API配置说明

## 📋 概述

期货日报生成器支持两种API密钥配置方式：

1. **本地开发** - 通过环境变量配置
2. **在线部署** - 用户在网页界面输入

---

## 🔑 需要的API密钥

### 1. DeepSeek API

**用途：** AI生成行情描述、主要观点、新闻资讯

**申请地址：** https://platform.deepseek.com/

**申请步骤：**
1. 访问官网注册账号
2. 登录后进入"API Keys"页面
3. 点击"Create API Key"创建密钥
4. 复制以 `sk-` 开头的密钥

**免费额度：** 新用户有免费额度可用

**密钥格式：** `sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

---

### 2. Serper API

**用途：** 搜索新闻资讯、获取专业数据（库存、基差、持仓等）

**申请地址：** https://serper.dev/

**申请步骤：**
1. 访问官网注册账号
2. 登录后在Dashboard页面查看API密钥
3. 复制显示的密钥

**免费额度：** 新用户免费2,500次搜索

**密钥格式：** 长度约40-50字符的字符串

---

## 💻 本地开发配置

### 方法1：环境变量（推荐）

**Windows (PowerShell):**
```powershell
$env:DEEPSEEK_API_KEY="sk-your-deepseek-api-key"
$env:SERPER_API_KEY="your-serper-api-key"
streamlit run 期货日报_AI增强专业版.py
```

**Windows (命令提示符):**
```cmd
set DEEPSEEK_API_KEY=sk-your-deepseek-api-key
set SERPER_API_KEY=your-serper-api-key
streamlit run 期货日报_AI增强专业版.py
```

**Linux/Mac:**
```bash
export DEEPSEEK_API_KEY="sk-your-deepseek-api-key"
export SERPER_API_KEY="your-serper-api-key"
streamlit run 期货日报_AI增强专业版.py
```

---

### 方法2：.env文件

1. 在项目根目录创建 `.env` 文件

2. 添加以下内容：
```
DEEPSEEK_API_KEY=sk-your-deepseek-api-key
SERPER_API_KEY=your-serper-api-key
```

3. 安装python-dotenv：
```bash
pip install python-dotenv
```

4. 在代码开头添加：
```python
from dotenv import load_dotenv
load_dotenv()
```

5. 运行程序：
```bash
streamlit run 期货日报_AI增强专业版.py
```

**注意：** `.env`文件应该添加到`.gitignore`，避免泄露密钥

---

### 方法3：直接在侧边栏输入

即使在本地开发，如果没有配置环境变量，也可以在网页左侧边栏直接输入API密钥。

**优点：** 简单快速，不需要配置环境变量

**缺点：** 每次启动都需要重新输入

---

## 🌐 在线部署配置（Streamlit Cloud）

### 部署步骤

#### 1. 准备Git仓库

```bash
# 初始化Git仓库
git init

# 添加.gitignore文件，排除敏感信息
echo ".env" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
echo ".DS_Store" >> .gitignore

# 提交代码
git add .
git commit -m "Initial commit"

# 推送到GitHub/GitLab等
git remote add origin <your-repo-url>
git push -u origin main
```

#### 2. 部署到Streamlit Cloud

1. 访问 https://streamlit.io/cloud
2. 登录GitHub账号
3. 点击 "New app"
4. 选择仓库、分支、主文件（期货日报_AI增强专业版.py）
5. 点击 "Deploy"

#### 3. 用户使用

用户访问您的应用后：
1. 在左侧边栏看到"⚙️ API配置"
2. 输入自己的DeepSeek API密钥
3. 输入自己的Serper API密钥
4. 系统显示配置状态
5. 配置完成后即可使用所有功能

---

## 🔒 安全性说明

### ✅ 安全做法

1. **不在代码中硬编码密钥**
   ```python
   # ❌ 错误做法
   DEEPSEEK_API_KEY = "sk-123456789..."
   
   # ✅ 正确做法
   DEEPSEEK_API_KEY = os.getenv("DEEPSEEK_API_KEY", "")
   ```

2. **不将.env文件提交到Git**
   - 确保`.env`在`.gitignore`中
   - 提供`.env.example`作为模板

3. **使用password类型输入框**
   ```python
   st.sidebar.text_input("API Key", type="password")
   ```

4. **会话级存储**
   - API密钥仅在当前浏览器会话有效
   - 关闭浏览器或刷新页面后需要重新输入
   - 不会被保存到服务器或本地存储

---

### ⚠️ 避免的做法

1. ❌ 在公开代码中硬编码API密钥
2. ❌ 将包含密钥的`.env`文件上传到GitHub
3. ❌ 在截图或文档中显示完整密钥
4. ❌ 与他人共享您的API密钥

---

## 🛠️ 配置验证

系统会自动验证API密钥格式：

### DeepSeek API验证

- ✅ 以 `sk-` 开头
- ❌ 格式错误提示

### Serper API验证

- ✅ 长度 ≥ 30字符
- ❌ 格式错误提示

### 功能可用性检查

| API配置状态 | 可用功能 |
|------------|---------|
| ✅ DeepSeek + ✅ Serper | **全部功能** |
| ✅ DeepSeek + ❌ Serper | K线图、行情描述、主要观点（受限）、新闻资讯（受限） |
| ❌ DeepSeek + ✅ Serper | K线图、新闻搜索 |
| ❌ DeepSeek + ❌ Serper | 仅K线图生成 |

---

## 📊 配置状态显示

在左侧边栏会实时显示：

```
📊 配置状态
✅ DeepSeek API
✅ Serper API
```

或

```
📊 配置状态
❌ DeepSeek API
❌ Serper API

⚠️ AI生成功能不可用
⚠️ 新闻搜索功能受限
```

---

## 🔗 相关链接

- **DeepSeek官网：** https://platform.deepseek.com/
- **Serper官网：** https://serper.dev/
- **Streamlit Cloud：** https://streamlit.io/cloud
- **项目GitHub：** [您的仓库链接]

---

## 💡 常见问题

### Q1: 本地开发一定要配置环境变量吗？

**A:** 不是必须的。您可以：
- 配置环境变量（方便，不用每次输入）
- 或在侧边栏输入（简单，但每次启动都要输入）

---

### Q2: 部署到Streamlit Cloud需要配置环境变量吗？

**A:** **不需要**。部署后：
- 用户在网页界面输入自己的API密钥
- 每个用户使用自己的密钥
- 不需要您提供密钥

---

### Q3: API密钥会被保存吗？

**A:** **不会**。API密钥：
- 仅存储在当前浏览器会话中
- 关闭浏览器或刷新页面后需要重新输入
- 不会保存到服务器或本地文件

---

### Q4: 如何保护我的API密钥？

**A:** 遵循以下原则：
1. 不要在代码中硬编码
2. 不要上传到GitHub
3. 不要与他人共享
4. 定期更换密钥
5. 发现泄露立即重置

---

### Q5: 免费额度用完了怎么办？

**A:** 
- **DeepSeek:** 可以充值购买更多额度
- **Serper:** 可以升级到付费计划
- 或使用其他API服务替代

---

## 📞 技术支持

如有问题，请联系：
- **邮箱：** 953534947@qq.com
- **作者：** 7haoge

---

**更新日期：** 2025-10-13
**版本：** v4.1（支持在线部署）

