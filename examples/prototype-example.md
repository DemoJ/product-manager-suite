# HTML 原型设计示例

## 场景

某电商平台的产品经理收到需求：用户反馈在商品列表页找不到想要的商品，希望增加筛选功能。

需求描述：
- 平台：移动端 App
- 功能：商品列表页增加筛选和排序
- 目标：提升用户找到商品的效率，降低搜索失败率

## 工作流

### 1. 确认平台

从需求中识别：**移动端**（App 内嵌页面）

### 2. 梳理页面

需要设计的页面：
- 商品列表页（主页面）
- 筛选弹窗（底部弹出）
- 商品详情页（点击商品进入）

### 3. 确定交互

**商品列表页：**
- 顶部搜索栏：点击跳转到搜索页
- 排序 Tab：综合/价格/销量，点击切换排序
- 筛选按钮：点击打开筛选弹窗
- 商品卡片：点击进入详情页
- 下拉刷新：刷新列表
- 上拉加载：加载更多商品

**筛选弹窗：**
- 分类筛选：多选
- 价格区间：输入最低/最高价格
- 品牌筛选：多选
- 重置按钮：清空所有筛选
- 确定按钮：应用筛选条件，关闭弹窗

**商品详情页：**
- 返回按钮：返回列表页
- 商品图片轮播
- 商品信息：标题、价格、描述
- 加入购物车按钮

### 4. 构建原型

基于 `templates/wireframe-base.html` 模板，创建完整原型。

### 5. 编写配套文档

见下方"原型说明文档"部分。

