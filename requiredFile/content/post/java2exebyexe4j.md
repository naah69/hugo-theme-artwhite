---
title: "java程序转为EXE，无需安装JAVA环境运行程序"
subtitle: ""
description: "通过exe4j打包jar为exe"
date: 2017-08-08
author: 那焱
image: ""
tags: ["exe4j"]
categories: ["JAVA"]
---

昨日在Google查资料的时候，需要使用ss，但是经常改ss配置好麻烦（穷逼使用免费的ss配置要经常改），毕竟我这么懒，于是着手开始写代码。（现已更新到c#软件）

写完之后因为要使用cmd命令行运行类，而我写的代码中使用了jsoup这个类库，于是要生成jar包运行。

由于eclipse的导出jar包不能带外部jar包，于是遇到了今天要使用的第一个工具net.sf.fjep.fatjar插件，下面进入正题。

# 1 生成Jar包
（net.sf.fjep.fatjar插件下载）

### 1.1 安装插件：
1. 解压这个zip文件包

2. 把插件文件夹中的net.sf.fjep.fatjar_0.0.27文件夹复制到Eclipse中插件文件夹下。
（有的时候复制在这个文件夹中会不好使，那就复制在dropins里，并把配置文件夹下的org.eclipse.update文件夹删除）

3. 重启Eclipse，并且右击Java项目，在菜单栏中会看到Build Fat Jar，这就说明配置成功

### 1.2 生成Jar：
1. 右键点击要生成的Java项目，选择Build Fat Jar。

2. 在弹出的窗口中，做如下操作，输入名称名称，选择主方法，勾选One-JAR，点击下一页

3. 勾选所有你在项目中引用的第三方jar包，点击完成。可以在项目下生成jar包

4. 使用java -jar命令测试jar文件是否可以正常运行。如果可以正常运行，那么请继续往下看。

# 2 精简JRE
### 2.1 精简JRE \ BIN
1. 在你的java代码中最后结束的位置假如如下代码

```
InputStreamReader ir = new InputStreamReader(System.in);
System.out.println("hello");
ir.read();
```

2. 编译你的代码，并使用命令行的java代码运行类

3. 这个时候命令行会执行你的代码，并且最后停留在输入字符界面，使用管理员权限打开ProcessExplorer（点击下载）。

4. 点击ProcessExplorer的标题栏的视图 - 显示下排窗口。

5. 点击ProcessExplorer的标题栏的视图 - 下排窗口显示内容 - 动态链接库。

6. 点击列表中中cmd.exe下的java.exe。

7. 记录下排窗口中所有带有jre的dll文件路径，关闭ProcessExplorer与cmd命令行窗口。

8. 将系统JAVA环境中的jre（100M +）复制到到我们刚刚生成的jar的目录下（jre文件夹与jar文件同目录）

9. 将我们刚刚记录的dll文件与java。exe 与服务器文件夹下的jvm.dll保留，其他文件全部删除
（我这个版本为jre7，其他版本没有测试）

10. 运行cmd命令行使用本目录下java.exe（非环境变量文件夹中的java.exe）测试我们的类

11. 如果成功运行即代表bin精简成功，此时的bin文件夹应该由原本的50M +变为10M左右，可以将文件删除并将源码中的上文代码删除。

### 2.2 精简JRE \ LIB
1. 在jar包的目录下建立txt文件，复制下方代码，将代码中jar包的名称改为你的jar包的名称，然后将txt扩展名改为bat。

```
java -jar -verbose：class ss.jar >> classlog.txt
```

2. 打开刚才建立的蝙蝠，等待命令行窗口消失后，打开jar包目录下classlog.txt。查看是否有运行日志，如果有运行日志请继续往下看。

3. 将下列代码修改source,dest,logname,jarArr,建立相应文件夹后编译运行。

