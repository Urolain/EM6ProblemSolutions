## EMobile问题解决手册

### 1.启动方面

- 启动报错，无法启动
  1. 检查启动使用JDK路径是否正确；
  2. JDK版本使用错误：6.5及之前版本不能使用JDK8；6.6版本支持JDK8
  3. EMobile设置的相关端口被其他程序或服务占用，更改EMobile的相关端口即可
  
- 启动卡住，无反应或者启动过程极为缓慢
  1. 检查该EMobile对应的ecology服务是否是正常运行的
  2. 若是EMobile迁移之后启动出现此现象，应该是插件地址未修改导致。
  
- 启动之后访问网页端报500错误
  1. JDK版本使用错误，6.5及之前版本不能使用JDK8；6.6版本支持JDK8
  2. jar包错误，检查EMobile/lib目录以及EMobile/webapps/ROOT/WEB-INF/lib目录下jar包是否与备案环境一致
  3. 页面编译报错，修改EMobile/conf目录下的resin.conf文件，加入以下语句，其中D:\WEAVER\JDK是启动对应的jdk路径
  
  ```xml
  <javac compiler="D:\WEAVER\JDK\bin\javac" args="-encoding UTF-8"/>
  ```
  
- 启动之后访问网页端，提示128错误
  1. 这个是db文件损坏导致。重新初始化EMobile或者修复db文件数据。
  2. 之前EMobile服务未正常且正确停止，导致EMobile服务进程仍然存在，db文件被占用。杀死老进程之后再启动。
  
- 启动之后电脑端访问显示空白
  1. EMobile安全包升级错误导致。还原page文件夹即可
  2. messages.properties文件加载失败，确认目录EMobile/webapps/ROOT/WEB-INF/classes/messages.properties文件名称是否正确

### 2.后台设置方面

- 后台设置的欢迎页、登录页等相关图片显示无法显示
  
  1. 请检查目录EMobile\webapps\ROOT\uploadfile文件夹下是否存在设置的图片
  
- 点击应用，应用的详细设置页面不显示，显示为空白

  1. 检查已经添加的应用是否都在【应用中心】可以找到。若有的应用找不到，则可以通过【批量编辑应用】将不存在的应用删除即可
  2. 应用中心都可以找到已经添加的应用，则请检查是否有移动建模的应用在ecology后台下架或取消发布。找到对应的应用，通过【批量编辑应用】将不存在的应用删除即可

- 添加用户，显示ecology登录页面或者报122错误
  1. mobile过滤器未正确添加。请在目录WEAVER/ecology/WEB-INF/下的web.xml文件中检查加入以下内容
  ```xml
  <filter>
  	<filter-name>Mobile</filter-name>
  	<filter-class>weaver.mobile.plugin.ecology.MobileFilter</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>Mobile</filter-name>
  	<url-pattern>/mobile/plugin/*</url-pattern>
  </filter-mapping>
  ```


- 添加用户时提示license人数已满问题，但是看页数记录并未超出
  1. 检查每一个用户组中是否存在以【分部】【部门】等条件添加的用户对象
- 开启了移动考勤，并添加考勤点，但依然无法进行考勤
  1. 要使用移动考勤，除了要在EMobile开启考勤、添加考勤点，还要再ecology后台设置一般工作时间和考勤次数等。
- WiFi考勤中已经添加了MAC地址，但是连接到对应wifi下还是不能考勤
  1. wifi的MAC地址设置错误，请重新查看wifi对应的MAC地址，重新添加

### 3.APP登录出错方面

- 登录显示无法连接服务器
  1. 检查输入的服务器地址是否正确，是否是EMobile服务对应的地址
  2. 检查服务器中EMobile是否正常启动
- 登录提示用户名或密码错误，但是实际是正确的
  1. 检查该EMobile服务配置的插件地址是否是对应的ecology服务地址
- 登录提示修改密码
  1. 检查ecology后台是否开启了定期修改密码
- 登录提示007错误
  1. 007错误是EMobile的License过期了，需要在EMobile后台重新上传license文件
- 登录提示105错误
  1. db数据库文件损坏或存在无效数据导致
  2. 系统出现异常，需要检查代码
- 登录提示110错误
  1. 登录的用户没有被添加到EMobile用户组中，在EMobile后台添加之后重新登录即可
- 登录提示114错误
  1. 登录的用户绑定了设备，需要使用指定的设备登录
- 登录报122错误
  1. 检查EMobile安装的插件对应的ecology服务是否正常运行

### 4.应用使用方面

- 点击新建流程常报122错误
  1. 检查ecology/WEB-INF/web.xml文件中是否存在connfast过滤器，若存在注释掉即可
- 待办应用中显示的待办流程和pc端数目不一致
  1. EMobile后台，待办应用设置中，选择的流程不是全部。不勾选，默认是显示所有类别的待办；若有勾选，则只显示勾选了流程类别
- 已办应用中流程比实际少
  1. EMobile后台，已办应用设置中，未勾选【包含已办事宜】。不勾选，已办应用默认显示的是自己已处理但是别的节点还未处理的流程。
- 在app中没有显示后台设置的应用
  1. 检查对应的用户是否在该应用或者该应用所在的应用分组的【可访问用户组】中
- 通讯录中不显示【同部门】、【组织】等6个选项
  1. EMobile后台，检查系统设置，插件系统设置中，检查是否都是设置了显示
