# MyBatis Generator Lombok plugin and Comment

## 实现的功能

- 主要整合了lombok插件实现getter/setter等通用方法的自动生成，同时自定义实现了一个注释生成器，
通过抓取数据库表里面的注释作为实体类的注释内容。

## 插件的用法

- 如果你想在你的maven中使用,就直接`git clone`这个项目到你的IDEA，然后使用`maven clean install`将这个项目添加到Maven仓库里去。
之后你只要在你的要使用这个插件的项目的pom.xml中加入如下内容便可：

```xml

<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <overwrite>true</overwrite>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>com.chrm</groupId>
            <artifactId>mybatis-generator-lombok-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</plugin>

```

同时添加配置文件`generatorConfig.xml`,使用的时候请根据项目需要自行修改对应配置

```xml


<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<!-- 配置生成器 -->
<generatorConfiguration>

    <!--classPathEntry:数据库的JDBC驱动,换成你自己的驱动位置 可选 -->
    <classPathEntry location="D:\MySQL\mysql-connector-java-5.1.47.jar"/>

    <!-- 一个数据库一个context,defaultModelType="flat" 大数据字段，不分表 -->
    <context id="MysqlTables" targetRuntime="MyBatis3Simple" defaultModelType="flat">

        <!-- 自动识别数据库关键字，默认false，如果设置为true，根据SqlReservedWords中定义的关键字列表；一般保留默认值，遇到数据库关键字（Java关键字），使用columnOverride覆盖 -->
        <property name="autoDelimitKeywords" value="true"/>

        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="utf-8"/>

        <!-- 带上序列化接口 -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
        <!-- 自定义的注释生成插件-->
        <plugin type="com.chrm.mybatis.generator.plugins.CommentPlugin">
            <!-- 抑制警告 -->
            <property name="suppressTypeWarnings" value="true" />
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="false" />
            <!-- 是否生成注释代时间戳-->
            <property name="suppressDate" value="true" />
        </plugin>
        <!-- 整合lombok-->
        <plugin type="com.chrm.mybatis.generator.plugins.LombokPlugin" >
            <property name="hasLombok" value="true"/>
        </plugin>


        <!-- 注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/><!-- 是否取消注释 -->
            <property name="suppressDate" value="false"/> <!-- 是否生成注释代时间戳-->
        </commentGenerator>

        <!-- jdbc连接-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://192.168.0.135:3306/xfs_store?serverTimezone=UTC"
                        userId="dev_write"
                        password="123456"/>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!-- 是否使用bigDecimal， false可自动转化以下类型（Long, Integer, Short, etc.） -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="com.xfs.mybatis.entity" targetProject="src/main/java">
            <!-- 是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值去掉前后空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成map.xml文件存放地址 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- 生成接口dao -->
        <javaClientGenerator targetPackage="com.xfs.mybatis.dao" targetProject="src/main/java" type="XMLMAPPER">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- table可以有多个,每个数据库中的表都可以写一个table，tableName表示要匹配的数据库表,也可以在tableName属性中通过使用%通配符来匹配所有数据库表,只有匹配的表才会自动生成文件 enableSelectByPrimaryKey相应的配置表示是否生成相应的接口 -->
        <table tableName="app_wheel_area" enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"
               enableSelectByPrimaryKey="true" enableUpdateByPrimaryKey="true"
               enableDeleteByPrimaryKey="true">
            <property name="useActualColumnNames" value="true"/>
        </table>

    </context>
</generatorConfiguration>



```

## Author
- GuoGuiRong 你的孤独 虽败犹荣

