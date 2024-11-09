# 使用说明
拉去项目后,需要先安装依赖,才可以启动项目
npm install

## 1. 安装Git和NodeJS  

- 在Windows上使用Git，可以从Git官网直接 https://git-scm.com/downloads，然后按默认选项安 装即可。安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就 说明Git安装成功！
- 在Git中绑定Github账号，打开“Git Bash”，在命令框中依次输入两行命令：

```sh
git config --global user.name “Your Name”        
git config --global user.email email@example.com
 # 其中Your Name和email@example.com替换成上面注册时的账户名和邮箱
```

- 由于 Hexo 是基于 Node.js 驱动的一款博客框架，所以安装NodeJS并配置环境变量。 (略),linux可以使用`NVM`管理工具
- 安装之后可以输入以下命令查看是否安装成功：

```sh
git version
node -v
npm -v
```

![image-20241108003151592](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080031637.png)

> npm换国内源:
>
> - 淘宝源官网:https://npmmirror.com/
>
> ![image-20241108003239851](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080032958.png)
>
> - 换源
>
> ```sh
> npm config set registry https://registry.npmmirror.com
> ```
>
> 查看是否成功:
>
> ```sh
> Administrator@DESKTOP-FBLS07J MINGW64 /d/Desktop
> $ npm get registry
> https://registry.npmmirror.com/
> ```

## 2. 安装Hexo

- 以上环境准备好了就可使用 npm 开始安装 Hexo 了，在命令行输入执行如下命令：

  ```sh
  npm install -g hexo-cli
  ```

- 安装 Hexo 完成后，在指定文件夹下打开“Git Bash”，再执行下列命令，Hexo 将会在指定文件夹中 新建所须要的文件：

  ```sh
  hexo init myBlog
  cd myBlog
  npm install
  ```

  若是上面的命令都没报错的话，就恭喜了，运行 hexo s 命令，其中 s 是 server 的缩写，在浏览 器中输入  http://localhost:4000  回车就能够预览效果了。

  ![image-20241108003555818](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080035876.png)

  

## 3. 加载主题

### 主题官网
https://hexo.io/themes/

### 所选主题:
https://github.com/blinkfox/hexo-theme-matery

### 官方文档
主页文档有说明,

修改参考:

https://blog.csdn.net/One_OR_Five/article/details/138545419?spm=1001.2014.3001.5506

https://blog.csdn.net/weixin_41387351/article/details/129994966?spm=1001.2014.3001.5506

https://blog.csdn.net/qq_41376237/article/details/113475727?spm=1001.2014.3001.5506

https://blog.csdn.net/houqinqin/article/details/136111867?spm=1001.2014.3001.5506

## 4. 项目的使用

> 从github拉取下来后,确定本地已经配置好hexo环境

**步骤:**

:one:拉取主项目:

```sh
git clone git@github.com:wylblog/xxxx.git
```

:two:更新模块:

```sh
1.进入到拉去的项目
cd xxxx
2.下载所需模块
npm install
```

![image-20241108004155627](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080041687.png)

:three:启动项目:

```sh
hexo server
或
hexo s
```

![image-20241108004253178](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080042247.png)

:four:查看web端

> 例如:

 http://localhost:4000/

![image-20241108004342429](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202411080043615.png)

## 5. hexo常用指令

### 5.1 基础命令:star:

1. **初始化项目**（仅在新建项目时使用）

   ```sh
   hexo init <folder>
   ```

2. **安装依赖**
   在项目根目录运行此命令，安装 `package.json` 中定义的所有依赖：

   ```bash
   npm install
   ```

3. **启动本地服务器**
   用于在本地查看网站，默认地址为 `http://localhost:4000`：

   ```bash
   hexo server
   或
   hexo s
   ```

   可以使用 `--debug` 查看详细调试信息：

   ```sh
   hexo server --debug
   ```

4. **生成静态文件**
   编译生成网站静态文件到 `public` 目录，便于本地预览或上传至服务器：

   ```sh
   hexo generate
   或
   hexo g
   ```

5. **清理缓存**
   清理 Hexo 缓存文件和生成的静态文件，解决可能的生成问题：

   ```sh
   hexo clean
   ```

6. **部署到远程仓库**
   部署生成的静态文件到指定的远程仓库（如 GitHub Pages）。需要在 `_config.yml` 中正确配置 `deploy` 部分。

   ```sh
   hexo deploy
   ```

   若遇到权限问题，请确保使用 SSH 密钥或配置正确的 GitHub 访问令牌。

### 5.2 内容管理命令:star:

1. **新建文章**
   在 `source/_posts` 目录下生成新的 Markdown 文件。

   ```sh
   hexo new post "Post Title"
   ```

2. **新建页面**
   生成新的页面文件（如 `about`、`contact` 等）：

   ```sh
   hexo new page "Page Name"
   ```

### 5.3 一键命令

1. **清理并生成**
   先清理缓存文件，再生成静态文件：

   ```sh
   hexo clean && hexo generate
   ```

2. **清理、生成并部署**
   执行完整流程，适合每次更新博客时使用：

   ```bash
   hexo clean && hexo generate && hexo deploy
   ```
