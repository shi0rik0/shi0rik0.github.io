---
title: "用CloudFlare Workers建立免费的Docker Hub镜像"
date: 2024-10-31T14:16:00+08:00
draft: false
---

2024 年 12 月 28 日更新：似乎[有消息](https://linux.do/t/topic/297635)称 CloudFlare 更新了用户条款，禁止将其服务用于代理用途。因此这个方法可能有风险。

由于中国封锁了 Docker Hub，导致在国内用 Docker 的时候会遇到很多问题。一个最简单的办法就是使用镜像，然而很遗憾的是，很多镜像站也被封锁了，因此我不得不自己建立一个 Docker Hub 镜像。在 GitHub 上，我发现一个[使用 CloudFlare Workers 建立 Docker Hub 镜像的项目](https://github.com/ciiiii/cloudflare-docker-proxy)。CloudFlare Workers 是有免费版的，这就意味着可以免费建立一个 Docker Hub 镜像。然而原项目的 README 文档写得实在是太简陋了，我摸索了很久才最终测试成功，因此就将一个更详细的步骤写在这里。

首先当然是要注册一个 CloudFlare 账号，然后点击原项目 README 中的“Deploy with Workers”的按钮，按照网页的提示操作。操作完成之后，应该会：

1. 在 CloudFlare 里创建 2 个分别名为`cloudflare-docker-proxy`和`cloudflare-docker-proxy-staging`，后者不知道是做什么用的，也不用管，最终访问的是前者。

2. 在你授权的 GitHub 账户下自动 fork 一个仓库。需要修改代码的时候就在自己的仓库下面修改，修改完之后 GitHub Actions 会自动部署更改。

接下来要处理一下域名的问题。默认情况下 worker 绑定的域名是`cloudflare-docker-proxy.<username>.workers.dev`，但很遗憾的是，workers.dev 这个域名也被墙了，所以我们需要配置一个自己的域名。首先要在 CloudFlare dashboard 的[“Add a domain”界面](https://dash.cloudflare.com/?to=/:account/add-site)把你的域名添加上去。如果你的域名不是在 CloudFlare 注册的，添加完之后还需要在域名注册商那里将 DNS 服务器改成 CloudFlare 指定的 DNS 服务器。之后看到状态是“Active”就算成功了。在进行下一步之前，要将 CloudFlare 上的 DNS 记录全部删掉。

然后在`cloudflare-docker-proxy` worker 的管理界面的“Settings”-“Domains & Routes”中把你的域名作为 custom domain 添加进去。

最后就是修改代码。把`src/index.js`中的`routes`修改为你的域名即可：

```js
const dockerHub = "https://registry-1.docker.io";

const routes = {
  "foo.com": dockerHub,
};
```

最后试着用镜像源 pull 一下镜像：

```sh
docker pull foo.com/hello-world
```

在 Docker image 的名字前面加上镜像域名只是一个临时的办法，要让镜像永久生效，可以参考[这篇文章](https://www.cnblogs.com/lanjianhua/p/18361448)。
