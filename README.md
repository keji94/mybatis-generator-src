# 修改点
## 1.字段注释、getter、setter、DAO类接口注释
org.mybatis.generator.internal.DefaultCommentGenerator 类
## 2.增加实体类 类注释方法调用
org.mybatis.generator.codegen.mybatis3.model.BaseRecordGenerator 第64行
## 3.mapper、实体类 后缀修改
org.mybatis.generator.api.IntrospectedTable#calculateModelAttributes

    `sb.append("Entity");`
    
org.mybatis.generator.api.IntrospectedTable#calculateJavaClientAttributes

    `sb.append("Dao");`
    
## 4.DAO类 类注释增加
org.mybatis.generator.codegen.mybatis3.javamapper.JavaMapperGenerator#getCompilationUnits

    `commentGenerator.addJavaDaoComment(interfaze, introspectedTable);`

org.mybatis.generator.internal.DefaultCommentGenerator#addJavaDaoComment （新增）

## 5.DAO类新增方法selectDeleteByPk
org.mybatis.generator.codegen.mybatis3.javamapper.JavaMapperGenerator

    `addSelectDeleteByPkMethod(interfaze);` 103行

org.mybatis.generator.codegen.mybatis3.javamapper.JavaMapperGenerator#addSelectDeleteByPkMethod (新增)
org.mybatis.generator.codegen.mybatis3.javamapper.elements.SelectDeleteByPkMethodGenerator (新增)

## 6.xml文件 主键查询增加shanchubz or isDesl条件 and 增加 selectDeleteByPk sql element
org.mybatis.generator.codegen.mybatis3.xmlmapper.elements.SelectByPrimaryKeyElementGenerator

            //主键查询增加删除标志条件
            String isDelColumnName = "";
            List<String> cloumns = parentElement.getCloumns();
            if (anyMatch("SHANCHUBZ", cloumns)) {
                isDelColumnName = "SHANCHUBZ";
            }
            if (anyMatch("is_del", cloumns)) {
                isDelColumnName = "is_del";
            }

            sb.append(" and "+isDelColumnName+" = '1'");


org.mybatis.generator.api.dom.xml.XmlElement#getCloumns (新增)

org.mybatis.generator.codegen.mybatis3.xmlmapper.elements.SelectDeleteByPkElementGenerator (新增)

org.mybatis.generator.codegen.mybatis3.xmlmapper.XMLMapperGenerator#addSelectDeleteByPkElement (新增)
org.mybatis.generator.codegen.mybatis3.xmlmapper.XMLMapperGenerator#getSqlMapElement
    
    `addSelectDeleteByPkElement(answer);`
    
   
# 使用方式
1.使用[客户端工程] (http://git.ichoice.cc/tool/mybatis-generator-client)

2.使用外部配置文件，直接运行jar包

    a.打开项目，使用mvn clean package 命令或idea maven 插件 打包。在target 包中找到mybatis-generator-1.3.5-plus.jar

    b.将mybatis-generator-1.3.5-plus.jar 和 generatorConfig.xml放在同一目录

    c.键入java -jar mybatis-generator-1.3.5-plus.jar -configfile generatorConfig.xml -overwrite 即可。

    注意：如果生成文件的目录不在工程中可能会出现乱码。
 
    