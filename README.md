
### 1\.简介


上一篇文章，宏哥已经在搭建的java项目环境中添加jar包实践了如何启动浏览器，今天就在基于maven项目的环境中给小伙伴们或者童鞋们演示一下如何启动浏览器。


### 2\.eclipse中新建maven项目


1\.依次点击eclipse的file \- new \- other ，如下图所示：


![](https://img2020.cnblogs.com/blog/1232840/202106/1232840-20210621153308830-1340547003.png)


2\.在搜索框输入关键字“maven”，然后选中“maven project”，如下图所示：


![](https://img2020.cnblogs.com/blog/1232840/202106/1232840-20210621153458991-860289036.png)


3\.选择创建后的工作区——项目存放的地址。如下图所示：


![](https://img2020.cnblogs.com/blog/1232840/202106/1232840-20210621160838780-257206243.png)


4\.选择Maven项目的模板也叫项目类型（quikstart或者webapp等等），，如果选择create a simple project，则跳过了下面的步骤，也就不存在这个问题了，但是如果需要选择项目类型，则不能勾选create a simple project）如下图所示：


![](https://img2020.cnblogs.com/blog/1232840/202106/1232840-20210621160944549-791466042.png)


5\.宏哥为了省事，直接选中create a simple project，点击next，输入Group Id和Artifact Id。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701111109606-517784329.png)


6\.点击“Finish”，查看新建的maven项目，如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701111152344-1504642.png)


到此，创建maven项目成功！！！


### 3\.maven项目加载playwright依赖jar


#### 3\.1加载playwright依赖


maven项目加载playwright依赖就不想上一篇java项目加载playwright那么费事需要把jar包引入到项目下，maven项目只需要将相关的jar包依赖配置到pom.xml文件中就会自动加载了。因此要给上面创建的maven项目中加载playwright依赖jar包，只需在pom.xml中引入playwright的jar包即可；具体步骤如下：


1\.查看maven仓库：[http://mvnrepository.com/](https://github.com)  如下图所示：


![](https://img2020.cnblogs.com/blog/1232840/202106/1232840-20210621165435772-2089093383.png)


2\.搜索playwright, 输入playwright，点击“Search”，如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240702083129007-1408234563.png)


3\.点击“[Playwright Main Library](https://github.com):[milou加速器](https://xinminxuehui.org)”,查看自己需要的playwright版本，playwright我们都会选择最新的（宏哥这里用1\.40\.0举例一下，最新可能有bug，老的可能有些方法不支持，宏哥这里选择一个不新也不旧的），如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701112129484-74324843.png)


4\.下载playwright\-1\.40\.0版本，点1\.40\.0进入页面后，只需要单击下边的编码就自动全选复制了。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701112510627-2090548169.png)




```


    com.microsoft.playwright
    playwright
    1.40.0

```


5\.copy到maven项目中的pom.xml中,如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701112815219-2009880094.png)


6\.playwright的jar包maven会自动加载，从右边路径可以看到jar的路径在本地仓库。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701113210429-1338266687.png)


需要其他的jar包只需配置到pom.xml中即可！是不是比之前介绍的方法简单多了哈！


#### 3\.2修改jdk版本


因为playwright的Java需要Java8以上，所以需要重新配置jdk。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701113356996-1764853320.png)


1\.右键JRE System Library\[JavaSe\-1\.7] \-\>properties。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701113522989-34755499.png)


2\.选择javaSE\-1\.8，如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701113625127-681250508.png)


3\.点击“OK”后，就变成JavaSE\-1\.8了，如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240701114040927-149796132.png)


好了，至此，基于maven的java\+Playwright自动化测试环境搭建就搭建成功了。下边就开始实践Maven项目如何启动浏览器。


### 4\.启动Chrome浏览器


大致思路：打开Chrome浏览器，访问百度网址，获取网址的title，然后再关闭Chrome浏览器。根据思路进行代码设计。



#### 4\.1代码设计


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240703112304452-25540519.png)


#### 4\.2参考代码




```
package com.bjhg.playwright;

import com.microsoft.playwright.Browser;
import com.microsoft.playwright.BrowserType;
import com.microsoft.playwright.Page;
import com.microsoft.playwright.Playwright;

/**
 * @author 北京-宏哥
 * 
 * @公众号:北京宏哥（微信搜索，关注宏哥，提前解锁更多测试干货）
 * 
 * 《刚刚问世》系列初窥篇-Java+Playwright自动化测试-4-启动浏览器-基于Maven（详细教程）
 *
 * 2024年7月10日
 */
public class LaunchChrome {
    
    public static void main(String[] args) {
        try (Playwright playwright = Playwright.create()) {
          //使用chromium浏览器，# 浏览器配置，设置以GUI模式启动Chrome浏览器（要查看浏览器UI，在启动浏览器时传递 headless=false 标志。您还可以使用 slowMo 来减慢执行速度。
          Browser browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(false).setSlowMo(50));
          //创建page
          Page page = browser.newPage();
          //浏览器打开百度
          page.navigate("https://www.baidu.com/");
          //打印title
          System.out.println(page.title());
          //关闭page
          page.close();
          //关闭browser
          browser.close();
        }
      }

}
```


#### 4\.3运行代码


1\.运行代码，右键Run AS\-\>java Application，就可以看到控制台输出，如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240703112227345-1523475634.png)


2\.运行代码后电脑端的浏览器的动作。如下图所示：


![](https://img2024.cnblogs.com/blog/1232840/202407/1232840-20240703112202994-1628739965.gif)



好了，到此，在Maven项目中如何启动Chrome浏览器，就完成了，Firefox和webkit的两个浏览器和Chrome的非常相似，宏哥就不在这里进行赘述了。


### 5\.小结


宏哥因为之前做过python、java语言和selenium，经常碰到头疼的问题就是：出现浏览器版本和驱动版本匹配的问题，新手一定要注意这个问题。但是playwright无论是Java还是python语言，无论是新手还是老鸟就都不需要担心这些问题了，而且今天讲解和分享的非常简单，就是简单换个方法就可以启动不同的浏览器了。好了，今天关于三大浏览器的驱动宏哥就分享到这里，感谢你耐心的阅读。


