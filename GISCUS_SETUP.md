# Giscus 评论系统配置指南

## 第一步：启用 GitHub Discussions

1. 访问你的 GitHub 仓库：https://github.com/laiziki486/laiziki486.github.io
2. 点击 **Settings**（设置）
3. 向下滚动找到 **Features**（功能）部分
4. 勾选 **Discussions**（讨论）

## 第二步：获取 Giscus 配置信息

1. 访问 https://giscus.app
2. 在 **语言** 下拉框中选择 **简体中文**
3. 在 **仓库** 输入框中填入：`laiziki486/laiziki486.github.io`
4. 等待验证通过（会显示绿色勾号）
5. 在 **Discussion 分类** 中选择 **Announcements**（公告）
6. 在 **特性** 部分：
   - 勾选 **启用主评论区的反应**
   - 输入位置选择 **评论框在评论上方**
7. 向下滚动到 **启用 giscus** 部分，复制以下信息：
   - `data-repo-id`（仓库 ID）
   - `data-category-id`（分类 ID）

## 第三步：更新配置文件

打开 `_config.yml` 文件，找到 `comments` 部分，将获取到的信息填入：

```yaml
comments:
  provider: giscus
  giscus:
    repo: laiziki486/laiziki486.github.io
    repo_id: 你的仓库ID  # 从 giscus.app 复制
    category: Announcements
    category_id: 你的分类ID  # 从 giscus.app 复制
    mapping: pathname
    strict: 0
    input_position: top
    lang: zh-CN
    reactions_enabled: 1
```

## 第四步：提交并推送

```bash
git add _config.yml
git commit -m "启用 Giscus 评论系统"
git push
```

等待 GitHub Pages 部署完成后，评论功能就会出现在每篇文章底部。

## 注意事项

- Giscus 使用 GitHub Discussions 存储评论，完全免费
- 访客需要登录 GitHub 账号才能评论
- 评论数据存储在你的 GitHub 仓库中，完全由你控制
- 支持 Markdown 格式和代码高亮
