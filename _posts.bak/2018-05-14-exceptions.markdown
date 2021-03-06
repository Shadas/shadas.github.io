---
layout: post
title: 异常处理的一些笔记
category: python
tag : python
--- 

最近一直在写python, 然后写了很多try...except..., 因为是脚本项目用的也很糙, 直接去捕获Exception, 然后靠具体的exception信息定位debug, 这样做处理开发中的bug是足够了的, 因为信息是给开发者看的, 也可以准确debug出那些自己开发代码中的错误, 之后又回来看这个处理, 感觉还是有问题, 给出的异常并没有区分种类, 仅仅是靠e的msg信息作了区分, 这样的话写log还可以, 但是给调用者就没法细分了, 因为看到的都是Exception  

其实工作了快3年接触的这几门语言c, php, python, go, 对于程序中产生的错误都不类似java那样, 强制走异常处理。 c本身是靠返回跟errno, go虽然提供了defer,panic,recover这一套异常处理, 但是大部分go的代码还是到处err != nil, php跟python倒是兼而有之, 各种错误返回都可以用, 也许是没怎么写过java, 所以总觉得自己用异常机制的时候很陌生, 不知道不同情况下的对应规则, 所以就去查了查看   

其实跳出来看, 不管是返回错误码还是异常, 都是我们程序遇到错误时的处理办法, 程序总是会出错的, 那么多可能的错误, 如果给出类型再来处理这样会轻松一些, 个人觉得java对异常的分类可以拿来当例子看看, 大致是分成三类, 都继承自throwable  
 
>Error, 系统运行时的错误, OutOfMemoryError, 通常遇到时, 这是无法挽回的异常, 程序是会终止    
>RuntimeException, 运行异常, 除0还有参数类型错误是属于这些, 个人觉得这些属于编码中的失误, 利用这些异常帮助我们debug, 并且代码最终应该能够避免到这些异常的触发, 因为我们最终版本的代码应该对类型什么的做过检查了  
>IOException, 这种异常在java中被叫做checked exception, 比如类似FileNotFoundExcetion, java编译器严格要求了对这类异常的处理, 调用包含checked exception的方法时, 必须catch或者重新throw这个异常  

看了一圈下来, 我自己觉得, 开发的时候如果不严格的场景, 比如你只是写给自己用的脚本, 可以直接try...except exception as e, 只要把e的信息记录到log并返回就好了, 因为异常是给开发者看的, 信息足够到能明确定位让你改bug就好了。 但是当用在某个系统的项目内时, 要细化到每个具体的异常, 让调用者可以看到具体信息以及区别, 因为很可能这里的信息要具体细化返回给客户了   

另外看资料的时候看到的, java的checked exception是有争议的, 很多大佬都有自己的观点, 本身我工作之后就没怎么用过java了, 看了很资料大致明白了这里面的争论点, 后来看到这样一句话:  
`Checked exceptions表示了关于一个操作的一些有用的信息, 这项操作调用者无法控制, 但是调用者又需要知道这些信息. 比如, 文件系统可能已满, 或者远端关闭了连接, 又或者访问授权不足以执行这项操作`  

形如这种情况的异常确实必须要求调用者来处理, 所以强制要求编译器检查是没问题的, 但是感觉这种IO exception其实是分情况的, 一种是调用者给的参数的相关IO, 另一种是我们的方法直接进行的IO。 前一种可以算是调用者自己在开发过程完善的异常, 后一种就是调用者必须catch处理的异常了。 看了一些java代码, 有场景就是哪里出现了checked exception就就地处理去catch掉, 不去throw而是替换成一个unchecked exception方便调用者使用  

总结一下, 平时写php都是在框架的保护下, 不管是用的yaf还是thinkphp都已经在框架的层面处理了异常, 用起来不需要多想, 感觉在工作之中处理异常还是要跟着公司的代码节奏来, 不管是错误码还是异常跟着规范就可以了, 如果是平时自己的项目, 尤其是爬虫的东西, 可以随性一点




