# javayh-data-soure 说明

## 使用方式
### 1.将javayh-boot-data-source作为项目模块
- 通过mavem进配置
使用本方法,将讲项目中的settings.xml文件内容放置到USER_HOME/.m2/settings.xml中

````
     <dependency>
          <groupId>com.javayh</groupId>
          <artifactId>javayh-boot-data-source</artifactId>
          version>1.0.0.RELEASE</version>
      </dependency>
````
- 配置文件
````
   spring:
      datasource:
        dynamic:
          enable: true #开启多数据源，如果不开启多数据源，改为false，
        druid:
          # JDBC 配置(驱动类自动从url的mysql识别,数据源类型自动识别)
          core:  #关闭多数据时请去掉，走正常的配置即可
            url: jdbc:mysql://localhost:3306/db1?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
            username: root
            password: root
            driver-class-name:  com.mysql.jdbc.Driver
          follower:
            url: jdbc:mysql://localhost:3306/db2?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
            username: root
            password: root
            driver-class-name:  com.mysql.jdbc.Driver
````
 
### 数据源切换使用

        public void get(){
            DataSourceHolder.setDataSourceKey(DataSourceKey.core);
            SysMenu sysMenu = menuMapper.findMenu("用户管理界面");
            DataSourceHolder.setDataSourceKey(DataSourceKey.follower);
            SysUser sysUser = userMapper.selectUserByName("admin");
            log.info("sysMenu ={}",sysMenu);
            log.info("sysUser ={}",sysUser);
        }
        
### 获取指定数据源

     @DataSource(name = "core")
     public void get(){
         SysMenu sysMenu = menuMapper.findMenu("用户管理界面");
         log.info("sysMenu ={}",sysMenu);
     }         
