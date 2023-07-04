---
title: word文档导出成PDF
date: 2021-03-02 11:11:43
tags: 
 - Java
 - Word转PDF
category: 工具
---

# Java实现Linux下Word文档导出成PDF

	最近在项目中有一个文档上传的功能，该功能要求用户上传一个Word文档，然后在前台可以下载Word文档或者在线浏览文档内容。
	Word文档的在线浏览好像不太容易实现，故采用将上传的Word文档另存为一份PDF版本，然后让其在线浏览PDF版本的文档，当下载的时候再下载Word版本。

---

百度了一下将Word转PDF方法大体上分为两大种

1. 调用现成的Word转PDF的API接口，一般是一些文档转换网站有提供，**虽说操作简单，但是大多数都要收费，而且免费版一般会有水印**，这种方法不考虑。
2. 一些开源的Java Word转PDF包，这种方式相对来说比较复杂，但是自定义程度高，而且基本不会收费。

下面我就来介绍一下我使用的第二种方法。

---

### JodConveter+OpenOffice

我使用的是[JodConveter](https://github.com/sbraconnier/jodconverter) +[OpenOffice](https://www.openoffice.org/)在linux下实现的，这套组合在Windows下也是没有问题的,只要下载对应的OpenOffice版本就可以了。

![image-20210302134315895](https://gitee.com/SimpleZzz/pic/raw/master/img/image-20210302134315895.png)

---

#### Linux下安装OpenOffice

Windows下安装OpenOffice就不用多说了，安装包打开之后无脑下一步就完事了，这里说一下Linux下的安装方法。

1. 首先下载linux下OpenOffice的安装包。我使用的是centos7.4所以直接下载这个rpm的包就可以了。下载后得到[Apache_OpenOffice_4.1.9_Linux_x86-64_install-rpm_zh-CN.tar.gz](https://sourceforge.net/projects/openofficeorg.mirror/files/4.1.9/binaries/zh-CN/Apache_OpenOffice_4.1.9_Linux_x86-64_install-rpm_zh-CN.tar.gz/download)这个文件。

    ![image-20210302134650357](https://gitee.com/SimpleZzz/pic/raw/master/img/image-20210302134650357.png)

2. 将下载好的文件上传至服务器并解压缩，会生成一个`zh-CN`文件夹。进入该文件夹下的`PRMS`文件夹。

3. 运行命令 `yum -y localinstall *.rpm`,成功之后会生成一个`desktop-integration`文件夹，进入该文件夹。

4. 运行命令`yum -y localinstall openoffice4.1.9-redhat-menus-4.1.9-9805.noarch.rpm`,安装成功之后会在`/opt`目录下生成`openoffice4`文件夹，这就是安装位置。

5. 至此OpenOffice安装就完成了，新版本的JodConveter可以自动调用系统中的OpenOffice而无需我们自己去手动启动它，这个我们后面会说。

---

#### 编写项目代码使用JodConveter

1. 首先项目中引入JodConveter的依赖

    ````xml
    <dependency>
        <groupId>org.jodconverter</groupId>
        <artifactId>jodconverter-local</artifactId>  
        <version>4.4.2</version>
    </dependency>
    ````

2. 新建一个工具类`JodConveterUtil`，用来提供我们Word转PDF的方法。

    ```java
    import org.jodconverter.core.office.OfficeException;
    import org.jodconverter.local.JodConverter;
    import org.jodconverter.local.office.LocalOfficeManager;
    
    import java.io.File;
    
    public class JodConveterUtil {
        /**
         * openoffice的安装目录，linux下默认为/opt/openoffice
         */
        static String officeHome = "/opt/openoffice4";
    
        /**
         * 启动OpenOffice使用的端口号，如果使用云服务器记得开端口权限
         */
        static int port = 8100;
        
        /**
         * 将源文件转换为目标文件的格式，此方法将自动进行识别文件类型，无需传入其他参数
         * @param source 源文件
         * @param target 目标文件
         * @throws OfficeException
         */
        public static synchronized void convert(File source, File target) throws OfficeException {
    
    
            LocalOfficeManager officeManager = LocalOfficeManager.builder()
                    .officeHome(officeHome)
                    .portNumbers(port)
                    .processTimeout(3000L)
                    .maxTasksPerProcess(4)
                    .install()
                    .build();
    		// 调用OpenOffice服务
            officeManager.start();
            JodConverter.convert(source).to(target).execute();
            // 使用完成关闭服务
            officeManager.stop();
        }
        // 此处做测试
        public static void main(String[] args) throws Exception{
            JodConveterUtil.convert(new File("C:\\Users\\Administrator\\Desktop\\压缩视频操作流程.docx"),new File("C:\\Users\\Administrator\\Desktop\\target.pdf"));
        }
    
    }
    ```

3. 该转换工具会根据文件后缀名自动识别文件类型并进行转换，所以传入的target文件的后缀名就是你要转换成的格式。

<font color="red">注：该工具类版本变动较大，最好还是看其提供的文档进行操作，此处给出的代码仅供当前版本作为参考。</font>

---

**我是SimpleZzz，一个正努力改变自己的打工人。**