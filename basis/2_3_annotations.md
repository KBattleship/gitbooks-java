# 2-3.Java注解

#### 1.基本语法

首先是注解的声明方式如下
```java
// @Target注解传入ElementType指明Nullable的作用域
@Target({ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD})
// @Retention注解用来指定其声明周期为运行时
@Retention(RetentionPolicy.RUNTIME)
// @Documented指定Javadoc进行记录此对象
@Documented
// @Nonnull为了告诉编译器这个域不可能为空
@Nonnull(when = When.MAYBE)
// @TypeQualifierNickname为类型限定符别称(备注部分具体解释此注解)
@TypeQualifierNickname
public @interface Nullable {
  
 // @interface 声明注解
  
}
```

#### 2.JDK元注解

