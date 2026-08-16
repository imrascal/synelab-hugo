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

## 〇、分支清理约定（重要）

> 仓库通过 AI 编程助手（如 Trae）执行代码修改任务时会创建临时分支。临时分支不应长期残留在 GitHub 上。

**规则：**
1. 每次代码修改任务会基于 `main` 创建临时分支（如 `trae/agent-*`）。
2. 功能完成后，**必须及时清理**已部署/已合并进 `main` 的临时分支。
3. 清理范围：GitHub 线上分支 + 本地分支 + 本地远程跟踪引用，全部一并删除。
4. 删除前先确认功能已进入 `main`（避免误删丢失内容），`main` 本身永不删除。

**清理命令（`gh` 已认证为仓库所有者时）：**
```bash
# 删除远程分支（逐个）
gh api -X DELETE "repos/imrascal/synelab-hugo/git/refs/heads/<分支名>"

# 删除本地分支
git branch -D <分支名>

# 清理本地过期的远程跟踪引用
git update-ref -d "refs/remotes/origin/<分支名>"
```

**注意事项：**
- 与分支关联的 PR 一旦创建无法删除，只会保留"关闭"记录，属正常现象。
- 若分支 tip 非 `main` 祖先但功能内容已存在于 `main`，说明改动是直接提交进 main 而非通过 PR 合并，判定是否已部署应对比分支与 `main` 的实际内容，而非只看合并状态。

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

---

## 十一、X-MOL 课题组页面 → synelab 同步指南（月度维护）

> **适用场景**：每月定期将 X-MOL 课题组网站的最新论文、新闻同步到 synelab.xyz。
> **核心教训**：仅靠"标题匹配"容易漏掉论文状态变更（如 Accepted→已发表、补充 DOI/URL 等）。必须结合 CrossRef 查询做二次校验。

### 11.1 数据源与访问方式

| 数据源 | URL | 访问方式 | 说明 |
|--------|-----|----------|------|
| 新闻列表 | `https://www.x-mol.com/groups/SynE/news` | **WebFetch** 可用 | 返回纯文本新闻列表（标题、日期、链接 ID） |
| 新闻详情 | `https://www.x-mol.com/groups/SynE/news/{id}` | **WebFetch** 可用 | 返回单条新闻的正文内容 |
| 论文列表 | `https://www.x-mol.com/groups/SynE/publications` | **WebFetch** 可用 | 返回纯文本论文列表，但**无 DOI/URL** |
| CrossRef API | `https://api.crossref.org/works/{DOI}` | **curl** 可用 | 权威的论文元数据查询，获取 DOI、卷期页码、发表日期 |

### 11.2 同步流程（论文）

#### Step 1：抓取 X-MOL 论文列表

```bash
# 使用 WebFetch 抓取，输出为纯文本格式
# 关键信息：编号、标题、作者、期刊、状态（Submitted/In revision/Accepted/已发表）
```

**注意**：X-MOL 论文列表只显示期刊名和状态，**不显示 DOI 和 URL**。必须通过 CrossRef 补充。

#### Step 2：识别新增论文 + 状态变更

```python
# 对比 data/publications.yaml，识别两类变更：
# A) 新增论文（X-MOL 有、YAML 没有）
# B) 状态变更（如 "Accepted" → 已发表，或补充了 DOI）

# 新增论文：添加到 since_2021 列表顶部，编号递增
# 状态变更：就地修改对应条目的 status/journal 字段
```

#### Step 3：CrossRef 查询补充 DOI/URL

**这是最关键的一步**——X-MOL 的论文经常显示 "Accepted" 但没有 DOI，需通过 CrossRef 查询最新状态：

```bash
# 查询单篇论文的 CrossRef 信息（通过标题搜索）
curl -s "https://api.crossref.org/works?query.title=论文标题&rows=3" | python3 -c "
import json, sys
data = json.load(sys.stdin)
for item in data['message']['items']:
    print(f\"Title: {item.get('title',[''])[0]}\")
    print(f\"DOI: {item.get('DOI','')}\")
    print(f\"Journal: {item.get('container-title',[''])[0]}\")
    published = item.get('published-print', item.get('published-online', {}))
    print(f\"Date: {published}\")
    print(f\"Type: {item.get('type','')}\")
    print('---')
"
```

**CrossRef 查询策略**：
1. **精确标题匹配**：用 `query.title` 参数搜索，取第一个匹配项
2. **验证匹配度**：对比标题相似度（建议 >90% 才算匹配）
3. **提取元数据**：DOI、期刊名、发表日期、卷期页码
4. **判断发表状态**：
   - `type: "journal-article"` + 有 `published-print` 日期 → 已发表
   - 有 DOI 但无发表日期 → Early Access / Accepted
   - 无 DOI → Submitted / In revision

