---
title: "利用chakraUI快速开发landing page"
date: 2023-10-16T09:31:28+08:00
draft: true
categories:
 - 网站开发
tags:
 - chakra

toc: true
---

### 这次的分享就是题目中提到的,还包括申请自己的域名和免费部署landing page的过程.

### 用到的工具分别是chakraui,namespace,netlify

# chakraUI

### 创建项目

打开chakraUI的官方文档(https://chakra-ui.com/getting-started/nextjs-guide),根据文档一步步的创建项目

```jsx
创建项目的命令
npx create-next-app dhh-chakra --typescript --use-npm
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/d49da9b1-5dee-43fa-bd78-d6d94e572ec0/Untitled.png)

全部选择no即可

### 配置charkraui

```jsx
npm i @chakra-ui/react @chakra-ui/next-js @emotion/react @emotion/styled framer-motion
npm i @chakra-ui/icons
npm i react-icons
```

### 在pages/_app.js中填写如下内容

```jsx
// pages/_app.js
import { ChakraProvider } from '@chakra-ui/react'

function MyApp({ Component, pageProps }) {
  return (
    <ChakraProvider>
      <Component {...pageProps} />
    </ChakraProvider>
  )
}

export default MyApp;
```

至此就是创建和配置chakraui的全部过程,是不是很简单

# 接下来打开chakraui的templete网站

https://chakra-templates.dev/ ,开始”写”landing page

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/04f2e3ff-01d5-457a-890d-c5a73da05d4f/Untitled.png)

landing page 页面主要包括以下几个部分

```jsx
navbar
hero
feature
stats
testimonials
contact
cta
footer
```

我们按照这个顺序往下不断罗列

选择自己喜欢的样子,然后点击code,直接复制粘贴代码到项目中

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/875481e0-818b-47c5-9805-cd8f95e68126/Untitled.png)

在项目中创建components目录

在components目录中创建hero.jsx,复制自己选择的hero代码并粘贴

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/8eccd48e-b3bf-4251-a6b7-4f5e146c974d/Untitled.png)

然后在项目的pages/index.jsx中导入hero组件

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/383891d8-e43c-4a3b-a9c4-16b99fc45d62/Untitled.png)

在项目的根目录中执行 npx run dev,在本地浏览器中就可以看到此页面

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/380e845d-1661-4127-92c6-484ad4bac39c/Untitled.png)

接下来我们继续补充页面的其他部分

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/b29c1313-7f8f-40fc-aa26-bb15764ad6b1/Untitled.png)

依次导入组件

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/d44a9cde-737c-4946-87d1-7c07a623c3df/Untitled.png)

执行npm run dev ,展示的结果如下

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/b31727a3-67a5-4979-bcd8-12fa6105fb87/Untitled.png)

# 

# 部署到免费的netlify网站

https://app.netlify.com/login

注册github,并利用github账号登录netlify网站,将项目代码部署到netlify中.

这部分的过程是: 将项目代码上传到github中,然后netlify拉取github中的代码并部署.

1. 上传代码到github

创建github仓库

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/5ce2559f-22df-4076-8818-946de67e3c69/Untitled.png)

上传到github

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/268e89d4-c8c5-4382-b8c0-5740b5db9cae/Untitled.png)

1. 在netlify部署

创建新的网站

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/4fcfdd3f-8600-430f-b0b9-f5f226271823/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/c86aad09-82ad-4f08-b34f-ecb63c2ec767/Untitled.png)

用默认配置,直接部署

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/9f58068e-de4d-4c99-b6f4-20105d0c9704/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/352f1237-cbd6-4a63-a483-7712af048260/Untitled.png)

当显示publish,说明可以访问了

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/3d6628b4-f511-4fbe-a5aa-f451a54ac2dc/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/93263633-d502-4837-9c0c-6495c109da7c/Untitled.png)

整个过程还是比较顺利的.这样就完成了从项目创建到网站部署的全过程.

# 配置自己的域名

netlify上域名是以netlify.app结尾的,如果要使用自己的域名的话,需要一些配置

以在namesilo上购买的域名为例子.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/69bc3344-4474-409c-a89e-456e75cd4d64/Untitled.png)

购买了自己的域名dhh888.top

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/c6732ecf-2a44-4f81-afe6-5201734d79e1/Untitled.png)

购买后在netlify中验证并添加该域名

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/e94c92b2-53bd-4563-a109-a4169a20bc9b/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/57992940-9219-4615-9689-0ecdf171d820/Untitled.png)

分别点开这里,找到DNS记录的配置

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/f04e703c-87f0-4789-95b5-0c58921ebab0/Untitled.png)

A记录的配置

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/7b5024fc-9110-46cb-8300-5658b58821da/Untitled.png)

CNAME记录的配置

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/0d61d9c5-d824-4fe1-a459-eae4ea1a92ea/Untitled.png)

进入namesilo管理页面,找到 dhh888.top的配置页面,将上面的记录配置添加上去

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/f8b45db3-712b-49f0-894b-1ff127b2a29d/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/b8e518ad-218b-4640-ad31-493096ffaea9/Untitled.png)

接下来就是等待域名生效,生效后这里就会消失.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/85931535-75cd-4d4b-bc52-c6f74f6dc1ee/Untitled.png)

最终的展示结果

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aeeb62f7-e003-45c7-bbf5-821c0da7d069/ef120b12-8a2a-4e6b-a435-acb618330efe/Untitled.png)

整个过程差不多20分钟就可以看到了.

### 总结: 上面就是全部的操作过程,首先是利用chakra templete 搭积木式的写landing page项目,然后将项目代码部署到netlify,然后在namesilo上购买自己的域名,将域名的DNS解析记录指向netlify,就可以使用自己的域名访问landing page