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
(https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)
（https://imweb.io/topic/5b13aa38d4c96b9b1b4c4e9d）
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

方案 1、客户端 pre-push  hook 里面完成    2、服务器端的 pre-receive 完成  ；但是这需要 服务器与移动端的交互，可能有点难。（先实现第一个吧）

shell 脚本:
pre-push：：

# 展示参数

# 找不到需要的参数  ===》 还是得仔细读东西啊 .在pre-push 文件上面有说明。==》仔细阅读啊 ====》 This hook is called with the following parameters:  ====》  看文档，就肯能 知道 git 调用 这个 bash 的时候，会带有什么参数了。以及在bash 有哪些变量参数。


for arg in $*
do
   echo "arg: $index = $arg"
   let index+=1
done

for arg in "$@"
do
   echo $arg
done

我靠，不行啊  pre-push  里面只有不多几个参数 $1 、$2  local_ref、 local_sha（本地commitid）、 remote_ref 、remote_sha（远程commitid）

其中 

 git push  远程仓库  分支
 
 看来只能是 git merge qiskit-sdk-py-learn-note  ???   但是没有 pre-merge 呀？

咋办？？？

local_sha  ===》 推到出分支 ？？？  commitid 这个行不通。为啥git就没有把 那个分支传进来呢。。。。。真是

stackoverflow上  pre-merge都没有好的解决方案。
https://stackoverflow.com/questions/19102714/how-would-i-write-a-pre-merge-hook-in-git
https://stackoverflow.com/questions/11532556/git-pre-merge-hook

呵呵了。

这些思路是对的 Yeah !!!
https://segmentfault.com/q/1010000000464961

1、
- git branch --merged
- git branch --no-merged  

2、
 git log | grep branch_name
 
 
 
 1、git branch --merged   获取合并过的分支。   
 2、检测是否包含目标分支
 
 
找不到 hook 还是没能把 分支传进来。。。。。。

chmod u+x pre-push

大体思路走通了，细节待完善。。。。。。  
example 目录
/Users/username/lab/qiskit-sdk-py/.git/hooks





     
