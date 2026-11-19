# 主页维护指南

本网站使用 Jekyll + GitHub Pages 构建，英文页在 `/`，中文页在 `/zh/`。
日常维护只需要修改 `_data/` 下的几个数据文件，**不需要写 HTML**。

修改后在本地预览，确认无误再提交、推送。GitHub Pages 会自动重新构建网站。

---

## 1. 文件都管什么

| 文件 | 作用 |
| --- | --- |
| `_data/profile.yml` | 姓名、身份、学校、About Me 简介、当前研究课题 |
| `_data/research.yml` | 三条研究方向（标题、简介、关键词） |
| `_data/projects.yml` | 代表性科研项目 |
| `_data/milestones.yml` | 科研阶段成果（里程碑） |
| `_data/publications.yml` | 学术论文 |
| `_data/education.yml` | 教育经历 |
| `_data/i18n.yml` | 页面栏目名、按钮文字（一般不用改） |
| `_config.yml` | 邮箱、GitHub、头像、社交链接、网站域名等 |
| `index.md` | 英文首页（只有 SEO 标题和描述） |
| `zh/index.md` | 中文首页（只有 SEO 标题和描述） |
| `_layouts/` `_includes/` `_sass/` | 页面模板和样式（一般不用改） |

**写作原则：每条数据同时包含英文 `en` 和中文 `zh`，两处一起改，避免信息不同步。**

---

## 2. 新增一个科研项目

打开 `_data/projects.yml`，在文件末尾追加一条记录（注意缩进，用空格，不要用 Tab）：

```yaml
- id: my-new-project          # 英文小写短横线，页面锚点用，不能重复
  status: ongoing             # ongoing / completed / exploratory
  period: "2026 – 2027"       # 可选，没有就留 ""
  title:
    en: "Project title in English"
    zh: "项目中文标题"
  summary:
    en: "One or two sentences about the research problem and scope."
    zh: "用一两句话说明研究问题与范围。"
  highlights:                 # 可选，列表
    en:
      - "First research point"
    zh:
      - "第一个研究要点"
  baseline: ""                # 可选：技术基线，如 ORB-SLAM3
  application:                # 可选：目标应用
    en: ""
    zh: ""
  challenges:                 # 可选：重点关注的困难场景
    en: []
    zh: []
  stages: []                  # 可选：已确认的阶段成果，见 2.2
  image: ""                   # 可选：/assets/img/xxx.png 或外部图片 URL
  code_url: ""                # 可选：GitHub 仓库
  paper_url: ""               # 可选：论文链接
  project_url: ""             # 可选：项目主页
  video_url: ""               # 可选：演示视频
```

- 留空的字段会自动隐藏，不会出现空按钮或失效链接。
- 不要写未经确认的精度、性能提升比例或正式成果。
- 项目状态显示文字在 `_data/i18n.yml` 的 `status` 下，可自行修改。

### 2.1 修改项目状态

把 `status:` 改成：

- `ongoing` → 在研 / Ongoing Research
- `completed` → 已完成 / Completed
- `exploratory` → 探索性实验 / Exploratory Study

### 2.2 记录项目阶段成果（stages）

阶段成果的原始记录存放在独立科研仓库 `robust-visual-localization` 的
`outcomes/approved/`。只有 `review.status: approved` 且 `publish.homepage: true`
的记录才允许同步到主页。

同步方式：

1. 在科研仓库运行 `python3 scripts/export_homepage_stages.py --project robust-visual-localization`；
2. 打开生成的 `build/homepage_stages_robust-visual-localization.yml`；
3. 整段复制到本项目 `_data/projects.yml` 中对应项目的 `stages:` 位置（替换 `stages: []`）；
4. 在主页仓库本地构建并预览，确认无误后再提交。

**不要直接在本文件写入未审核结果。** 如果只是临时记录，请留在科研仓库的
`outcomes/draft/` 中。

只有当结果真实完成并经确认后，才在对应项目的 `stages` 中添加记录。
日常代码提交、调试过程不算科研成果。

可用类别（`category`）：

| 值 | 含义 |
| --- | --- |
| `baseline_evaluation` | 基线评估 |
| `failure_case_analysis` | 失败案例分析 |
| `frontend_improvements` | 前端改进 |
| `backend_relocalization` | 后端与重定位改进 |
| `robustness_experiments` | 鲁棒性实验 |
| `reproducible_release` | 可复现研究发布 |

示例（把项目中的 `stages: []` 替换为列表）：

```yaml
  stages:
    - category: baseline_evaluation
      date: "2026-06"         # 必须真实，建议加引号
      title:
        en: "Baseline evaluation in texture-poor corridors"
        zh: "弱纹理走廊场景下的基线评估"
      summary:
        en: "Short factual description of what was evaluated."
        zh: "客观说明评估内容，不写未验证的结论。"
      scene:                  # 实验场景
        en: "Indoor corridor with texture-poor walls"
        zh: "弱纹理墙面的室内走廊"
      algorithm_version: ""   # 算法版本
      config: ""              # 实验配置
      baselines: []           # 对比方法，如 ["ORB-SLAM3"]
      metrics: []             # 评估指标，如 ["ATE RMSE", "tracking success rate"]
      figure: ""              # 结果图表：/assets/img/xxx.png 或外部 URL
      video_url: ""           # 实验视频
      repo_url: ""            # GitHub 仓库（确认后再填真实链接）
      code_version: ""        # 代码版本 / commit
      release_url: ""         # Release 链接
      cv_description:         # 可用于简历的描述
        en: ""
        zh: ""
```

