# Streamlit Cloud 部署指南

## 🎯 概述

本文档详细说明如何将**期货日报生成器（AI赋能版）**部署到Streamlit Cloud，让用户通过网页访问使用。

---

## 📋 部署前准备

### 1. 必需条件

- ✅ GitHub账号
- ✅ 项目代码已准备好
- ✅ `requirements.txt`文件已配置
- ✅ 代码中不包含硬编码的API密钥

### 2. 文件检查清单

确保项目包含以下文件：

```
期货日报/
├── 期货日报_AI增强专业版.py  # 主程序
├── requirements.txt           # 依赖包列表
├── README.md                  # 项目说明
├── .gitignore                 # Git忽略文件
├── API配置说明.md             # API配置指南
└── Streamlit部署指南.md       # 本文档
```

### 3. requirements.txt检查

确保包含所有依赖：

```txt
streamlit>=1.28.0
akshare>=1.12.0
pandas>=2.0.0
python-docx>=0.8.11
matplotlib>=3.7.0
mplfinance>=0.12.9
requests>=2.31.0
beautifulsoup4>=4.12.0
feedparser>=6.0.10
lxml>=4.9.0
```

---

## 🚀 部署步骤

### 第一步：准备Git仓库

#### 1.1 初始化Git仓库（如果还没有）

```bash
cd 期货日报
git init
```

#### 1.2 创建.gitignore文件

```bash
# 创建.gitignore
echo ".env" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
echo ".DS_Store" >> .gitignore
echo "*.log" >> .gitignore
echo "k_line_chart.png" >> .gitignore
echo "期货日报_*/" >> .gitignore
```

或手动创建`.gitignore`文件，内容如下：

```
# 环境变量（不要上传API密钥）
.env

# Python
__pycache__/
*.pyc
*.pyo
*.pyd

# 系统文件
.DS_Store
Thumbs.db

# 日志文件
*.log

# 生成的文件
k_line_chart.png
期货日报_*/
```

#### 1.3 提交代码

```bash
# 添加所有文件
git add .

# 提交
git commit -m "Initial commit: 期货日报生成器 v4.1"
```

#### 1.4 推送到GitHub

```bash
# 在GitHub上创建新仓库（假设名为 futures-daily-report）
# 然后执行以下命令

git remote add origin https://github.com/你的用户名/futures-daily-report.git
git branch -M main
git push -u origin main
```

---

### 第二步：部署到Streamlit Cloud

#### 2.1 访问Streamlit Cloud

访问：https://streamlit.io/cloud

点击 **"Sign in with GitHub"**

#### 2.2 创建新应用

1. 点击 **"New app"** 按钮

2. 填写部署信息：
   - **Repository：** 选择您的GitHub仓库
   - **Branch：** `main`（或您的主分支）
   - **Main file path：** `期货日报_AI增强专业版.py`

3. （可选）高级设置：
   - **Python version：** 3.9 或 3.10
   - **Custom subdomain：** 自定义访问域名

#### 2.3 点击Deploy

点击 **"Deploy!"** 按钮，等待部署完成（通常需要3-5分钟）

#### 2.4 部署成功

部署成功后，您会得到一个公开访问链接，例如：

```
https://你的用户名-futures-daily-report-xxx.streamlit.app
```

---

## 🔧 部署后配置

### 无需配置！

**重要：** 由于我们采用了用户侧配置的方式，部署后：

- ✅ 不需要在Streamlit Cloud配置API密钥
- ✅ 不需要设置环境变量
- ✅ 每个用户使用自己的API密钥
- ✅ 更安全、更灵活

### 用户使用流程

1. 用户访问您的应用链接
2. 在左侧边栏输入自己的API密钥：
   - DeepSeek API Key
   - Serper API Key
3. 系统验证并显示配置状态
4. 配置完成后即可正常使用

---

## 🎨 自定义配置（可选）

### 1. 自定义域名

在Streamlit Cloud的应用设置中：
- Settings → General → App URL
- 可以修改子域名

### 2. 设置应用图标

在`期货日报_AI增强专业版.py`中：

```python
st.set_page_config(
    page_title="期货日报生成器（AI赋能版）",
    page_icon="📊",  # 可以改为其他emoji或图片路径
    layout="wide"
)
```

### 3. 添加Google Analytics（可选）

在Streamlit Cloud的应用设置中：
- Settings → Advanced → Analytics

---

## 📊 监控和管理

### 查看应用状态

在Streamlit Cloud dashboard中可以查看：
- ✅ 应用运行状态
- ✅ 访问日志
- ✅ 资源使用情况
- ✅ 错误日志

### 更新应用

有两种方式更新应用：

#### 方法1：推送代码（自动部署）

```bash
# 修改代码后
git add .
git commit -m "更新说明"
git push

# Streamlit Cloud会自动检测并重新部署
```

#### 方法2：手动重启

在Streamlit Cloud dashboard中：
- 点击 "⋮" → "Reboot app"

