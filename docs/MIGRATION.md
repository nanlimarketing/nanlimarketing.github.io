# al-folio v1 迁移记录

日期：2026-10-16（Asia/Shanghai）。

## 来源与备份

- 原网站目录：`../nanlimarketing.github.io`，原分支 `master`。
- 原网站提交：`e731133`。
- 原网站中未提交的 `_pages/about.md` 已一起保存到迁移副本的 `32f53a4`。
- 完整迁移前备份标签：`backup/pre-al-folio-v1`。
- 迁移分支：`migrate/al-folio-v1`。
- 官方 starter 提交：`ba1c8104a2f39c76ebc2fa9da1b02c66b75ea855`。
- 核心插件：`al_folio_core 1.0.15`；其余插件沿用官方 `Gemfile` 和锁文件中的精确版本。

遵循官方 [al-folio v1 migration skill](../.agents/skills/al-folio-v1-migration/SKILL.md)。
迁移在独立副本中进行，原目录及其中未提交的修改保持原样。

## 内容保留与格式调整

- 保留姓名、中文姓名、职位、个人简介、研究兴趣、办公室地址和头像。
- 两份 BibTeX 论文库完整保留，包括论文标题、摘要、链接、录用状态、获奖备注和排序。
- 保留 Research 中三个分组及全部 work-in-progress 内容。
- 保留 Teaching 中全部课程和学期。
- 保留 `assets/json/resume.json` 的全部有效数据，以及 `assets/pdf/CV.pdf`；省略空字段，避免新版 CV 显示空白章节和联系项目。
- 保留全部原有文章、项目、新闻及附件；未引入新版 starter 的额外示例页面、书籍和课程。
- 保留网站验证文件，以及根域名和原有页面地址。
- Google Scholar、ORCID、ResearchGate 和邮箱迁入 `_data/socials.yml`。
- 修正 `_data/coauthors.yml` 中 Li 和 Zhao 两个条目的 URL 缩进。
- 首页 `profile.address` 改为 `profile.more_info`；新闻和最新文章改为 v1 的配置对象并保持关闭。
- 首页底部社交区保持隐藏，顶部社交图标保持启用。
- CV 明确选择 `cv_format: jsonresume`；下载路径保持 `/assets/pdf/CV.pdf`。
- 站点使用 JSONResume 和现有 PDF，故移除不适用的 RenderCV 自动生成工作流及其示例配置、示例 PDF。
- 删除未被使用的旧式 CV YAML；该文件含示例人物数据，完整原件仍可从备份标签获取。
- 原有 Google Analytics 配置处于关闭状态，迁移后仍关闭；measurement ID 保留在配置注释中。
- 停用模板附带的外部博客源和演示评论配置，避免将模板作者的内容或评论入口带入个人站点。

## 主题文件与兼容性

旧 `_layouts`、`_includes`、`_sass`、`_plugins` 和旧主题 CSS/JS 不再复制到站点中。
默认实现来自官方插件。当前迁移不保留本地模板覆盖，因此无需接受覆盖文件。

原有文章和项目中的 `.html` include 改为对应的 `.liquid` include。
旧 Mermaid 标签改为新版支持的 Mermaid 代码块。
表格示例的 `datatable` 标志改为新版 `pretty_table`。
Twitter 示例改用浏览器端嵌入并保留原链接，避免网站构建依赖 Twitter API 的可访问性。
Plotly 地图示例使用无需 token 的 OpenStreetMap 底图，替换失效的 Stamen Terrain 配置，保留原图数据。
使用 v1 默认的 Tailwind 样式，`al_folio.compat.bootstrap.enabled: false`。
旧文章和项目使用的基本栅格类已由核心样式提供；表格示例改用 v1 内置表格引擎。
浏览器检查发现 Bootstrap 兼容插件会隐藏桌面导航，因此未启用该过渡插件。
表格示例的 `data-toggle="table"` 是内置表格引擎支持的输入，升级审计仍会将其列为非阻塞提示。

网页 CV 使用新版插件的统一排版，布局可能与旧版不同。个人数据保持不变。
此次迁移不修改原文中的拼写、课程名称、论文描述或联系信息。

## 本地构建环境

本机通过现有 Ubuntu 24.04 WSL、Ruby 3.2.3 和 Bundler 4.0.6 进行验证。
GitHub Actions 沿用官方 Ruby 3.3.5 构建配置。
Ruby 开发头文件、ImageMagick 和 Jupyter nbconvert 已安装到 WSL。
本地依赖、验证脚本、日志和预览截图位于被忽略的 `.runtime/`；不进入网站发布内容。

## 回退和发布

当前迁移在独立分支中，尚未推送或部署。继续使用原目录即可保留现有网站状态。

如需查看迁移前的完整内容，可在迁移副本中创建另一分支：

```sh
git switch -c review/pre-v1 backup/pre-al-folio-v1
```

确认迁移效果后，将 `migrate/al-folio-v1` 推送到自己的 `origin` 并通过 PR 合入 `master`。
部署工作流仅在推送到 `master` 或 `main` 后发布到 `gh-pages`，PR 只构建。
不要推送到 `alshedivat/al-folio`。

## 验证结果

- 内容保留检查：41 项通过。非空个人简历数据、两份论文库、个人附件和原网站工作区状态均已核对。
- 生成站点检查：6 项通过。四个个人页面的本地链接有效，六篇论文完整渲染，构建工具未进入发布目录。
- 官方升级审计：0 个阻塞问题、4 个非阻塞提示，全部来自旧表格示例中内置表格引擎支持的 `data-toggle="table"`。
- 覆盖审计：未发现本地文件覆盖官方插件。
- 完整 Jekyll 生产构建通过，Notebook 转换成功。
- 使用官方 `purgecss.config.js` 处理 9 个生成的样式文件后，浏览器检查仍通过。
- 浏览器检查：23 项通过，无本地引用资源失败、无页面脚本错误。涵盖桌面页面、四个个人页面的手机布局、手机导航、主题切换、论文摘要、CV 下载及站点验证文件。
- 表格示例的 JSON 数据加载、CV 空白摘要不显示，均已额外检查。
- Prettier 格式检查与 starter 样式边界检查通过。

运行的主要命令：

```sh
bundle install
bundle exec al-folio upgrade audit --no-fail
bundle exec al-folio upgrade overrides audit --fail-on-stale
JEKYLL_ENV=production bundle exec jekyll build --trace
npm run lint:style-contract
npm run lint:prettier
```

本机 Windows 上的 PurgeCSS CLI 存在绝对路径解析问题，因此使用相同版本的
PurgeCSS API 和相同配置验证了部署处理。GitHub Actions 在 Linux 上仍使用官方 CLI。
GitHub Actions 尚未执行，因为迁移分支尚未推送。

本机预览地址：<http://127.0.0.1:8089/>。截图与详细检查结果保存在 `.runtime/`。
