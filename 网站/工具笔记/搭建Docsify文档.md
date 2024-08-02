Docsify
一个神奇的文档站点生成器。
简单轻巧（~21kB）
没有生成静态的html文件
主题丰富

## 安装

### 本地搭建

如果你想在本地搭建：
npm安装：

```bash
Copynpm i docsify-cli -g
```

初始化：

```bash
Copydocsify init ./docs
```

本地预览：

```bash
Copydocsify serve docs
```

进入`http://localhost:3000`就能看到效果咯！

### 托管在网上
新建一个`index.html`内容为：

```html
Copy<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta charset="UTF-8">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/vue.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      //...
    }
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
</body>
</html>
```

### CDN的选择

CDN可以选择：

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/docsify/4.11.2/docsify.min.js"></script>
<script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
```

这样就可以看到一个最基本的网页啦！

## 如何写文章

只需用Markdown语法写好一个`.md`的文章放在根目录或子目录后就会自动识别了。
我自己测试好像用html的也可以，直接把后缀名改成`.md`，但效果可能不太好。
文章链接对应：

```text
Copy/README.md  =>  domain.com/#/
/hello.md  =>  domain.com/#/hello
/hello/hi.md  =>  domain.com/#/hello/hi
```

## 个性化

### 自定义加载文字

只需在`index.html`中新增：

```html
Copy<div id="app">Please wait...</div>
```

### 自定义侧边栏

只需在`index.html`中新增：

```html
Copy<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
```

后创建一个文件叫做`_sidebar.md`，将你的文件输入进去：

```scss
Copy* [Home](/)
* [Guide](guide.md)
```

`_sidebar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。
例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_sidebar.md`获取文件，如果不存在则从`/_sidebar.md`获取。

> 注意，如果是托管在网上，请在文件根目录新增名叫`.nojekyll`的空文件。

为了更好地SEO，您可以在每个文件后面自定义标题：

```markdown
Copy* [Home](/)
* [Guide](guide.md "The greatest guide in the world")
```

默认情况下会自动根据文章标题生成目录，如果不想要，可以再`index.html`中新增：

```html
Copy<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2
  }
</script>
```

`subMaxLevel: 2`表示只显示`h1~h2`的标题，对应`#`和`##`。
如果你想忽略某个标题，则可以在文章中新增`{docsify-ignore}`：

```markdown
Copy# Getting Started
## Header {docsify-ignore}
```

如果想忽略全部的标题，则可以新增`{docsify-ignore-all}`：

```markdown
Copy# Getting Started {docsify-ignore-all}
## Header
```

表示忽略`{docsify-ignore-all}`下的全部标题

> `{docsify-ignore-all}`和`{docsify-ignore}`在正文中都不会显示

### 自定义导航栏

写法一：
在`index.html`中新增：

```html
Copy<body>
  <nav>
    <a href="#/">EN</a>
    <a href="#/zh-cn/">中文</a>
  </nav>
  <div id="app"></div>
</body>
```

> 所有路径都必须用`#/`来书写

写法二：
在根目录新增`_navbar.md`文件：
写法同`_sidebar.md`

```markdown
Copy* [En](/)
* [chinese](/zh-cn/)
```

你也可以按照如下来写多级导航栏：

```markdown
Copy* Getting started
  * [Quick start](quickstart.md)
  * [Writing more pages](more-pages.md)
  * [Custom navbar](custom-navbar.md)
  * [Cover page](cover.md)

* Configuration
  * [Configuration](configuration.md)
  * [Themes](themes.md)
  * [Using plugins](plugins.md)
  * [Markdown configuration](markdown.md)
  * [Language highlight](language-highlight.md)
```

`_navbar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。
例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_navbar.md`获取文件，如果不存在则从`_navbar.md`获取。

### 封面

设置：

```html
Copywindow.$docsify = {
    coverpage: true,
}
```

后再根目录创建`_coverpage.md`
输入内容就可以显示在封面了
效果见https://xhemj.gitee.io/books/

### 主题颜色

设置：

```html
Copywindow.$docsify = {
    themeColor: '#c30aff',
}
```

`#c30aff`就是主题的颜色了

### 外链打开方式

设置：

```html
Copywindow.$docsify = {
	externalLinkTarget: '_blank',
}
```

`_blank`表示在新标签页中打开

## 插件

### 表情插件