---

## 🔒 安全性最佳实践

### ✅ 已实现的安全措施

1. **API密钥不在代码中**
   ```python
   # ✅ 正确做法
   DEFAULT_DEEPSEEK_API_KEY = os.getenv("DEEPSEEK_API_KEY", "")
   ```

2. **使用password输入框**
   ```python
   # ✅ 密钥不会在屏幕上明文显示
   st.sidebar.text_input("API Key", type="password")
   ```

3. **会话级存储**
   - API密钥仅在当前会话有效
   - 不会保存到数据库或文件

4. **.gitignore配置**
   - `.env`文件不会上传到GitHub

### ⚠️ 需要注意的

1. **不要在截图中显示API密钥**
2. **不要在文档中写入真实密钥**
3. **定期更换API密钥**
4. **监控API使用量**

---

## 🐛 常见问题

### Q1: 部署失败，显示"Module not found"

**原因：** `requirements.txt`中缺少某个依赖包

**解决方法：**
```bash
# 在本地测试
pip install -r requirements.txt
streamlit run 期货日报_AI增强专业版.py

# 确保所有依赖都正常后，再推送代码
git add requirements.txt
git commit -m "更新依赖"
git push
```

---

### Q2: 部署后应用加载很慢

**原因：** 可能是首次加载，需要安装依赖

**解决方法：**
- 首次部署通常需要3-5分钟
- 后续访问会快很多
- 如果持续很慢，检查代码是否有死循环或大量计算

---

### Q3: 如何查看错误日志？

**方法1：** 在Streamlit Cloud dashboard
- 点击应用 → Logs → 查看实时日志

**方法2：** 在应用页面
- 点击右下角的 "Manage app" → View logs

---

### Q4: 应用可以被所有人访问吗？

**默认：** 是的，部署后的应用是公开的

**限制访问：**
- 在Streamlit Cloud的免费版中，无法限制访问
- 如需限制，可以：
  1. 升级到Streamlit Teams
  2. 或在代码中添加密码验证

---

### Q5: 免费版有什么限制？

**Streamlit Cloud免费版限制：**
- ✅ 1个公开应用
- ✅ 资源：1 GB内存，1 CPU
- ⚠️ 如果7天无人访问，应用会休眠
- ⚠️ 不支持私有应用（需要升级）

---

### Q6: 如何添加密码保护？

可以使用`streamlit-authenticator`库：

```python
import streamlit_authenticator as stauth

# 配置用户和密码
names = ['用户1']
usernames = ['user1']
passwords = ['password123']

# 创建认证器
authenticator = stauth.Authenticate(names, usernames, passwords, 
                                     'cookie_name', 'signature_key')

# 添加登录表单
name, authentication_status, username = authenticator.login('Login', 'main')

if authentication_status:
    # 显示应用内容
    st.write('欢迎')
elif authentication_status == False:
    st.error('用户名或密码错误')
elif authentication_status == None:
    st.warning('请输入用户名和密码')
```

---

## 📈 性能优化建议

### 1. 使用st.cache_data缓存数据

```python
@st.cache_data(ttl=3600)  # 缓存1小时
def get_market_data(symbol):
    # 获取市场数据
    return data
```

### 2. 异步加载

对于耗时操作，使用`st.spinner`提示用户：

```python
with st.spinner("正在生成K线图..."):
    # 耗时操作
    result = generate_kline()
```

### 3. 减少重复请求

使用session_state存储数据，避免重复请求：

```python
if 'market_data' not in st.session_state:
    st.session_state.market_data = fetch_data()
```

---

## 🎯 部署检查清单

部署前请确认：

- [ ] 代码中没有硬编码的API密钥
- [ ] `.env`文件在`.gitignore`中
- [ ] `requirements.txt`包含所有依赖
- [ ] 代码在本地运行正常
- [ ] 已推送到GitHub
- [ ] 在Streamlit Cloud创建应用
- [ ] 应用部署成功并可访问
- [ ] 测试API密钥配置功能
- [ ] 测试所有功能是否正常

---

## 🔗 相关资源

- **Streamlit官方文档：** https://docs.streamlit.io/
- **Streamlit Cloud文档：** https://docs.streamlit.io/streamlit-community-cloud
- **GitHub Pages：** https://pages.github.com/
- **项目README：** [README.md](README.md)
- **API配置说明：** [API配置说明.md](API配置说明.md)

---

## 📞 支持

如有问题，请联系：
- **邮箱：** 953534947@qq.com
- **作者：** 7haoge

---

## 🎉 部署成功！

恭喜！您的期货日报生成器现在已经可以通过网页访问了！

**分享您的应用：**
```
https://你的用户名-futures-daily-report-xxx.streamlit.app
```

**下一步：**
1. 分享链接给用户
2. 收集用户反馈
3. 持续改进功能
4. 定期更新维护

---

**更新日期：** 2025-10-13
**版本：** v4.1（支持在线部署）