- 通讯录中显示的人员不全
  1. 重新同步人力资源
  2. 确认是否开启了分权
- 通讯录中人员显示的顺序问题
  1. EMobile支持三种顺序——【人力资源显示顺序】：通讯录则按照ecology中的顺序进行显示；【人员姓氏首字母】；【人员id】
- 找到一个人员之后，没有发送消息的按钮
  1. 检查检查ecology/WEB-INF/prop目录下的EMobileRong文件是否配置正确。公有云配置提供的正确的AppKey和AppSecret；私有云则将AppKey配置为 8w7jv5yu7ucqy即可。
- 我的收藏中不显示收藏的文档
  1. 收藏的文档是显示在文档应用中的收藏中，我的收藏显示的是收藏的消息等内容
- app不能修改头像，维护手机、电话、邮箱信息
  1. 代码禁止了，可以放开
- 没有修改密码等选项
  1. 代码禁止了，可以放开

- 切换主次账号失败
  1. 检查ecology/WEB-INF/prop目录下的EMobile4文件中serverUrl是否配置正确

### 5.消息及推送(公有云)方面

- 消息显示未连接，token为空，没有消息推送

  1. 检查ecology/WEB-INF/prop目录下的EMobileRong文件，AppKey和AppSecret是否正确配置
  2. 客户EMobile服务器访问以下两个公有云是否正常

  ```properties
  api.cn.ronghub.com
  api-cn.ronghub.com
  ```

- 没有横幅栏提醒

  1. 提供ecology/WEB-INF/prop目录下的EMobileRong文件以及测试账号给开发人员进行接口测试

- 归档、抄送没有消息推送

  1. 归档和抄送流程默认不推送，如需推送，修改ecology/WEB-INF/prop目录下的Mobile文件中archivingReminder为1

- 没有流程消息推送

  1. 检查ecology/WEB-INF/prop目录下EMobile4文件配置情况，EMobilePush需要设置为1，serverUrl需要配置为EMobile服务的地址；pushKey请从EMobile后台，系统状态，e-mobile消息推送密钥处获取
  2. 检查手机设置是否允许EMobile app消息通知
  3. 检查EMobile app设置——流程消息提醒中是否勾选了对应的流程
  4. 以上都检查无误，则提供ecology/WEB-INF/prop目录下的EMobileRong文件，推送对应日期的ecology日志（目录ecology/log）和EMobile日志（目录EMobile/webapps/ROOT/WEB-INF/logs）给开发人员排查

### 6.消息及推送(私有云)方面

- 消息显示未连接，token为空

  1. 检查私有云服务是否允许正常，访问以下地址，访问之后显示一个登录页面则是正常运行

  ```properties
  http://[私有云服务所在服务器ip] + : + 9090
  例如
  http://192.168.81.137:9090
  ```

  	2. 检查ecology/WEB-INF/prop目录下OpenfireModule文件配置是否正确。openfireEMobileUrl要配置EMobile服务可以访问到的私有云全地址。若私有云服务和EMobile服务在同一台服务器，可以配置http://127.0.0.1:9090；openfireMobileClientUrl要配置EMobile服务外网域名或ip
   	3. 检查5222端口是否映射。使用telnet指令，执行以下指令，执行之后按方向上键，出现【遗失对主机的连接。】等字样，说明端口已经

  ```bash
  telnet [openfireMobileClientUrl配置的地址] 5222
  例如
  telnet 192.168.81.137 5222
  ```

  4. 以上三点都是正常的，则按照以下次序重启服务：重启ecology服务、重启私有云服务、重启EMobile服务，然后再重新登录app测试

- 没有流程消息推送

  1. 检查ecology/WEB-INF/prop目录下EMobile4文件配置情况，EMobilePush需要设置为1，serverUrl需要配置为EMobile服务的地址；pushKey请从EMobile后台，系统状态，e-mobile消息推送密钥处获取
  2. 检查私有云服务是否允许正常
  3. 检查手机设置是否允许EMobile app消息通知
  4. 检查EMobile app设置——流程消息提醒中是否勾选了对应的流程
  5. 以上都检查无误，则提供推送对应日期的ecology日志（目录ecology/log），EMobile日志（目录EMobile/webapps/ROOT/WEB-INF/logs）和私有云日志（目录e-message/logs）给开发人员排查
  
### 7.其他

- mobile版本升级后（比如从5.0.6升级到6.5），下载页面显示还是以前的版本
  1. 需要修改EMobileVersion.properties文件。打开ecology/WEB-INF/prop/ EMobileVersion.properties，设置version=Ni4w

- 主次账号切换失败
  1. 打开ecology/WEB-INF/prop/ EMobile4.properties文件，检查serverUrl是否配置正确。

- 网页端登录EMobile显示为空白页面，但是app是可以正常登录
  1. 这种情况大概率是升级EMobile安全补丁包时没有匹配正确版本而导致的。如果有备份文件，将备份文件目录EMobile\webapps\ROOT\WEB-INF下的page文件夹替换当前使用page文件夹即可。如果没有备份，则联系开发人员处理

- 启动无报错，但是resin日志和mobile日志都没有生成，服务也并未真正启动起来
  1. 这个情况可能是EMobile所在服务器磁盘容量满了，导致日志没有成功打印。清理出空间之后，再启动尝试。
