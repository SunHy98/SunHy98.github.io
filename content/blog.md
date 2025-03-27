---
title: "使用 Hugo + Github 搭建个人博客"
---

# Hugo 安装

## MacOS 安装

在 MacOS 上，推荐使用包管理器 Homebrew 进行安装

```shell
brew install hugo
```

![image-20250327111651137](https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327111651137.png)

查看安装是否成功

```shell
hugo version
```

![image-20250327111812698](https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327111812698.png)

## Windows 和 Linux 安装

在 Windows 和 Linux 上，推荐使用预编译二进制文件安装Hugo。

下载地址：https://github.com/gohugoio/hugo/tags

> 1. 根据操作系统和架构下载对应的版本
> 2. 下载后，解压缩二进制文件到目标目录
> 3. 添加目标目录到环境变量 PATH 中
> 4. 验证文件的执行权限

# 新建本地项目

## 使用 hugo 命令生成项目

```shell
hugo new site Blog
```



执行上面的命令，会在当前所在目录下，生成一个名为 Blog 的项目

![image-20250327113024063](https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327113024063.png)

## 项目文件说明



> 1. archetypes：存放内容模板文件，支持通过`hugo new` 命令生成预设格式的Markdown文档。优先级：项目目录模板 > 主题模板‌。
>
> 2. assets：存储需要预处理的静态资源（SCSS/JS），Hugo Pipes会编译后输出到`resources/`‌。
>
> 3. content：存放所有Markdown格式的内容文件，子目录层级自动映射为网站URL结构。支持通过Front Matter设置内容类型‌。
>
> 4. data：存放结构化数据文件（JSON/XML），可通过`.Site.Data`调用‌。
>
> 5. i18n：多语言配置文件，支持国际化字符串映射‌。
>
> 6. layouts：包含HTML模板文件，控制页面渲染逻辑：
>
>    ​		├─ `_default/`（基础模板）
>    ​		├─ `partials/`（可复用组件）‌
>    ​		└─ 主题模板继承机制：项目模板 > 主题模板‌
>
> 7. static：直接复制到发布目录的原始文件（如图片/PDF），不经过编译处理‌。
>
> 8. themes：主题存放目录，可通过`git submodule`或直接克隆方式安装‌。
>
> 9. resources：自动生成的编译缓存文件，包含优化后的CSS/JS和图片资源‌。
>
> 10. config：多环境配置文件目录（`config.toml/` 、`config/production.toml`），支持TOML/YAML格式‌。
>
> 11. public：默认生成路径，存放编译后的完整静态网站文件‌。
>
> 12. hugo.toml：Hugo 静态网站生成器的核心配置文件，包括定义网站元数据等功能。

# 主题配置

## 主题下载

Hugo 官网有许多主题可供选择，官网主题地址：https://themes.gohugo.io/

下面以`blowfish`主题为例进行演示，blowfish主题地址：https://themes.gohugo.io/themes/blowfish/

> 主题的安装有以下几种方式，具体说明参见主题使用文档：https://blowfish.page/docs/installation/#install-using-hugo
>
> - 方式一：可以直接在主题地址页面进行下载，然后将下载的主题文件解压后，移动到 hugo 项目的`themes/`目录下。
> - 方式二：使用 git 安装主题。
> - 方式三：使用 hugo 安装。



下面以 git 安装主题的方式进行演示：

1. 进入到 hugo项目的根目录下，初始化本地库

```shell
git init
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327120300321.png" alt="image-20250327120300321" style="zoom:50%;" />

2. 添加 Blowfish 作为子模块

```shell
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327120708234.png" alt="image-20250327120708234" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327120802636.png" alt="image-20250327120802636" style="zoom:50%;" />

## 设置主题

在 hugo 项目的根目录下，删除`hugo.toml`，然后将主题文件下的`/config/_default`中所有的`*.toml`文件复制到 hugo 项目的`config/_default/`文件夹中。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327121600285.png" alt="image-20250327121600285" style="zoom:50%;" />

复制上图中主题文件夹下的文件到你的hugo项目中。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327121847025.png" alt="image-20250327121847025" style="zoom:50%;" />

