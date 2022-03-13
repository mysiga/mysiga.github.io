# mysiga.github.io使用
[Hexo部署博客](https://hexo.io/zh-cn/docs/)
### 配置环境
- 安装node

```
brew install node
```
- 安装 Hexo

```
npm install -g hexo-cli
```
- 安装 hexo-deployer-git

```
npm install hexo-deployer-git --save
```

#### 文章发布
- 编写好的markown文件放到source/_posts下面
- 生成静态网页(需要跳转到hexo初始化文件目录下)

```
hexo generate
```
- 启动本地服务器

```
hexo server
```
- 发布部署

```
hexo deploy
```

#### 碰到问题

- [DTrace 错误 （Mac OS X）](https://hexo.io/zh-cn/docs/troubleshooting#DTrace-%E9%94%99%E8%AF%AF-%EF%BC%88Mac-OS-X%EF%BC%89)

```
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```
DTrace 安装可能有错误 , 使用下列命令:

```
 npm install hexo --no-optional
```