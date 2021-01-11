# Linux

Linux basic codes for practice
(From Lulab-Tsinghua University)



## 1\) Examples

* grep

```text
grep -v -c pattern FILES  #(-v exclude -c count lines)
grep -l pattern FILES #(-l: list of the files)
```

* head, tail V.S. cut
* cat V.S. paste 

```text
head -100 FILE
tail -100 FILE
cut -f 2 FILE #(-f: field)
cut -d ; -f 2 FILE #(-d change delimiter)

cat FILES* > NEW_FILE
paste FILES* > NEW_FILE
```

* rev and tac

```text
echo john temp | rev     #(output pmet nhoj)
tac    #(reverse by lines)
```

* wc

```text
ls * |wc -w   or  wc -l  #(count the number of files)
```

* comm and vimdiff, diff

```text
comm -2 file1 file2
diff file1 file2
```

* sort

```text
sort at position 1, delimeter is ','; -r reverse sort
sort -t ',' -k 1 sort -r -t ',' -k 1
```

* uniq

```text
cat human.gtf | cut –f 2,3 | sort | uniq –c
```

* seq

```bash
for i in `seq 1 2 10`; do echo "$i"; done
for i in `ls *`;do head "$i" | cut -f 3 ; done
```

* sed

```bash
cat 1.gtf | head |sed 's/1802/12/'
```

* awk

```bash
cat 1-6.gtf |head | awk '{if($4>1000 && $5>2900)print}' | cut –f 3 | sort | uniq –c
```

* history

```text
Ctrl - r : search previous command
!V (im) : last command (be cautious !)
```

* find and locate

```bash
find . -name "*.gtf"  # find . -name "pattern"
locate
```

* xargs 

grep for pattern in all files on the system:

```text
find / | xargs grep pattern > out &
```

Move files in olddir to newdir, showing each command:

```text
ls olddir | xargs -i  mv olddir/{} newdir/{}  
find . -type f -name "*.txt" | xargs -i  mv {} newdir/{}
```

## 2\) Tips

### \(1\) bashrc and .bash\_profile

Example:

```bash
# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi


# User specific environment and startup programs
if [ -f $HOME/shortcuts ]; then
        source $HOME/shortcuts
fi
PATH=$HOME/bin:$PATH
export PATH


# User specific aliases and functions
alias qstat="qstat -u '*'"
#alias screen="/usr/bin/screen -D -R"
#alias rm="$HOME/bin/del.sh"
#alias undel="$HOME/bin/del.sh -u"
#alias ls="ls --color"
alias ld="ls -d"
alias c="clear"
alias l="ls -alh"
alias lf="ls -F|grep /"
alias lt="ls -tlr"
alias mv="mv -i"
alias cp="cp -pi"
alias diff="diff -b"


#PERL5LIB=$MYHOME/perllib:$MYHOME/perllib/lib64/perl5/site_perl/5.8.5:$MYHOME/perlib/lib/perl5/site_perl/5.8.5
#export PERL5LIB
#export R_LIBS_USER=~/R:/data/apps/R_library
```

