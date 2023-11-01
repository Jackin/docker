# Docker 基础镜像

## Debian Linux 基础镜像

*buster* 现在更常见，未来 *bullseye* 会越来越多。一般情况下，其它几个都不是我们的第一选择。

| 代号	       | 版本号           |
|-----------|---------------|
| bookworm	 | Debian 12 的代号 |
| bullseye	 | Debian 11 的代号 |
| buster	   | Debian 10 的代号 |
| stretch	  | Debian 9 的代号  |
| jessie	   | Debian 8 的代号  |
| wheezy	   | Debian 7 的代号  |
| squeeze	  | Debian 6 的代号  |


[Debian 管理员手册](https://www.debian.org/doc/manuals/debian-handbook/index.zh-cn.html)

[Debian 发行版本](https://www.debian.org/releases/)

## Alpine Linux 基础镜像

Alpine 是众多 Linux 发行版中的一员，和 CentOS、Ubuntu、Archlinux 之类一样，只是一个发行版的名字，号称小巧安全，有自己的包管理工具 apk 。

> 官方 Alpine 镜像的文档：http://gliderlabs.viewdocs.io/docker-alpine/

但是 Alpine 的小是有代价的，在不轻易间可能会给你造成麻烦！

Alpine Linux 除了插件了一些不必要的软件之外，特别重要的是，它使用了 *musl libc* 代替了大名鼎鼎的 *glibc* 。

*musl libc* 含有和 *glibc* 一样的标准功能，但是问题是 *glibc* 还有标准功能之外的扩展功能，由于 *glibc* 的历史地位和市场占有率，导致 *glibc* 的扩展功能实际上的使用也很广泛！

有不少软件的编译、安装和运行都用到了 *glibc* 的扩展功能，因此这些软件在使用了 *glibc* 的基础颈项上就能运行，在使用了 *musl libc* 的 alpine 上则不行。

另外，Alpine 并没有像 Red Hat 或 Canonical 之类的大公司为其提供维护支持，软件包的数量也比这些发行版少很多（如果只看开箱即用的默认软件仓库，Alpine 只有 10000 个软件包，而 Ubuntu、Debian 和 Fedora 的软件包数量均大于 50000。）

[Package Database](https://pkgs.alpinelinux.org/packages)

[Alpine Linux Wiki](https://wiki.alpinelinux.org/wiki/Main_Page)

[APKBUILD Reference](https://wiki.alpinelinux.org/wiki/APKBUILD_Reference)

[APKBUILD examples](https://wiki.alpinelinux.org/wiki/APKBUILD_examples)

## :slim 镜像

如果实在不想折腾，可以选择一个折衷的镜像 xxx:slim。

`slim` 镜像一般都基于 Debian 和 glibc，删除了许多非必需的软件包，优化了体积。

如果构建过程中需要编译器，那么 `slim` 镜像不适合，除此之外大多数情况下还是可以使用 `slim` 作为基础镜像的。

`slim` 镜像是完整镜像的配对版本，通常只安装特定工具所需的最小包。