#### Step 4：更新 data/publications.yaml

```yaml
# 新增条目模板
  - number: 42
    title: "论文标题"
    authors: "作者列表（保留 X-MOL 格式）"
    journal: "期刊名 2026"
    year: 2026
    doi: "10.xxxx/xxxxx"          # CrossRef 获取
    url: "https://doi.org/10.xxxx/xxxxx"  # CrossRef 获取

# 状态变更（例如 Accepted → 已发表，补充 DOI/URL）
  - number: 34
    title: "..."
    journal: "J. Am. Chem. Soc. 2026"  # 去掉 ", Accepted"
    year: 2026
    doi: "10.1021/jacs.6c12708"        # 新增
    url: "https://doi.org/10.1021/jacs.6c12708"  # 新增
```

### 11.3 同步流程（新闻）

#### Step 1：抓取新闻列表

```bash
# WebFetch 抓取 https://www.x-mol.com/groups/SynE/news
# 输出格式：
# - [• Research | 新闻标题](/groups/SynE/news/{id}) 2026-06-18
```

#### Step 2：对比现有新闻文件

```bash
# 列出 content/news/ 目录下所有 .md 文件
# 提取每条新闻的 source URL 中的 ID：
grep -h "Source: x-mol.com" content/news/*.md | grep -oP 'news/\K[0-9]+' | sort -n

# 对比 X-MOL 列表中的 ID，找出新增 ID
```

#### Step 3：抓取新闻详情

```bash
# 对每个新增 ID，用 WebFetch 抓取详情页
# 提取：标题、正文、发布日期、配图 URL
```

#### Step 4：创建新闻文件

在 `content/news/` 和 `content/en/news/` 下各创建一个文件：

```yaml
# content/news/short-slug.md（中文版）
---
title: "新闻标题（中文）"
date: 2026-08-16
tag: research        # research 或 activities
---

正文内容（中文翻译，参考现有新闻风格）

![配图描述](/images/news/short-slug.png)

*[Source: x-mol.com](https://www.x-mol.com/groups/SynE/news/{id})*
```

### 11.4 提交与推送

```bash
git add data/publications.yaml content/news/新文件.md content/en/news/新文件.md
git commit -m "content: sync x-mol updates YYYY-MM-DD"
git push origin main
```

### 11.5 部署验证

```bash
# 检查 GitHub Actions 状态
curl -s -H "Authorization: token ${GH_TOKEN}" \
  "https://api.github.com/repos/imrascal/synelab-hugo/actions/runs?per_page=1" \
  | python3 -c "
import json, sys
r = json.load(sys.stdin)['workflow_runs'][0]
print(f\"Status: {r['status']} / {r.get('conclusion', 'N/A')}\")
print(f\"URL: {r['html_url']}\")
"

# 或直接访问 https://github.com/imrascal/synelab-hugo/actions
```

### 11.6 常见坑点与对策

| 坑点 | 对策 |
|------|------|
| **curl/WebFetch 触发阿里云滑块验证码** | X-MOL 主页和新闻页有时会拦截。使用 WebFetch 即可访问，若被拦截改用浏览器 MCP |
| **论文列表无 DOI** | 必须通过 CrossRef API 补充。不要假设"Accepted"状态不需要 DOI——很多已分配 DOI 但状态仍为 Accepted |
| **论文状态误判** | 用 CrossRef 的 `type` 字段判断：`journal-article` = 已发表；有 DOI 无 type = Early Access |
| **新闻 source URL 错位** | 创建新闻文件时严格核对 X-MOL 新闻 ID，避免 A 文指向 B 文的 URL |
| **论文编号错乱** | 新增论文的编号必须基于 YAML 中最大编号递增，不能跳号或重复 |
| **中英文新闻不同步** | 新增新闻必须同时在 `content/news/` 和 `content/en/news/` 下创建对应文件 |

### 11.7 完整检查清单

每次同步完成前，逐项确认：

- [ ] X-MOL 论文列表与 YAML 逐一对比（新增 + 状态变更）
- [ ] 所有论文的 DOI/URL 已通过 CrossRef 验证
- [ ] X-MOL 新闻列表与 `content/news/` 逐一对比
- [ ] 新增新闻的中英文版本均已创建
- [ ] 新闻 source URL 指向正确的 X-MOL 新闻 ID
- [ ] `data/publications.yaml` 注释中的论文总数已更新
- [ ] `git diff` 审核所有变更内容
- [ ] GitHub Actions 构建成功
