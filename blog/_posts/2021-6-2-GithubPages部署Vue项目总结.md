---
date: 2021-6-2
tag:
  - Vue
  - GithubPages
author: liningjiang
location: Chengdu
---

# 使用 Github Pages 部署 Vue 项目总结

## 1、准备一个自己的域名

- GitHub 默认的免费域名强制开启 https

- 在 https 协议中无法发出 http 请求

- 我们项目中使用的接口都是 http 协议的，所以需要准备一个自己的域名，因为自定义域名可以选择使用 http 协议或者 https 协议

- 或者你让接口开发者为接口服务提供 https 的支持

## 2、在域名管理后台添加 `CNAME` 记录

- 主机就是域名前面的前缀名字
  - 比如：toutiao.lining-jiang.xyz
- 指向必须指向你的 github 地址，
  - 比如：yumengqaq.github.io
- TTP 默认一小时

## 3、在项目的 `public` 目录中添加 `CNAME` 文件

- ```
  toutiao.lining-jiang.xyz
  ```

## 4、生成 GitHub 访问令牌

- 点击用户 settings
- 左侧有一个 Developer settings
- 点击 Personal access tokens，再点击 Generate new token
- 勾选 repo、workflow
- 点击创建成功（备注：token 仅显示 1 次，之后无法查看，所以建议把 token 保存到你的私密位置。）

## 5、创建远程仓库（如果创建就不需要了）

## 6、将 GitHub 访问令牌添加到远程仓库的 secrets 中

- ```
  Name：ACCESS_TOKEN
  Value: 之前生成的 GitHub 访问令牌
  ```

## 7、在项目根目录中添加 `.github/workflows/main.yml`

- ```
  name: build and deploy

  # 当 master 分支 push 代码的时候触发 workflow
  on:
    push:
      branches:
      - master

  jobs:
    build-deploy:
      runs-on: ubuntu-latest
      steps:
      # 下载仓库代码
      - uses: actions/checkout@v2

      # 缓存依赖
      - name: Get yarn cache
        id: yarn-cache
        run: echo "::set-output name=dir::$(yarn cache dir)"
      - uses: actions/cache@v1
        with:
          path: ${{ steps.yarn-cache.outputs.dir }}
          key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
          restore-keys: |
            ${{ runner.os }}-yarn-

      # 安装依赖
      - run: yarn

      # 打包构建
      - run: yarn build

      # 发布到 GitHub Pages
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v2
        env:
          PERSONAL_TOKEN: ${{ secrets.ACCESS_TOKEN }} # 访问秘钥
          PUBLISH_BRANCH: gh-pages # 推送分支
          PUBLISH_DIR: ./dist # 部署目录
  ```

## 8、把项目代码推送到远程仓库

## 9、查看构建部署状态（它需要执行一个构建部署的流程，没那么快）

最后就是如何更新项目网站？

```
很简单，修改源代码，把更新提交到远程仓库即可。
说白了你可以忽略网站部署这件事儿了。
```

然后你可以通过仓库中的 Action 查看构建部署状态（非必须）。
