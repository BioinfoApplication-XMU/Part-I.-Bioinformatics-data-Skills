I have learnt how to upload files to Github.
I will upload my note learning R basic in the new branch.
# HelloR_Rbasic
*I will write basic R codes here.Now I'm trying to add the file to the repositories*
## Unit 1 An introduction to R
R是一种为统计计算和绘图而生的语言和环境，它是一套开源的数据分析解决方案。相较于其他数据分析软件如SPSS、Excel等，R在**免费**的同时拥有全面的统计研究能力，提供各式各样的统计分析技术，拥有**顶尖的制图功能**，同时其功能可以被整合进其它语言编写的应用程序，十分强大。
### R的获取与安装
http://cran.r-project.org
### 赋值符号 <-
R语言由函数和赋值构成。在R中，使用<-作为赋值符号，使用=赋值可能会出现问题。
`x <- rnorm(5)`
### 注释符号#
用#注释可增加代码可读性，与python等其他语言同理。
### 一个简单的R会话示例
```
age <- c(1,3,5,2,11,9,3,9,12,3)
weight <- c(4.4,5.3,7.2,5.2,8.5,7.3,6.0,10.4,10.2,6.1)
mean(weight)
sd(weight)
cor(age,weight)
plot(age,wright)
q()
```
