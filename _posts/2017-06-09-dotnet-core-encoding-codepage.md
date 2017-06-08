---
categories: blog
tags: 
    -name: dotnetCore
    -name: Encoding
title: .net Core 中使用中文字符集
date: 2017-06-09 02:30:33 +0800
---
### .net Core 中使用中文字符集
#### 缘由
在 .net Core 中，默认只支持 `ASCII`、`UTF8`、`UTF16` 和 `UTF7`。此外的一切字符集都需要一个叫做 `System.Text.Encoding` 的包才能支持其他语言的字符集。

#### 步骤

**1、引用包**

打开项目文件，在 `project` 标记内加入以下内容：
{% highlight xml linenos %}
<ItemGroup>
    <PackageReference Include="System.Text.Encoding" Version="4.3.0" />
    <PackageReference Include="System.Text.Encoding.CodePages" Version="4.3.0" />
    <PackageReference Include="System.Text.Encoding.Extensions" Version="4.3.0" />
</ItemGroup>
{% endhighlight %}

**2、注册代码页提供程序**

在程序运行之初加入以下代码：
{% highlight csharp linenos %}
Encoding.RegisterProvider(CodePagesEncodingProvider.Instance);
{% endhighlight %}

好了， 现在可以像以前在 .net Framework 中那样通过 `Encoding` 类获取需要的字符集了。