- 没有数据的字段保持 `""` 或 `[]`，页面会自动隐藏。
- 不编造实验数据、改进幅度、成功率或论文成果。
- 未经验证的内容不要标记为 Completed / Validated。

---

## 3. 新增一条科研里程碑

打开 `_data/milestones.yml`，删除文件末尾的 `[]`，按下面格式添加：

```yaml
- date: "2026-06"             # 必须真实，至少精确到年月，建议加引号
  title:
    en: "Paper accepted at ICRA 2027"
    zh: "论文被 ICRA 2027 接收"
  description:
    en: "A short factual description of the milestone."
    zh: "客观描述这一阶段性成果。"
  url: ""                     # 可选：相关链接
```

里程碑只记录真实、可确认的成果（实验阶段完成、代码开源、论文投稿/录用等）。
**没有内容时保持 `[]`，主页会自动隐藏整个模块。**

---

## 4. 新增一篇论文

打开 `_data/publications.yml`，删除文件末尾的 `[]`，按下面格式添加：

```yaml
- title: "Paper title"
  authors: "A. Author, <strong>Yu Cong</strong>, B. Author"
  venue: "IEEE International Conference on Robotics and Automation (ICRA)"
  year: "2027"
  pdf: ""
  arxiv: ""
  project_url: ""
  code_url: ""
```

- 作者里可以用 `<strong>Yu Cong</strong>` 把自己的名字加粗。
- 没有论文时保持 `[]`，Publications 模块会自动隐藏。

---

## 5. 更新中英文简介

打开 `_data/profile.yml`：

```yaml
name:
  en: Yu Cong
  zh: 于聪

position:
  en: Master's Student in Control Engineering
  zh: 控制工程专业硕士研究生

affiliation:
  en: Soochow University, China
  zh: 苏州大学

about:
  en:
    - "第一段英文简介。"
    - "第二段英文简介。"
  zh:
    - "第一段中文简介。"
    - "第二段中文简介。"

current_topic:
  en: "Current research topic in English"
  zh: "当前研究课题中文名称"
```

- `about` 是段落列表，每个 `-` 是一段，可以增加或删除。
- 修改 `_data/research.yml` 可以调整三条研究方向和关键词。
- 修改 `_data/education.yml` 可补充教育经历；`period` 留空则不显示时间。

---

## 6. 配置头像

当前网站**不显示头像**（`avatar: ""`），这是有意的：不上传未经确认的照片。

如果以后想显示头像，打开 `_config.yml`，二选一：

**方式 A：使用外部图片 URL（推荐，不把照片放进仓库）**

```yaml
avatar: "https://example.com/your-photo.jpg"
```

**方式 B：使用仓库内图片**

1. 把自己的图片放到 `assets/img/avatar.png`（建议正方形、大小 200–400 KB）。
2. 设置：

```yaml
avatar: "/assets/img/avatar.png"
```

> 隐私提示：头像一旦公开显示，任何访问网站的人都可以下载或转发。
> 不想公开照片时，保持 `avatar: ""` 即可，布局会自动适应。

---

## 7. 修改邮箱和社交链接

打开 `_config.yml`：

```yaml
email: yc1683654@gmail.com
github_link: https://github.com/YuCong419

# 留空字符串即隐藏对应图标
google_scholar: ""
orcid: ""
cv_link: ""
linkedin: ""
twitter: ""
```

- `cv_link` 可以填外部链接，或把自己的 PDF 放到 `assets/files/` 后填 `/assets/files/cv.pdf`。
- Google Scholar、ORCID、LinkedIn 等确认后再填写，不留占位假链接。

---

## 8. 本地预览

需要先安装 Ruby（3.1 及以上）和 Bundler，然后在项目目录执行：

```bash
bundle install
bundle exec jekyll serve
```

浏览器打开 <http://localhost:4000> 查看英文页，<http://localhost:4000/zh/> 查看中文页。

> 如果提示缺少 `webrick`，执行 `bundle add webrick` 后再启动。
> 修改 `_data/` 或 `_config.yml` 后需要重新启动预览服务。

---

## 9. 提交和发布

确认本地预览无误后：

```bash
git status
git diff
git add -A
git commit -m "Update research projects"
git push
```

推送后 GitHub Pages 会自动部署（通常 1–2 分钟）。
线上地址：<https://yucong419.github.io/> 和 <https://yucong419.github.io/zh/>。

---

## 10. 常见问题

**模块不显示？**
`milestones`、`publications` 等数据文件为空时会自动隐藏，这是正常行为。

**中文页面样式或图片路径错误？**
所有资源路径都通过 Jekyll 的 `relative_url` 生成，不要手写 `./assets/...`。

**YAML 报错？**
检查缩进（只用空格）、冒号后是否有空格、包含 `:` 的文本是否加了引号。

**是否需要改 `_layouts/` 或 `_sass/`？**
通常不需要。新增内容优先改 `_data/`；确需调整版式时再改模板和样式。

---

## 11. 内容真实性约定

- 不编造论文、奖项、专利、项目成果或实验数据。
- 未确认的经历和链接保持留空或隐藏。
- 研究兴趣可以写，但不能暗示已经取得成果。
- 导师姓名、本科信息、具体时间等未确认内容暂不展示。