[more examples](https://github.com/lulab/PI/blob/master/workflow/bash_profile)

### \(2\) nohup, screen and qsub {#nohup}

* run something at background

`./run.bat >& run.log &`

`fg`

* **nohup**:

`nohup nice -19 run.bat >& run.log&`

* **tmux**:
  * start a new session: `tmux` or `tumx new-session -s session-name`
  * detach: ctrl-a, d
  * re-attach: `tmux attach-session -d -t session-name #detach it first`
* **screen**: \# a popular alternative of tmux
  * start a new session: `screen`
  * detach: ctrl-a, d
  * re-attach: `screen -R -D  # detach it first`
* **qsub**:
  * qlogin not allowed in some servers 
  * Check `qstat -u '\*'`
  * [example script](https://github.com/lulab/PI/blob/master/workflow/run_bins.pbs)

### \(3\)  secure your files

* make your files Read-only 
  * permission for a executable bash script is usually **755**
  * using `chmod -R a-w` for raw data and input files
* 777, rwxrwxrwx is forbidden \(using `chmod -R o-w`\)
* Change user group and permission

```text
usermod –G admin  john  #  add sudo

usermod –G lulab  liyang  #  add to lulab group
chgrp –R  lulab  yourdir/
chmod –R  g+w   yourdir/

chmod  a+x  yourdir/
chmod  o+x  yourdir/
chmod  o-w  yourdir/
```

* Blocking the root:

vim /etc/ssh/sshd\_config  
`#PermitRootLogin yes —>no`

### \(4\) Setup ssh key {#ssh-key}

* ssh-keygen  \(authorized\_keys  id\_rsa.pub 权限设置为 600\)

```text
ssh-keygen -t rsa
copy authorized key in ~/.ssh/id_rsa.pub to remote_machine:~/.ssh/athorized_keys
```

> This will be very useful later when we work on remote machine as a local one, especially for jobs like **backup** and **script editing.**

### \(5\) System

* kernel version

```text
uname -r 
uname -a
```

* mount dir/ to local machine using NFS or sshfs \(then you can Edit text and view figures remotely\)

_I recommend using "transmit" app in your mac instead of mounting NFS or sshfs._

```text
$ yum install fuse-sshfs
$ usermod -a -G fuse john
sshfs john@nye:/Users/john /mnt/nyefs fusermount -u mountpoint
automounting: in /etc/fstab
sshfs#john@nye: /mnt/nyefs fuse uid=500,gid=100,allow_other 0 0
mount /mnt/nyefs; umnout /mnt/nyefs
```

* forward the log file to an email address

```text
vim /home/.forward or /root/.forward
  zhi_lu@tsinghua.edu
```

* Kill batch job 

```text
top
ps -edalf | grep username 
kill -9 PID
```

## 3\) More Readings and Practices

* [**Teaching Video**](../getting-startted.md#learning-materials)：Week I. 2. Linux
* **for Beginners** 
  * 阅读和练习《鸟哥的Linux私房菜-基础学习篇》如下章节:
  * 《“笨办法”学python》附录“命令行快速入门”  

> 第5章  
> 5.3.1 man page  
> 第6章  
> 6.1用户与用户组  
> 6.2 LINUX文件权限概念  
> 6.3 LINUX目录配置  
> 第7章Linux文件与目录管理  
> 7.1目录与路径  
> 7.2文件与目录管理  
> 7.3文件内容查阅  
> 7.5命令与文件的查询  
> 7.6权限与命令间的关系  
> 第8章  
> 8.2文件系统的简单操作  
> 第9章  
> 9.1压缩文件的用途与技术  
> 9.2 Linux系统常见的压缩命令  
> 9.3打包命令：tar  
> 第10章vim程序编辑器 （或者其他编辑器文档）  
> 第11章 认识与学习bash  
> 第25章 LINUX备份策略  
> 25.2.2完整备份的差异备份  
> 25.3鸟哥的备份策略  
> 25.4灾难恢复的考虑  
> 25.5重点回顾
>
> 第11章 认识与学习bash  
> 第12章 正则表达式与文件格式化处理  
> 第13章 学习shell script

* **for Advanced Readers** 

《Bioinformatics Data Skills》

> 3\) Remedial Unix Shell

## 4\) Homework

1. 解释gtf/gff文件中第4、5列（$4,$5\)代表什么，exon长度应该是$5-$4+1还是$5-$4
2. 从gtf/gff文件中寻找3个最长的exon：

   `grep exon *.gtf | awk '{print $5-$4+1}' | sort -n | tail -3`这个方法有什么bug？

   有新的方法加分，但必须注释清楚每个语句和参数的意义和结果。  

3. 从gtf/gff文件中寻找并计算每一个transcript的长度，注意不能重复计算，不能包含intron。