先在在`index.html`中新增：

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
```

> 我自己测试是我上面推荐的三个CDN都可以使用

即可输入
`:100:` => 💯

### 搜索插件

新增`index.html`：

```html
Copy<script>
  window.$docsify = {
    search: 'auto', 

    search : [
      '/',            // => /README.md
      '/guide',       // => /guide.md
      '/get-started', // => /get-started.md
      '/zh-cn/',      // => /zh-cn/README.md
    ],

    
    search: {
      maxAge: 86400000, 
      paths: [], 
      placeholder: 'Type to search',

      
      placeholder: {
        '/zh-cn/': '搜索',
        '/': 'Type to search'
      },

      noData: 'No Results!',


      noData: {
        '/zh-cn/': '找不到结果',
        '/': 'No Results'
      },

      depth: 2,

      hideOtherSidebarContent: false,


      namespace: 'website-1',
    }
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
```

那我们来解释一下：
1.指定搜索路径
`search: 'auto',`表示是否搜索，默认是`auto`
或：

```html
Copysearch : [
      '/',            // => /README.md
      '/guide',       // => /guide.md
      '/get-started', // => /get-started.md
      '/zh-cn/',      // => /zh-cn/README.md
    ],
```

如：`/`就表示`README.md`
`guide`表示`/guide.md`
设置后就表示只搜索这几个目录
2.`maxAge: 86400000,`到期时间（官方这么说），不用改动
3.`paths: [],`可以设置搜索的目录，或设置`auto`或`/`，貌似和`search:[]`一样？
4.搜索框的提示
`placeholder: 'Type to search',`
或：

```html
Copyplaceholder: {
        '/zh-cn/': '搜索',
        '/': 'Type to search'
      },
```

这样可以设置中英文目录的搜索框的提示
`noData: 'No Results!',`表示无结果时显示的文字
或：

```html
CopynoData: {
        '/zh-cn/': '找不到结果',
        '/': 'No Results'
      },
```

分别设置中英文
5.标题深度
`depth: 2,`（官方这么解释）可以设置为`1-6`
6.隐藏其他侧边栏内容
`hideOtherSidebarContent: false,`（官方这么解释）默认为`false`
7.避免搜索索引冲突
`namespace: 'website-1',`可以自己设置
同一域下的多个网站之间可以设置名称
避免搜索索引冲突
其实有很多参数都不用设置
比如我的配置是：

```html
Copysearch: 'auto',
search: {
maxAge: 86400000,
paths: '/',
placeholder: '搜索...',
noData: '未找到结果，换个搜索词试试？',
namespace: 'XhemjBlog',
	},
```

### Google Analytics

就是谷歌统计
直接新增：

```html
Copy<script>
  window.$docsify = {
    ga: 'UA-XXXXX-Y'
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/ga.min.js"></script>
```

`ga: 'UA-XXXXX-Y'`就是谷歌统计的编号
或：

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js" data-ga="UA-XXXXX-Y"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/ga.min.js"></script>
ga: 'UA-XXXXX-Y'`=`data-ga="UA-XXXXX-Y"
```

### 外链脚本插件

如果你需要在`.md`文件中引用如：

```html
Copy<script src="https://domain.com/xxx.js" ></script>
```

安装：

```xml
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/external-script.min.js"></script>
```

> 照这样看来是可以在`.md`中写`.html`的……

### 图片缩放插件

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
```

效果就是点击一下图片可以放大
如：
[![logo](https://docsify.js.org/_media/icon.svg)](https://docsify.js.org/_media/icon.svg)
如果不想缩放可以设置：

```markdown
Copy![](image.png ":no-zoom")
```

### 复制代码插件

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify-copy-code"></script>
```

效果可以自己看上面的所有代码

### Disqus评论插件

```html
Copy<script>
  window.$docsify = {
    disqus: 'shortname'
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/disqus.min.js"></script>
```

详见：https://disqus.com/

### Gitalk评论插件

```html
Copy<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/gitalk/dist/gitalk.css">

<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/gitalk.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/gitalk/dist/gitalk.min.js"></script>
<script>
  var gitalk = new Gitalk({
    clientID: 'Github Application Client ID',
    clientSecret: 'Github Application Client Secret',
    repo: 'Github repo',
    owner: 'Github repo owner',
    admin: ['Github repo collaborators, only these guys can initialize github issues'],
    // facebook-like distraction free mode
    distractionFreeMode: false
  })
</script>
```

可以使文章实现评论效果，教程详见：https://github.com/gitalk/gitalk/

### 链接下一篇文章插件
效果见[https://xhemj.gitee.io/books/#/p/how-to-use-Docsify.md](https://xhemj.gitee.io/books/#/p/how-to-use-Docsify)

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify-pagination/dist/docsify-pagination.min.js"></script>
```

也可以自定义：

```html
Copywindow.$docsify = {
pagination: {
            previousText: '上一篇',
            nextText: '下一篇',
            crossChapter: true,
            crossChapterText: true,
        },
	}
```

更多插件可以见https://docsify.js.org/#/awesome?id=plugins

> 以下是我自己使用的插件

### Social Share分享插件

经过测试，无法直接在`index.html`中嵌入代码
需要先安装上面的外链脚本插件

```html
Copy<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/external-script.min.js"></script>
```

后在`.md`文件中写下：

```markdown
Copy<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<div class="social-share"></div>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>
```

即可在文件中嵌入分享插件
详细自定义教程可见：https://github.com/overtrue/share.js

### 嵌入Markdown文件插件

```html
Copy<script src="https://unpkg.com/docsify-remote-markdown/dist/docsify-remote-markdown.min.js">
```

可以自定义：

```html
Copywindow.$docsify = {
remoteMarkdown: {
    tag: 'md',
		},
	}
```

使用方法：

```markdown
Copy[你设置的tag，如：md](https://domain.com/markdown.md)
```

效果如上方的分享插件就可以直接链接：
而不用写分享代码

```html
Copy<script src="https://unpkg.com/docsify-footer-enh/dist/docsify-footer-enh.min.js"></script>
```

自定义：

```html
Copywindow.$docsify = {
footer: {
            copy: '',
            auth: '',
            pre: '<hr>',
            style:'text-align: center;'
        },
	}
```

实测`copy`和`auth`可以随便写
写什么文字代码都可以
`pre`是正文和Footer的分割线，默认`<hr>`
效果可以见[https://xhemj.gitee.io/books/#/p/How-to-Use-Docsify.md](https://xhemj.gitee.io/books/#/p/How-to-Use-Docsify)