# SynE Lab 网站维护指南

> 本指南面向实验室成员，用于日常更新网站内容。

---

## 快速操作速查

| 要做什么 | 修改哪个文件/目录 | 命令 |
|---------|------------------|------|
| 添加新闻 | `content/news/YYYY-MM-DD-标题.md` | 直接创建文件 |
| 添加组员 | `content/people/姓名.md` | 直接创建文件 |
| 添加论文 | `data/publications.yaml` | 编辑 YAML |
| 修改研究方向 | `content/research/_index.md` | 直接编辑 |
| 修改联系方式 | `hugo.toml` | 编辑配置 |
| 发布更新 | 推送 main 分支 | `git push` |

---

## 一、添加新闻

### 1.1 创建新闻文件

在 `content/news/` 目录下创建新文件，文件名格式：`YYYY-MM-DD-简短标题.md`

示例：`content/news/2026-07-10-new-paper-nature.md`

文件内容：
```yaml
---
title: Research | 新论文发表在 Nature Chemistry
date: 2026-07-10
tag: research        # 可选: research（科研）或 activities（活动）
---

新闻正文内容，支持 Markdown 格式。

- 可以写多段
- 支持 [链接](https://example.com)
- 支持 **加粗**、*斜体*
```

### 1.2 提交并发布

```bash
git add content/news/2026-07-10-new-paper-nature.md
git commit -m "Add news: 新论文发表在 Nature Chemistry"
git push origin main
```

推送后，GitHub Actions 会自动构建并部署（约 1-2 分钟）。

---

## 二、添加组员

### 2.1 创建组员文件

在 `content/people/` 目录下创建新文件，文件名格式：`姓名拼音.md`

示例：`content/news/zhang-san.md`

```yaml
---
title: 张三                # 显示姓名
role: phd                 # 角色: pi/postdoc/phd/master/undergraduate/visitor
photo: /images/people/zhang-san.jpg   # 头像路径
role_label: 博士研究生     # 显示的角色名称
date: 2025-09-01          # 入组日期
---

个人简介（可选）。
```

### 2.2 准备头像

将头像图片放入 `static/images/people/` 目录，文件名与 `photo` 字段一致。

图片要求：
- 格式：JPG 或 PNG
- 尺寸：建议 400x400 像素以上
- 比例：正方形（1:1）

### 2.3 提交并发布

```bash
git add content/people/zhang-san.md static/images/people/zhang-san.jpg
git commit -m "Add member: 张三"
git push origin main
```

---

## 三、添加论文

### 3.1 编辑论文数据

打开 `data/publications.yaml`，在 `since_2021` 列表**顶部**添加新论文：

```yaml
since_2021:
  - number: 41              # 新编号，递增
    title: "论文完整标题"
    authors: "作者A, **张三**, **纪清清***, 作者D"
    journal: "Nature Chemistry"
    year: "2026"
    doi: "10.1038/s41557-026-xxxxx"    # 可选
    highlight: "被 Nature 亮点报道"      # 可选
```

格式说明：
- `number`：递增编号
- `authors`：作者列表；**加粗**表示 SynE 组员；`*` 表示通讯作者；`#` 表示共同一作
- `doi`：有 DOI 时自动添加链接
- `highlight`：如有特殊报道，会高亮显示

### 3.2 提交并发布

```bash
git add data/publications.yaml
git commit -m "Add publication: Nature Chemistry 2026"
git push origin main
```

---

## 四、修改研究方向

编辑文件：`content/research/_index.md`

```markdown
---
title: 研究方向
---

## 新的研究方向标题

研究内容描述...

1. 研究兴趣一
2. 研究兴趣二
```

如需更换研究方向图片，替换 `static/images/research-1.png` 和 `research-2.png`。

---

## 五、修改联系方式

编辑文件：`hugo.toml`

找到 `[params]` 部分，修改以下字段：

```toml
[params]
  email = "jiqq@shanghaitech.edu.cn"
  phone = "(021)-20684747"
  address = "上海市浦东新区华夏中路393号..."
```

---

## 六、添加招聘/招生信息

编辑文件：`content/positions/_index.md`

```markdown
---
title: 加入我们
---

### 博士后招聘

招聘详情...

### 研究生招生

招生详情...

有意者请将简历发送至：**jiqq@shanghaitech.edu.cn**
```

---

## 七、完整更新流程

```bash
# 1. 进入项目目录
cd synelab-hugo

# 2. 拉取最新代码（多人协作时）
git pull origin main

# 3. 修改文件（用任意编辑器）
# 例如：添加新闻、修改论文列表等

# 4. 本地预览（可选，修改较多时建议先预览）
hugo server -D
# 浏览器访问 http://localhost:1313/

# 5. 提交修改
git add .
git commit -m "更新内容描述"

# 6. 推送发布
git push origin main

# 7. 等待自动部署（约 1-2 分钟）
# 访问 https://synelab.xyz 查看效果
```

---

## 八、检查部署状态

1. 打开 `https://github.com/imrascal/synelab-hugo/actions`
2. 最新的 workflow 记录：
   - 绿色勾 = 部署成功
   - 红色叉 = 构建失败，点击查看错误日志

---

## 九、常见问题

### Q1: 推送后网站没有更新？

- 检查 GitHub Actions 是否成功（绿色勾）
- 等待 1-2 分钟（CDN 缓存刷新）
- 强制刷新浏览器：`Ctrl + F5`

### Q2: 图片上传后不显示？

- 确认图片路径与 front matter 中的 `photo` 字段一致
- 确认图片已 `git add` 并提交
- 图片文件名区分大小写

### Q3: 新闻没有出现在首页？

- 确认 `date` 字段格式正确：`YYYY-MM-DD`
- 首页只显示最新的 5 条新闻

### Q4: 论文列表不更新？

- 确认 `data/publications.yaml` 语法正确（可用在线 YAML 校验工具检查）
- 编号不要重复

### Q5: 多人同时修改冲突？

```bash
git pull origin main        # 先拉取最新代码
git add .
git commit -m "更新内容"
git push origin main
```

如有冲突，手动编辑冲突文件后重新提交。

---

## 十、文件位置速查

```
synelab-hugo/
├── content/
│   ├── _index.md              # 首页文字
│   ├── research/_index.md     # 研究方向
│   ├── people/                # 组员（每人一个 .md 文件）
│   ├── news/                  # 新闻（每条一个 .md 文件）
│   ├── publications/_index.md # 论文页面说明
│   ├── positions/_index.md    # 招聘信息
│   └── contact/_index.md      # 联系页面
├── data/
│   └── publications.yaml      # 论文数据
├── static/images/             # 图片资源
│   ├── logo.png               # Lab Logo
│   ├── pi.jpg                 # PI 头像
│   ├── hero.jpg               # 首页大图
│   ├── research-1.png         # 研究方向图1
│   ├── research-2.png         # 研究方向图2
│   └── people/                # 组员头像
└── hugo.toml                  # 站点配置（联系方式等）
```

---

## 维护联系人

如有技术问题，请联系网站管理员。