## 原型代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>商品列表筛选 - 移动端原型</title>
  <style>
    /* Token 变量系统 */
    :root {
      --text-primary: #1a1a1a;
      --text-regular: #333333;
      --text-secondary: #666666;
      --text-placeholder: #999999;
      --border-strong: #d9d9d9;
      --border-regular: #e8e8e8;
      --border-light: #f0f0f0;
      --bg-page: #f5f5f5;
      --bg-card: #fafafa;
      --bg-container: #ffffff;
      --btn-primary: #333333;
      --btn-primary-hover: #555555;
      --selected-bg: #f0f0f0;
      --shadow-light: 0 1px 4px rgba(0,0,0,0.08);
      --shadow-medium: 0 4px 12px rgba(0,0,0,0.12);
      --shadow-heavy: 0 8px 24px rgba(0,0,0,0.16);
      --radius-sm: 4px;
      --radius-md: 8px;
      --radius-lg: 12px;
      --spacing-xs: 4px;
      --spacing-sm: 8px;
      --spacing-md: 12px;
      --spacing-lg: 16px;
      --spacing-xl: 24px;
      --spacing-2xl: 32px;
      --font-size-xs: 12px;
      --font-size-sm: 14px;
      --font-size-md: 16px;
      --font-size-lg: 20px;
      --transition-fast: 0.15s ease;
      --transition-normal: 0.2s ease;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", sans-serif;
      font-size: var(--font-size-sm);
      line-height: 1.5;
      color: var(--text-regular);
      background-color: var(--bg-page);
    }

    .mobile-container {
      max-width: 375px;
      margin: 0 auto;
      background: var(--bg-container);
      min-height: 100vh;
      position: relative;
      box-shadow: var(--shadow-medium);
    }

    /* 顶部搜索栏 */
    .search-bar {
      position: sticky;
      top: 0;
      background: var(--bg-container);
      padding: var(--spacing-md) var(--spacing-lg);
      border-bottom: 1px solid var(--border-regular);
      z-index: 100;
    }

    .search-input {
      width: 100%;
      padding: var(--spacing-sm) var(--spacing-md);
      border: 1px solid var(--border-strong);
      border-radius: var(--radius-sm);
      font-size: var(--font-size-sm);
    }

    /* 排序和筛选栏 */
    .filter-bar {
      display: flex;
      background: var(--bg-container);
      border-bottom: 1px solid var(--border-regular);
      padding: 0 var(--spacing-lg);
    }

    .sort-tab {
      flex: 1;
      padding: var(--spacing-md);
      text-align: center;
      color: var(--text-secondary);
      cursor: pointer;
      border-bottom: 2px solid transparent;
      transition: all var(--transition-fast);
    }

    .sort-tab.active {
      color: var(--text-primary);
      border-bottom-color: var(--text-primary);
    }

    .filter-btn {
      padding: var(--spacing-md) var(--spacing-lg);
      color: var(--text-secondary);
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: var(--spacing-xs);
    }

    .filter-btn::after {
      content: '⚙';
      font-size: var(--font-size-md);
    }

    /* 商品列表 */
    .product-list {
      padding: var(--spacing-md);
    }

    .product-card {
      background: var(--bg-container);
      border: 1px solid var(--border-regular);
      border-radius: var(--radius-md);
      margin-bottom: var(--spacing-md);
      overflow: hidden;
      cursor: pointer;
      transition: all var(--transition-fast);
    }

    .product-card:hover {
      border-color: var(--border-strong);
      box-shadow: var(--shadow-light);
    }

    .product-image {
      width: 100%;
      height: 200px;
      background: var(--border-light);
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--text-placeholder);
    }

    .product-info {
      padding: var(--spacing-md);
    }

    .product-title {
      font-size: var(--font-size-sm);
      color: var(--text-primary);
      margin-bottom: var(--spacing-sm);
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .product-price {
      font-size: var(--font-size-md);
      color: var(--text-primary);
      font-weight: 600;
    }

    .product-sales {
      font-size: var(--font-size-xs);
      color: var(--text-secondary);
      margin-top: var(--spacing-xs);
    }

    /* 筛选弹窗 */
    .overlay {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.5);
      display: none;
      z-index: 1000;
    }

    .overlay.active {
      display: block;
    }

    .bottom-sheet {
      position: fixed;
      bottom: -100%;
      left: 50%;
      transform: translateX(-50%);
      width: 100%;
      max-width: 375px;
      background: var(--bg-container);
      border-radius: var(--radius-lg) var(--radius-lg) 0 0;
      transition: bottom var(--transition-normal);
      z-index: 1001;
      max-height: 80vh;
      overflow-y: auto;
    }

    .bottom-sheet.active {
      bottom: 0;
    }

    .sheet-header {
      padding: var(--spacing-lg);
      border-bottom: 1px solid var(--border-regular);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .sheet-title {
      font-size: var(--font-size-md);
      font-weight: 600;
      color: var(--text-primary);
    }

    .sheet-close {
      font-size: var(--font-size-lg);
      color: var(--text-secondary);
      cursor: pointer;
    }

    .sheet-content {
      padding: var(--spacing-lg);
    }

    .filter-section {
      margin-bottom: var(--spacing-xl);
    }

    .filter-label {
      font-size: var(--font-size-sm);
      font-weight: 600;
      color: var(--text-primary);
      margin-bottom: var(--spacing-md);
    }

    .filter-options {
      display: flex;
      flex-wrap: wrap;
      gap: var(--spacing-sm);
    }

    .filter-option {
      padding: var(--spacing-sm) var(--spacing-md);
      border: 1px solid var(--border-strong);
      border-radius: var(--radius-sm);
      cursor: pointer;
      transition: all var(--transition-fast);
    }

    .filter-option.selected {
      background: var(--selected-bg);
      border-color: var(--text-primary);
    }

    .price-range {
      display: flex;
      gap: var(--spacing-sm);
      align-items: center;
    }

    .price-input {
      flex: 1;
      padding: var(--spacing-sm);
      border: 1px solid var(--border-strong);
      border-radius: var(--radius-sm);
    }

    .sheet-footer {
      padding: var(--spacing-lg);
      border-top: 1px solid var(--border-regular);
      display: flex;
      gap: var(--spacing-sm);
    }

    .btn-reset {
      flex: 1;
      padding: var(--spacing-md);
      border: 1px solid var(--border-strong);
      background: var(--bg-container);
      border-radius: var(--radius-sm);
      cursor: pointer;
    }

    .btn-apply {
      flex: 2;
      padding: var(--spacing-md);
      background: var(--btn-primary);
      color: var(--bg-container);
      border: none;
      border-radius: var(--radius-sm);
      cursor: pointer;
    }

    /* Toast */
    .toast {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(0, 0, 0, 0.8);
      color: var(--bg-container);
      padding: var(--spacing-md) var(--spacing-xl);
      border-radius: var(--radius-sm);
      z-index: 2000;
      display: none;
    }

    .toast.active {
      display: block;
      animation: fadeInOut 2s ease;
    }

    @keyframes fadeInOut {
      0% { opacity: 0; }
      20% { opacity: 1; }
      80% { opacity: 1; }
      100% { opacity: 0; }
    }
  </style>
</head>
<body>
  <div class="mobile-container">
    <!-- 顶部搜索栏 -->
    <div class="search-bar">
      <input type="text" class="search-input" placeholder="搜索商品">
    </div>

    <!-- 排序和筛选栏 -->
    <div class="filter-bar">
      <div class="sort-tab active" data-sort="default">综合</div>
      <div class="sort-tab" data-sort="price">价格</div>
      <div class="sort-tab" data-sort="sales">销量</div>
      <div class="filter-btn" onclick="openFilter()">筛选</div>
    </div>

    <!-- 商品列表 -->
    <div class="product-list">
      <div class="product-card">
        <div class="product-image">商品图片</div>
        <div class="product-info">
          <div class="product-title">无线蓝牙耳机 降噪运动耳机 超长续航</div>
          <div class="product-price">¥299</div>
          <div class="product-sales">月销 1234</div>
        </div>
      </div>

      <div class="product-card">
        <div class="product-image">商品图片</div>
        <div class="product-info">
          <div class="product-title">智能手表 心率监测 运动计步</div>
          <div class="product-price">¥599</div>
          <div class="product-sales">月销 567</div>
        </div>
      </div>

      <div class="product-card">
        <div class="product-image">商品图片</div>
        <div class="product-info">
          <div class="product-title">便携式充电宝 20000mAh 快充</div>
          <div class="product-price">¥199</div>
          <div class="product-sales">月销 2345</div>
        </div>
      </div>

      <div class="product-card">
        <div class="product-image">商品图片</div>
        <div class="product-info">
          <div class="product-title">机械键盘 青轴 RGB背光 游戏键盘</div>
          <div class="product-price">¥399</div>
          <div class="product-sales">月销 890</div>
        </div>
      </div>
    </div>
  </div>

  <!-- 筛选弹窗 -->
  <div class="overlay" id="filter-overlay" onclick="closeFilter()"></div>
  <div class="bottom-sheet" id="filter-sheet">
    <div class="sheet-header">
      <div class="sheet-title">筛选</div>
      <div class="sheet-close" onclick="closeFilter()">×</div>
    </div>

    <div class="sheet-content">
      <div class="filter-section">
        <div class="filter-label">分类</div>
        <div class="filter-options">
          <div class="filter-option" onclick="toggleFilter(this)">耳机</div>
          <div class="filter-option" onclick="toggleFilter(this)">手表</div>
          <div class="filter-option" onclick="toggleFilter(this)">充电宝</div>
          <div class="filter-option" onclick="toggleFilter(this)">键盘</div>
        </div>
      </div>

      <div class="filter-section">
        <div class="filter-label">价格区间</div>
        <div class="price-range">
          <input type="number" class="price-input" placeholder="最低价">
          <span>-</span>
          <input type="number" class="price-input" placeholder="最高价">
        </div>
      </div>

      <div class="filter-section">
        <div class="filter-label">品牌</div>
        <div class="filter-options">
          <div class="filter-option" onclick="toggleFilter(this)">品牌 A</div>
          <div class="filter-option" onclick="toggleFilter(this)">品牌 B</div>
          <div class="filter-option" onclick="toggleFilter(this)">品牌 C</div>
          <div class="filter-option" onclick="toggleFilter(this)">品牌 D</div>
        </div>
      </div>
    </div>

    <div class="sheet-footer">
      <button class="btn-reset" onclick="resetFilter()">重置</button>
      <button class="btn-apply" onclick="applyFilter()">确定</button>
    </div>
  </div>

  <!-- Toast -->
  <div class="toast" id="toast">已应用筛选条件</div>

  <script>
    // 排序 Tab 切换
    const sortTabs = document.querySelectorAll('.sort-tab');
    sortTabs.forEach(tab => {
      tab.addEventListener('click', () => {
        sortTabs.forEach(t => t.classList.remove('active'));
        tab.classList.add('active');
      });
    });

    // 筛选弹窗
    function openFilter() {
      document.getElementById('filter-overlay').classList.add('active');
      document.getElementById('filter-sheet').classList.add('active');
    }

    function closeFilter() {
      document.getElementById('filter-overlay').classList.remove('active');
      document.getElementById('filter-sheet').classList.remove('active');
    }

    // 筛选选项切换
    function toggleFilter(element) {
      element.classList.toggle('selected');
    }

    // 重置筛选
    function resetFilter() {
      document.querySelectorAll('.filter-option').forEach(opt => {
        opt.classList.remove('selected');
      });
      document.querySelectorAll('.price-input').forEach(input => {
        input.value = '';
      });
    }

    // 应用筛选
    function applyFilter() {
      closeFilter();
      showToast('已应用筛选条件');
    }

    // Toast
    function showToast(message) {
      const toast = document.getElementById('toast');
      toast.textContent = message;
      toast.classList.add('active');
      setTimeout(() => {
        toast.classList.remove('active');
      }, 2000);
    }
  </script>
</body>
</html>
```

## 原型说明文档

```markdown
# 商品列表筛选功能 - 原型说明

## 原型文件
- 文件名：product-list-filter.html
- 平台：移动端
- 页面数量：1 个主页面 + 1 个弹窗

## 页面清单
| 页面 | 说明 |
|---|---|
| 商品列表页 | 展示商品列表，支持排序和筛选 |
| 筛选弹窗 | 底部弹出，支持分类、价格、品牌筛选 |

## 交互说明

### 排序功能
- 位置：商品列表上方
- 排序项：综合、价格、销量
- 交互：点击切换排序方式，当前选中项高亮
- 默认：综合排序

### 筛选弹窗
- 触发：点击"筛选"按钮
- 弹出方式：从底部滑出，高度 80vh
- 关闭方式：
  - 点击遮罩层
  - 点击右上角 ×
  - 点击"确定"或"重置"按钮

### 筛选条件
**分类筛选：**
- 类型：多选
- 选项：耳机、手表、充电宝、键盘
- 选中态：背景变灰，边框加深

**价格区间：**
- 类型：输入框
- 字段：最低价、最高价
- 校验：只能输入数字，最低价 ≤ 最高价

**品牌筛选：**
- 类型：多选
- 选项：品牌 A、品牌 B、品牌 C、品牌 D

### 操作按钮
**重置：**
- 功能：清空所有筛选条件
- 效果：取消所有选中，清空价格输入

**确定：**
- 功能：应用筛选条件
- 效果：关闭弹窗，列表刷新为筛选结果，Toast 提示"已应用筛选条件"

## 业务规则
- 排序和筛选可叠加使用
- 筛选后列表实时更新
- 筛选条件在页面停留期间保持
- 离开页面后筛选条件清空

## 边界情况
- 筛选结果为空：展示空状态，提示"未找到符合条件的商品"
- 价格输入非法：自动过滤非数字字符
- 最低价 > 最高价：提交时提示"最低价不能大于最高价"
- 商品标题超长：最多显示 2 行，超出省略号

## 待确认问题
- [ ] 是否需要保存用户上次的筛选条件？
- [ ] 筛选条件是否需要 URL 化，支持分享？
- [ ] 品牌列表是否从后端动态获取？
- [ ] 是否需要支持更多排序方式（如新品优先）？
```

## 交付物

1. **HTML 原型文件**：`product-list-filter.html`
   - 可直接在浏览器打开
   - 所有交互可操作
   - 黑白灰风格

2. **配套说明文档**：`product-list-filter-spec.md`
   - 页面清单
   - 交互说明
   - 业务规则
   - 边界情况
   - 待确认问题
