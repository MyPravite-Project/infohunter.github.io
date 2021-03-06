---
layout: post
title: 《OpenSSL辟邪剑法》第一式
date: 2016-10-14 22:00:00
---

# OpenSSL前世今生

## SSL协议

说到人类互联网的历史，就不得不提到一个古老而响亮的名字，Netscape。这家总部位于美国加州的公司，为人类贡献了并留下了很多有意义的东西，例如JavaScript，Gecko，Mozilla项目，当然也包括SSL协议。早在1995，Netscape提出了SSL的第一个可用版本SSL 2.0，在1996年，Netscape发布了SSL 3.0的协议规范，由此拉开了当代Web流量加密的序幕。当时在Netscape做首席科学家的Taher Elgamal博士是公认的“SSL之父”。

> Taher Elgamal
> Taher Elgamal博士是著名的ElGamal非对称加密和签名算法的发明者，广泛使用的DSA签名算法属于ElGamal算法的变种版本。Taher Elgamal博士师从密码学大师Martin Hellman，就是DH算法中的那个'H'，而ElGamal算法就是基于DH算法而来。Taher Elgamal博士目前在Salesforce.com担任安全方向CTO。

Netscape在自家的全线产品中实现了对SSL协议的支持，包括Netscape Navigator浏览器以及服务器端的FastTrack和Enterprise web server等。但是由于Netscape是一家美国公司，受当时美国对密码技术的出口限制，Netscape的那些提供SSL的产品无法在美国之外使用高安全强度的加密技术。例如，当时Netscape的产品分为“美国国内版本”和“国际版本”，在国际版本的产品中，对称秘钥的长度被限制在40-bit，RSA私钥长度被限制在512-bit，都是可以被很容易的破解。现今随着互联网和开源软件的崛起，优秀的密码学系统被广泛的使用，密码学出口限制的实际效果已经大打折扣，但这种限制在当时确实是一个问题。

## SSLeay

1995年，当时在澳大利亚Bond大学工作的Tim Hudson，有几个项目需要使用SSL，但是受到当时美国的密码学出口限制的影响，他唯一能获得SSL这项技术的途径，就是使用Netscape的出口产品。具Tim Hudson描述，使用Netscape的产品，存在着很多的问题：

1. 首先要取得RSA Data Security（即现在的RSA Security LLC）的专利授权
2. 其次Netscape的license费用比较昂贵，当时是30000美元
3. 最后是Netscape收密码学出口管制的影响，只能提供低安全等级的加密技术，例如40-bit的对称加密

Tim Hudson认为，即使付出了高昂的成本，也只会得到低安全强度的加密算法，这无法解决他项目上的问题，因此他决定自行开发一个SSL库来支撑他在美国之外的几个研究项目。Tim找到了同在Bond大学读PhD的Eric Young，来共同实现一个新的SSL库。Eric是Tim在昆士兰大学（The University of Queensland）时期的同窗好友，因为Eric的时间比较充足而且他已经开发了一个DES加密算法的实现，所以由Eric主要来负责SSL库本身的开发，而Tim主要负责应用程序端的接入开发、编写文档以及处理用户问题。这个SSL库以Eric的全名首字母缩写命名（Eric Andrew Young），是为SSLeay，其读音为S-S-L-E-A-Y，按单个字母读。SSLeay支持SSL 2, SSL 3以及TLS 1,，其大部分API和今天的OpenSSL相差无几，关于当年SSLeay的文档可以在这里找到：http://www.umich.edu/~x509/ssleay/

1998年8月，Eric和Tim加入了RSA Data Security公司，他们基于SSLeay为RSA公司开发SSL-C，一个应用在RSA的BSAFE产品中SSL SDK，而SSLeay则停止了开发，也再没有新的版本发布。2007年，Tim离开了RSA，转而继续运营Cryptsoft公司，该公司是Eric和Tim在加入RSA之前创办的，而Eric则一直留在RSA工作。2012年开始Tim成为了OpenSSL基金会的合伙人并在2014年成为OpenSSL开发团队成员。

在Eric和Tim加入RSA之前，他们为一家名为C2Net的公司工作了大约一年半的时间，并主要负责SSLeay的开发，这段工作经历过为后来的OpenSSL的诞生打下了伏笔。