> 上面是使用 git 来安装 Blowfish，所以需要修改`hugo.toml`文件，添加`theme = "blowfish"`到文件顶部。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327122914359.png" alt="image-20250327122914359" style="zoom:50%;" />

## 主题更新

因为上面是使用 git 来安装主题的，所以只需执行以下命令，主题的最新版本就会下载到您的本地存储库中

```git
git submodule update --remote --merge
```

子模块更新后，再重新构建项目即可。

# 项目配置

> 下面对部分配置进行说明，具体配置参见主题的使用文档：https://blowfish.page/zh-cn/docs/configuration

## 语言设置

>如果要配置多语言，则需要为网站上的每种语言创建一个语言配置文件。
>
>默认情况下，Blowfish 包含了英语的配置文件`config/_default/languages.en.toml`。
>
>如果想要设置中文和英语两种语言，则按以下步骤操作即可。



1. 创建文件`config/_default/languages.zh-cn.toml`

文件内容可以复制`languages.en.toml`，然后修改即可。下面展示部分修改的配置。



```toml
disabled = false
languageCode = "zh-cn" # 语言代码。
languageName = "Chinese" # 语言名称。
weight = 1 # 权重决定了在构建多语言时的语言顺序。
title = "sun_blog" # 网站的标题。它将在网站头部和底部进行展示。

[params]
  displayName = "简体中文" # 语言在网站中的展示名。
  isoCode = "zh-CN" # 用于 HTML 元数据的 ISO 语言代码。
  rtl = false # 用于指定是否是 RTL 语言。设置为 true 则网站会从右向左重排内容。
  dateFormat = "2006年01月02日" # 日期格式。
```



2. 设置默认语言为中文

修改`hugo.toml`，设置默认语言为中文

```toml
defaultContentLanguage = "zh-cn"
```

# 项目部署

## 常规部署

### 创建Github仓库



<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327135653682.png" alt="image-20250327135653682" style="zoom:50%;" />

### 修改BaseUrl

修改 hugo 项目中的 `hugo.toml`，设置baseURL



```toml
baseURL = "https://SunHy98.github.io"
```



### 项目打包

运行 `hugo -D` 进行项目构建，会在项目的根目录下生成public文件夹，这就是最终的目标文件。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327140420274.png" alt="image-20250327140420274" style="zoom:50%;" />

### 项目推送到Github

进入public文件夹下，按照以下命令，操作即可。

1. 初始化本地库

```git
git init
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327140722849.png" alt="image-20250327140722849" style="zoom:50%;" />

2. 提交本地工作区文件到本地库

```git
git add .
git commit -m "提交说明"
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327140946758.png" alt="image-20250327140946758" style="zoom:50%;" />

3. 修改本地分支名称

> 在Git中，默认的主分支名称是`master`，但在最近的版本控制实践中，许多项目开始使用`main`作为默认的主分支名称。可以使用下面的命令，将本地的master分支更名为main分支。



```git
git branch -M main
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327141411764.png" alt="image-20250327141411764" style="zoom:50%;" />



4. 关联远程仓库

```git
git remote add origin git@github.com:SunHy98/SunHy98.github.io.git
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327141542889.png" alt="image-20250327141542889" style="zoom:50%;" />

5. 推送本地项目到远程仓库

```git
git push -u origin main
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327141709090.png" alt="image-20250327141709090" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327141810615.png" alt="image-20250327141810615" style="zoom:50%;" />

### 访问博客网站

1. 进入仓库的页面，选择`Settings` 

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327141930860.png" alt="image-20250327141930860" style="zoom:50%;" />

2. 选择Pages，可以在这里看到站点部署的网址

   部署可能需要等待一段时间，才能看到该网址。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327142320653.png" alt="image-20250327142320653" style="zoom:50%;" />

3. 访问博客

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327143940813.png" alt="image-20250327143940813" style="zoom:50%;" />

## 自动部署



> GitHub 允许你使用 Actions 在 GitHub Pages 上托管静态网站。如果想要启用此功能，需要你在代码库中启用 Pages 并创建一个新的 Actions 工作流，以此来构建和部署你的网站。
>
> 部署流程参见主题说明文档：https://blowfish.page/zh-cn/docs/hosting-deployment/#github-pages

### 创建工作流配置文件

在 Hugo 项目的根目录下创建文件`.github/workflows/gh-pages.yml`

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327145755505.png" alt="image-20250327145755505" style="zoom:50%;" />

```yml
# .github/workflows/gh-pages.yml

