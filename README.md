# git_hook
一次线上事故，有人把测试分支合并到待上线的分支。然后自己研究了一下如何避免的方法。

一般上线分支名称不应该带有 test 字样的。  
思路：在运维merge or push 的hook 函数检测此分支、以及这个分支曾经merge过的分支名称是否包含 “test” 字样  
     如果有抛出异常给与提醒，不予执行此操作  or  有一定的 交互（是否通过 权限 交给运维）  
    
这个脚本应该是运行在运维上线的机器上的。因为开发人员不可避免，会在自己本机，做测试（建立 带有test字样的分支）。但是上线的分支，我觉得绝对要避免的。  
我曾经的技术总监不同意这一点，我表示无语。难道不是在制度上避免此类错误发生么。虽然概率小，但是一旦上线，那后果可能是极其严重的。


思路基本就是如上。  
1、查看git 的钩子函数  
在 .git/hooks 目录下看git 给的默认钩子函数。  
参考blog（http://wubaoguo.com/2016/12/02/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/git%E9%92%A9%E5%AD%90%E8%AF%A6%E8%A7%A3/）
（https://github.com/geeeeeeeeek/git-recipes/wiki/5.4-Git-%E9%92%A9%E5%AD%90%EF%BC%9A%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BD%A0%E7%9A%84%E5%B7%A5%E4%BD%9C%E6%B5%81）
applypatch-msg.sample、  
commit-msg.sample、 
post-update.sample、  
pre-applypatch.sample、  
pre-commit.sample、  
pre-push.sample、  
pre-rebase.sample、  
pre-receive.sample、   
prepare-commit-msg.sample、  
 

服务端钩子 (存在于服务端的仓库中) 
update.sample 
pre-receive.sample  
post-receive.sample、

2、运维上线代码分支流程一般是：  
先拉 开发人员分支 ===》 release 分支  ====》 然后在把 release 分支 推到master 分支。  
所以我想的是有一个premerge hook ===》 但是git 没有。。。。这就呵呵了  





     
