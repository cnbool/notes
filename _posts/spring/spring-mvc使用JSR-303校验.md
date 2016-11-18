title: spring mvc使用JSR-303校验
categories:
  - spring
date: 2015-07-02 18:59:04
tags:
---

### Maven配置 ###

添加JSR-303 API和Hibernate实现

```xml
    <dependency>
        <groupId>javax.validation</groupId>
        <artifactId>validation-api</artifactId>
        <version>1.1.0.Final</version>
    </dependency>
    <dependency>
       <groupId>org.hibernate</groupId>
       <artifactId>hibernate-validator</artifactId>
       <version>5.1.3.Final</version>
    </dependency>
```
    

### Bean属性上添加JSR-303注解 ###

```java
    public class BlacklistInfo {
        @Pattern(regexp="^(13[0-9]|15[012356789]|18[02356789]|14[57])[0-9]{8}$", message="请填写正确的电话号码")
        private String phonenum;
        @Max(value=20, message="备注长度过长")
        private String remark;

        //..
    }
```

 ### Bean Validation 中的 constraint ###

    Constraint  详细信息

    @Null   被注释的元素必须为 null

    @NotNull    被注释的元素必须不为 null

    @AssertTrue 被注释的元素必须为 true

    @AssertFalse    被注释的元素必须为 false

    @Min(value) 被注释的元素必须是一个数字，其值必须大于等于指定的最小值

    @Max(value) 被注释的元素必须是一个数字，其值必须小于等于指定的最大值

    @DecimalMin(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值

    @DecimalMax(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值

    @Size(max, min) 被注释的元素的大小必须在指定的范围内

    @Digits (integer, fraction) 被注释的元素必须是一个数字，其值必须在可接受的范围内

    @Past   被注释的元素必须是一个过去的日期

    @Future 被注释的元素必须是一个将来的日期

    @Pattern(value) 被注释的元素必须符合指定的正则表达式

    Hibernate Validator 附加的 constraint

    Constraint  详细信息

    @Email  被注释的元素必须是电子邮箱地址

    @Length 被注释的字符串的大小必须在指定的范围内

    @NotEmpty   被注释的字符串的必须非空

    @Range  被注释的元素必须在合适的范围内

 ### Controller配置 ###

```java
    @RequestMapping(value = "/add", method = RequestMethod.POST)
    public String add(@Valid BlacklistInfo blacklistInfo, Errors errors, ModelMap map) {
        if (errors.hasErrors()) {
            List<ObjectError> list = errors.getAllErrors();
            StringBuilder sb = new StringBuilder();
            for (ObjectError err : list) {
                sb.append(err.getDefaultMessage()).append("<br/>");
            }
            map.put("info", sb.toString());
        }
    }
```

Bean前的@ModelAttribute替换为@Valid,error包含验证后的错误列表

ObjectError.getDefaultMessage()获得默认提示信息，即注解中message的内容