```
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.Charset;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;

public class sm {

 private String source="C:/Users/Naah/Desktop/ss/jre7/lib";
 private String dest="C:/Users/Naah/Desktop/ss/jre7/lib";
 private static String logname="C:/Users/Naah/Desktop/ss/classlog.txt";
 private String[] jarArr=new String[]{"rt","charsets"} ;
 /***
 *
 * @param source 类源目录
 * @param dest 类拷贝目的目录
 * @param dest classlog的路径
 * @param jarArr 需要的提取的jar文件
 */
 public static void main(String[] args) {
 // TODO Auto-generated method stub

 try {
 List list=finJar(logname,"ss.jar");
 InputStreamReader ir = new InputStreamReader(System.in);
 System.out.println("请清理lib目录并解压相关jar包,输入任何字符继续！");
 ir.read();
 String a="";
 for (String string : list) {
 a=a+"\n"+string;
 }
 a=a.substring(1,a.length());
 FileOutputStream out=new FileOutputStream(new File(logname));
 out.write(a.getBytes());
 out.flush();
 out.close();

 sm obj = new sm();
 obj.readAndCopy(logname);
 } catch (IOException e) {
 e.printStackTrace();
 }

 }


 public static List finJar(String txtPath,String jarname) throws IOException{
 List list=new ArrayList<>();
 List jar=new ArrayList<>();
 List a=Files.readAllLines(Paths.get(txtPath), Charset.forName("GBK"));
 for (String s : a) {
// if(s.contains("ss.jar")==false&&!list.contains(s.substring(s.length()-16,s.length())))
// list.add(s.substring(s.length()-16,s.length()));
 try{
 if(s.contains(jarname)==false&&!jar.contains(s.substring(s.lastIndexOf('\\')+1, s.lastIndexOf(']'))))
 jar.add(s.substring(s.lastIndexOf('\\')+1, s.lastIndexOf(']')));


 if(s.contains(jarname)==false&&!list.contains(s.substring(s.indexOf(' ')+1, s.indexOf("from")-1)))
 list.add(s.substring(s.indexOf(' ')+1, s.indexOf("from")-1).replace('.', '/'));
 }
 catch(Exception e)
 {continue;}

//
 }
 for (String string : jar) {
 System.out.println(string);
 }
 return list;
 }

 public void readAndCopy(String logName)
 {
 int count = 0; // 用于记录成功拷贝的类数
 try
 {
 FileInputStream fi = new FileInputStream(logName);
 InputStreamReader ir = new InputStreamReader(fi);
 BufferedReader br = new BufferedReader(ir);

 String string = br.readLine();
 while(string != null)
 {
 if(copyClass(string) == true)
 count++;
 else
 System.out.println("ERROR " + count + ": " + string);
 string = br.readLine();
 }
 }
 catch (IOException e)
 {
 System.out.println("ERROR: " + e);
 }
 System.out.println("count: " + count);
 }

 /***
 * 从原jar路径提取相应的类到目标路径，如将java/lang/CharSequence类从rt目录提取到rt1目录
 * @param string 提取类的全路径
 * @return
 * @throws IOException
 */
 public boolean copyClass(String string) throws IOException
 {
 String classDir = string.substring(0,string.lastIndexOf("/"));
 String className = string.substring(string.lastIndexOf("/")+1,string.length()) + ".class";

 boolean result =false;

 for(String jar : jarArr){
 File srcFile = new File(source + "/"+jar+"/" + classDir + "/" + className);
 if(!srcFile.exists())
 {
 continue;
 }

 byte buf[] = new byte[256];
 FileInputStream fin = new FileInputStream(srcFile);

 /* 目标目录不存在,创建 */
 File destDir = new File(dest + "/"+jar+"1/" + classDir);
 if(!destDir.exists())
 destDir.mkdirs();

 File destFile = new File(destDir + "/" + className);
 FileOutputStream fout = new FileOutputStream(destFile);
 int len = 0;
 while((len = fin.read(buf)) != -1)
 {
 fout.write(buf,0,len);
 }
 fout.flush();
 result = true;
 break;
 }
 return result;
 }

}
```

4. 在运行时，将lib目录下除了提示的jar包与amd64文件夹下的jvm.cfg保留，其他全部删除。

5. 将提示的jar包以文件夹方式解压到lib目录下。

6. 输入任何字符回车继续执行。

7. 提示数量并且没有提示任何错误则为精简成功。

8. 将在我这下载的rt.jar(点我下载)和charsets.jar(点我下载)解压覆盖到你的rt1和charsets1文件夹中。

9. 删除lib目录下原来的jar包，将带1的文件夹压缩为.zip格式，并重命名将1去掉，将.zip改为.jar即可完成。

# 3 生成exe文件
exe4j_windows-x64_5_0_1.zip（点我下载）

### 3.1 生成exe文件
1. 解压破解exe4j，否则生成后有提示没注册（License Information框为注册）,然后点击Next。

2. 只有一个jar文件选AR IN EXE。如果即将包装的程序中还包含目录或者非jar文件选择使用Regular mode,然后点击Next。

3. Application info 设置程序名称和EXE文件的输出目录（EXE与jar和JRE同目录）,然后点击Next。

4. Executable info 设置程序名称，可选择性的选择exe图标（Icon）。

5. 点击左侧32-bit or 64-bit,勾选此选项可减少错误发生率，然后点击左侧Java invocation。

6. 点击绿色加号，选择Archive，点击下方。。。按钮选择要封装的jar,然后点击OK,然后选择Main class,然后点击Next。

7. 设置需要的JRE版本，若没有版本最高限制保持其为空即可，然后点击左侧Search sequence。

8. 选中框中内容，多次点击红叉删除干净后，点击绿色加号。

9. 选择Directory，然后点击。。。选择jar目录下的我们精简过的JRE,然后点击左侧Splash screen。

10. 可选择性的设置程序的启动画面，然后点击左侧Message设置各种错误信息,然后点击Next生成EXE。

11. 点击目录下的EXE查看是否成功运行！如果没有使用GUI的话是没有任何提示的！

12. 如果成功运行的话，就可以将jar删除！将exe与Jre压缩到一个压缩包进行传输！

好了，教程到了这里就结束了，如果你已经成功来了，快把你的程序发送给你的小伙伴们使用吧！
