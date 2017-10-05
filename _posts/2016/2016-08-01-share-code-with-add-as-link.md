---
layout: post
title:  "使用链接共享 Visual Studio 中的代码文件"
date:   2016-08-01 11:04:42 +0800
categories: visualstudio
permalink: /visualstudio/2016/08/01/share-code-with-add-as-link.html
---

如果你还在通过复制来共享代码就太 out 了！  
我们知道，将公共的代码抽取成库是非常好的代码复用手段，但有时我们需要复用的代码（或文件）无法提取到库中。这时，Visual Studio 自带的 Link 功能就派上用场了。

---

## 链接的文件长这样

![链接文件](/assets/2016-08-01-share-file-as-link.png)

如上图，链接文件其图标上会有一个小图标![图标](/assets/2016-08-01-link-icon.png)。图中的 A 和 B 两个项目都是应用程序项目（不是类库），所需的图标和数据库是需要共享的，于是将其设为了链接。

## 怎么创建链接文件？

#### 最快的方法：

按住 Alt 键，在 Visual Studio 中将一个文件拖拽到另一个文件夹中即可完成文件的链接。  
如果你想同时拖拽多个文件，你需要先拖拽，再在拖拽的过程中按住 Alt 键，否则只能拖一个文件出来（不知 Visual Studio 2015 为什么要这么设计）。

#### 普通的方法：

在 Visual Studio 项目上或文件夹上 `右键→Add→ExistingItem`，选好文件后，不要直接点右下角的 `[Add]` 按钮，而是点击 `[Add]` 按钮旁边的下拉按钮，选择 `Add As Link`。

## 这个过程发生了什么？

我们观察下包含链接的项目文件：

```xml
...
<!-- 这是普通的文件 -->
<None Include="Log.config"/>
...
<!-- 这是链接的文件 -->
<None Include="..\Project B\icon.ico">
    <Link>icon.ico</Link>
</None>
...
```

可以发现，Include 实际上是指向另一个项目中的文件，这样，msbuild 在编译的时候直接读取 include 的文件路径即可实现编译，都不需要特殊考虑。  
在标签内部，有一个 `Link` 标签，告诉 Visual Studio 在此项目中应该显示到哪个文件夹下，是什么文件名。

---

#### 参考资料
[MSDN - Share code with Add as Link](https://msdn.microsoft.com/en-us/library/windows/apps/jj714082(v=vs.105).aspx)