# 安装
先安装ruby和ruby-dev
```sh
gem install fluentd
```
# 配置
## 生成
```sh
fluentd -s [目录]
```
会在目录（默认为/etc/fluent）下生成默认配置文件fluent.conf
## 输入
### tcp
```xml
<source>
  @type forward
</source>
```
```sh
echo json字符串 | fluent-cat [标签]
```
### http
```xml
<source>
  @type http
  port 端口
  bind IP地址，默认0.0.0.0
</source>
```
```
http://IP地址/[标签]?json=json字符串
```
## 过滤
链式过滤  
符号|匹配
-|-
*|1级标签
**|0或多级标签
{a,b}|a或b
#{...}|ruby表达式
### 按json键值排除
```xml
<filter [标签]>
  @type grep
  <exclude>
    key 键
    pattern 正则表达式
  </exclude>
</filter>
```
### 增加键值
```xml
<filter [标签]>
  @type record_transformer
  <record>
    键 值
  </record>
</filter>
```
## 输出
若match位于filter前，则filter对该match不起作用  
一个事件被match一次后不再继续
### stdout
非缓冲式输出
```xml
<match [标签]>
  @type stdout
</match>
```
### file
```xml
<match [标签]>
  @type file
  path 存储目录
</match>
```
## label
当配置增多时，顺序执行的source、filter、match将会很乱，可采用label将其组合
```xml
<source>
  ...
  @label @label名
</source>
<label @label名>
  <filter>
    ...
  </filter>
  <match>
    ...
  </match>
<label>
```
在&lt;label&gt;外的针对 含label的source 的filter将不生效
## 系统
```xml
<system>
  log_level error # fluentd日志级别
  process_name 名称 # ps -elf的CMD列显示supervisor:名称和worker:名称
</system>
```
## @include
导入其它配置
```xml
@include 相对、绝对路径或URL
```
## 使用环境变量
"#{ENV['变量名']}"  
运行时环境变量更改无效
# 启动
```sh
fluentd [-c 配置文件]
```
# 插件
fluentd依赖插件收集日志并推送  
收集插件命名为in_源，负责将日志转换为fluentd事件，包括标签（用于分类）、时间、记录（json格式）  
推送插件命名为out_目的
# 用例
## java
### fluent.conf
```xml
<source>
  @type forward
</source>
<match 前缀.**>
  ...
</match>
```
### pom.xml
```xml
<dependency>
  <groupId>org.fluentd</groupId>
  <artifactId>fluent-logger</artifactId>
  <version>0.3.3</version>
</dependency>
```
### .java
```java
import java.util.*;
import org.fluentd.logger.FluentLogger;

public class App {
    private static FluentLogger LOG = FluentLogger.getLogger(前缀);

    public static void main(String[] args) {
        Map<String, Object> data = new HashMap<String, Object>();
        data.put(键, 值);
        LOG.log(后缀, data);// 最终标签为 前缀.后缀
    }
}
```
### python
```sh
pip install fluent-logger
```
.py
```python
from fluent import sender
from fluent import event
sender.setup(前缀, host='localhost', port=24224)
event.Event(后缀, json字典)
```