name: Blog

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./public
```

在上面的配置文件中，需要填写`github_token`

1. 生成token

   Setttings -> Developer Settings -> Personal access tokens -> Tokens（classic）

   <img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327151411044.png" alt="image-20250327151411044" style="zoom:50%;" />

2. 设置token

   可以直接将token填写到上面的配置文件，但为了安全，可以作为环境变量，配置在GitHub上

   进入仓库页面 -> Setttings -> Secrets and variables -> Actions -> new repository secret

   <img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327152633730.png" alt="image-20250327152633730" style="zoom:50%;" />

3. 将上面的secret Name设置到工作流文件中

   ```yml
   jobs:
     build-deploy:
       runs-on: ubuntu-20.04
       concurrency:
         group: ${{ github.workflow }}-${{ github.ref }}
       steps:
         - name: Checkout
           uses: actions/checkout@v3
           with:
             submodules: true
             fetch-depth: 0
   
         - name: Setup Hugo
           uses: peaceiris/actions-hugo@v2
           with:
             hugo-version: "latest"
   
         - name: Build
           run: hugo --minify
   
         - name: Deploy
           uses: peaceiris/actions-gh-pages@v3
           if: ${{ github.ref == 'refs/heads/main' }}
           with:
             github_token: ${{ secrets.deploy_token }}
             publish_branch: gh-pages
             publish_dir: ./public
   ```

### 创建.gitignore

由于自动部署，是交给GitHub去完成，所以不需要上传public、resources等构建的输出目录和文件。

```gitignore
public
resources
.hugo_build.lock
```

### 推送到远程库

进入到 hugo 项目的根目录，初始化本地库

之前在下载主题的时候，已经初始化过了，这里就可以跳过了

```git
git init
```

1. 提交本地工作区文件到本地库

```git
git add .
git commit -m "提交说明"
```



3. 修改本地分支名称

> 在Git中，默认的主分支名称是`master`，但在最近的版本控制实践中，许多项目开始使用`main`作为默认的主分支名称。可以使用下面的命令，将本地的master分支更名为main分支。



```git
git branch -M main
```

4. 关联远程仓库

```git
git remote add origin git@github.com:SunHy98/SunHy98.github.io.git
```

5. 推送本地项目到远程仓库

> 注意：由于之前使用了常规部署方式，导致当前远程库的版本已经和本地不一致。可以尝试新建一个GitHub仓库进行提交，也可以使用强制推送命令。如果没有进行过常规部署，就不会有问题。

现在的 Hugo 项目中，有两个 .git 文件，可以先把public里面的 .git 删除

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327164810418.png" alt="image-20250327164810418" style="zoom:50%;" />

然后尝试将本地库的提交推送到远程仓库中



```git
git push -u origin main
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327164945158.png" alt="image-20250327164945158" style="zoom:50%;" />

由于之前的常规部署已经对远程库进行了修改，所以这里无法直接推送。下面使用强制推送。

```git
git push -f -u origin main
```

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327165147231.png" alt="image-20250327165147231" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327165915142.png" alt="image-20250327165915142" style="zoom:50%;" />

将工作流配置文件推送到GitHub，GitHub会自动运行工作流。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327165958970.png" alt="image-20250327165958970" style="zoom:50%;" />

我们需要在代码仓库的 Settings > Pages ，将分支改为 gh-pages 。

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327170052736.png" alt="image-20250327170052736" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327170148154.png" alt="image-20250327170148154" style="zoom:50%;" />

### 测试自动部署

我们将这篇博客放到 hugo 项目的 content 目录中

<img src="https://cdn.jsdelivr.net/gh/SunHy98/image_site/image/image-20250327172451118.png" alt="image-20250327172451118" style="zoom:50%;" />

然后将文件提交到远程库

