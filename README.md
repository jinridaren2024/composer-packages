# Composer Packages Repository

这是一个基于 [Composer Satis](https://github.com/composer/satis) 的私有包索引仓库，
用于发布 `jinridaren-org` 组织下的 Composer 包。

## Repository URL

在业务项目的 `composer.json` 中添加：

```json
{
  "repositories": [
    {
      "type": "composer",
      "url": "https://jinridaren-org.github.io/composer-packages/"
    }
  ]
}
```

然后安装包，例如：

```bash
composer require jinridaren-group/money-calc
```

## Build & Deploy

本仓库通过 GitHub Actions 执行 Satis 构建并发布到 GitHub Pages：

- Workflow: `.github/workflows/deploy.yml`
- Output directory: `public/`
- Pages URL: `https://jinridaren-org.github.io/composer-packages/`

## Security

- 不要将 GitHub Token 写入可提交文件（如 `satis.json`、`.env`、`auth.json`）。
- 令牌仅通过 CI 环境变量或 Composer 全局认证注入。

## 如何新增私有包

以新增 `jinridaren-org/foo-bar` 为例：

1. 创建包仓库（如 `foo-bar`），并确保仓库内 `composer.json` 至少包含：

```json
{
  "name": "jinridaren-org/foo-bar", 
  "type": "library",
  "autoload": {
    "psr-4": {
      "Jinridaren\\FooBar\\": "src/"
    }
  }
}
```

2. 提交代码并打版本标签（建议语义化版本）：

```bash
git tag v1.0.0
git push origin v1.0.0
```

3. 在本仓库 `satis.json` 的 `repositories` 中添加该包仓库：

```json
{
  "type": "vcs",
  "url": "https://github.com/jinridaren-org/foo-bar.git"
}
```

4. 提交并推送本仓库，触发 `.github/workflows/deploy.yml` 构建发布。

5. 发布完成后验证索引（可选）：

```bash
curl -I https://jinridaren-org.github.io/composer-packages/packages.json
```

6. 在业务项目安装：

```bash
composer clear-cache
composer require jinridaren-org/foo-bar:^1.0
```

### 常见问题

- `Could not find a matching version`：通常是未打 tag，或包名与仓库 `composer.json` 的 `name` 不一致。
- `packages.json 404`：通常是 Pages 未成功发布，或仓库 URL（`org/corp`）配置不一致。