## C2Net

C2Net是一家位于美国加州提供隐私加密服务的公司，成立于1994年，主要提供包括浏览器、服务器以及代理软件等多种产品，并在其产品中提供完善的加密手段保护用户的隐私安全。在1997年，C2Net成立了欧洲公司，由Mark J. Cox担任欧洲公司的总经理，负责C2Net全球化产品的开发，于此同时，Eric和Tim于1997年也加入了C2Net公司，负责SSLeay的开发，而C2Net的全球化产品中使用的就是SSLeay来提供SSL功能。

当时C2Net的拳头产品是一款名为Stronghold的Web服务器软件，全面支持SSL/TLS协议以及高强度的密码学算法。该产品的主要竞争对手是Netscape的Web服务器软件。当时Stronghold也是唯一一款在全球范围销售的提供强加密算法的同类产品，这主要得益于Stronghold是在美国之外开发的（英格兰和澳大利亚），当然C2Net依然要为RSA的专利买单，直到RSA专利过期。

在1998年的时候，Tim和Eric离开了C2Net，加入了RSA Security，而C2Net则在2000年的时候被Red Hat收购，Mark Cox也成为了Red Hat的员工，目前负责Red Hat的安全方向。

> C2Net
> C2Net是一家在互联网安全领域举足轻重的公司，其具体的介绍可以参考：https://awe.com/mark/history/c2net.html 以及关于Stronghold的信息：https://awe.com/mark/history/stronghold.html

## OpenSSL

当Tim和Eric在1998年离开C2Net之后，Mark决定启动一个新的开源项目以继承SSLeay，这个项目就是OpenSSL。OpenSSL代替了SSLeay对C2Net的产品进行支持，并作为一个成功的开源项目被广泛的应用。

OpenSSL从1998年释放第一个版本0.9.1c开始，到最新的1.1.0b版本，期间也经历了许多波折，其中对于OpenSSL项目影响最大的就是2014年爆出的心脏滴血（Heartbleed）漏洞。

> Heartbleed
> http://heartbleed.com/，其主要影响是，攻击者可以利用该漏洞对服务器内存进行读取，进而获取各种秘钥、用户名/密码等信息。

心脏滴血漏洞被披露之后，Linux基金会为了提高互联网基础设施的软件质量，发起了名为Core Infrastucture Initiatives (CII)的项目，该项目联合了多家商业公司，包括IBM，微软，Intel，AWS等，其目的是为了资助一些关键的开源项目，帮助它们变得更加完善且高质量，而OpenSSL则顺理成章的成为了CII第一个资助的开源项目。在此之前，OpenSSL处于‘缺衣少粮’的状态，每年只能依靠捐款而获得大概2000美元的经费。CII资助了开发人员在OpenSSL项目上进行了全职开发，并希望此举能达到提高OpenSSL的安全性等目的。

在心脏滴血漏洞公布之后，OpenBSD和Google分别对OpenSSL进行了fork，开始发展自己的SSL/TLS库，他们各自库的名称分别名为LibreSSL和BoringSSL。LibreSSL是OpenBSD社区在2014年4月从OpenSSL中fork出的一个版本，主要目的是为了重构OpenSSL的代码并增强安全性，LibreSSL的第一个版本在2014年7月发布，初始版本为2.0.0，LibreSSL中删除了OpenSSL中遗留的大量无用和老旧代码，并增加了一些新的特性，例如ChaCha和Poly1305算法等。在2014年6月，Google也基于OpenSSL fork出了自己的版本，BoringSSL。BoringSSL并不旨在取代OpenSSL成为一个通用的SSL库，而是更多的为了Google自己的业务需要而独立开发的版本，并主要在Chrome和Android中使用。

根据Mark J. Cox的说法，在未来，OpenSSL希望能成为最广泛使用的通用型SSL库，并且基于心脏滴血之后的种种措施和手段，期望能将OpenSSL这个品牌变得更加正面以减弱那些负面的影响。关于LibreSSL和BoringSSL，Mark表示OpenSSL会和他们在安全问题上保持沟通，并维持良好的合作关系。
