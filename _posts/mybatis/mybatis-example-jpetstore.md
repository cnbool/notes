title: 'MyBatis Example:JPetStore'
categories:
  - mybatis
date: 2013-08-14 00:28:38
tags:
---

http://code.google.com/p/mybatis/下载JPetStore,了解下MyBatis怎么使用.
JPetStore源代码包结构大体如下:
--domain
--persistence
--service
--web

说明:
domain: 只包含属性和get,set方法的bean对象
persistence: 包含数据访问接口,类名字以Mapper结尾
service: 业务逻辑层
web: action层

spring配置信息:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Copyright 2010 The myBatis Team

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
-->

<beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xmlns:tx="http://www.springframework.org/schema/tx"
     xmlns:jdbc="http://www.springframework.org/schema/jdbc"
     xmlns:context="http://www.springframework.org/schema/context"
     xsi:schemaLocation="
     http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd
     http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
     http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-3.0.xsd
     http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
     http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">

   <!-- in-memory database and a datasource -->
    <jdbc:embedded-database id="dataSource">
        <jdbc:script location="classpath:database/jpetstore-hsqldb-schema.sql"/>
        <jdbc:script location="classpath:database/jpetstore-hsqldb-dataload.sql"/>
    </jdbc:embedded-database>

    <!-- transaction manager, use JtaTransactionManager for global tx -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!-- enable component scanning (beware that this does not enable mapper scanning!) -->    
    <context:component-scan base-package="org.mybatis.jpetstore.service" />

    <!-- enable autowire -->
    <context:annotation-config />

    <!-- enable transaction demarcation with annotations -->
    <tx:annotation-driven />

    <!-- define the SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="typeAliasesPackage" value="org.mybatis.jpetstore.domain" />
    </bean>

    <!-- scan for mappers and let them be autowired -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="org.mybatis.jpetstore.persistence" />
    </bean>
</beans>
```

52-55: sqlSessionFactory有两个属性dataSource和domain包名
58-60: MapperScannerConfigurer类的basePackage属性指定了Mapper所在包,即persistence包

** Mapper: **
org.mybatis.jpetstore.persistence.AccountMapper

```java
package org.mybatis.jpetstore.persistence;

import org.mybatis.jpetstore.domain.Account;

public interface AccountMapper {

  Account getAccountByUsername(String username);

  Account getAccountByUsernameAndPassword(Account account);

  void insertAccount(Account account);

  void insertProfile(Account account);

  void insertSignon(Account account);

  void updateAccount(Account account);

  void updateProfile(Account account);

  void updateSignon(Account account);

}
```

** Mapper.xml: **
orgmybatisjpetstorepersistenceAccountMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.mybatis.jpetstore.persistence.AccountMapper">

  <cache />

  <select id="getAccountByUsername" parameterType="string" resultType="Account">
    SELECT
          SIGNON.USERNAME,
          ACCOUNT.EMAIL,
          ACCOUNT.FIRSTNAME,
          ACCOUNT.LASTNAME,
          ACCOUNT.STATUS,
          ACCOUNT.ADDR1 AS address1,
          ACCOUNT.ADDR2 AS address2,
          ACCOUNT.CITY,
          ACCOUNT.STATE,
          ACCOUNT.ZIP,
          ACCOUNT.COUNTRY,
          ACCOUNT.PHONE,
          PROFILE.LANGPREF AS languagePreference,
          PROFILE.FAVCATEGORY AS favouriteCategoryId,
          PROFILE.MYLISTOPT AS listOption,
          PROFILE.BANNEROPT AS bannerOption,
          BANNERDATA.BANNERNAME
    FROM ACCOUNT, PROFILE, SIGNON, BANNERDATA
    WHERE ACCOUNT.USERID = #{username}
      AND SIGNON.USERNAME = ACCOUNT.USERID
      AND PROFILE.USERID = ACCOUNT.USERID
      AND PROFILE.FAVCATEGORY = BANNERDATA.FAVCATEGORY
  </select>

  <select id="getAccountByUsernameAndPassword" parameterType="Account" resultType="Account">
    SELECT
      SIGNON.USERNAME,
      ACCOUNT.EMAIL,
      ACCOUNT.FIRSTNAME,
      ACCOUNT.LASTNAME,
      ACCOUNT.STATUS,
      ACCOUNT.ADDR1 AS address1,
      ACCOUNT.ADDR2 AS address2,
      ACCOUNT.CITY,
      ACCOUNT.STATE,
      ACCOUNT.ZIP,
      ACCOUNT.COUNTRY,
      ACCOUNT.PHONE,
      PROFILE.LANGPREF AS languagePreference,
      PROFILE.FAVCATEGORY AS favouriteCategoryId,
      PROFILE.MYLISTOPT AS listOption,
      PROFILE.BANNEROPT AS bannerOption,
      BANNERDATA.BANNERNAME
    FROM ACCOUNT, PROFILE, SIGNON, BANNERDATA
    WHERE ACCOUNT.USERID = #{username}
      AND SIGNON.PASSWORD = #{password}
      AND SIGNON.USERNAME = ACCOUNT.USERID
      AND PROFILE.USERID = ACCOUNT.USERID
      AND PROFILE.FAVCATEGORY = BANNERDATA.FAVCATEGORY
  </select>

  <update id="updateAccount" parameterType="Account">
    UPDATE ACCOUNT SET
      EMAIL = #{email},
      FIRSTNAME = #{firstName},
      LASTNAME = #{lastName},
      STATUS = #{status},
      ADDR1 = #{address1},
      ADDR2 = #{address2,jdbcType=VARCHAR},
      CITY = #{city},
      STATE = #{state},
      ZIP = #{zip},
      COUNTRY = #{country},
      PHONE = #{phone}
    WHERE USERID = #{username}
  </update>

  <insert id="insertAccount" parameterType="Account">
    INSERT INTO ACCOUNT
      (EMAIL, FIRSTNAME, LASTNAME, STATUS, ADDR1, ADDR2, CITY, STATE, ZIP, COUNTRY, PHONE, USERID)
    VALUES
      (#{email}, #{firstName}, #{lastName}, #{status}, #{address1},  #{address2,jdbcType=VARCHAR}, #{city}, #{state}, #{zip}, #{country}, #{phone}, #{username})
  </insert>

  <!--  
  TODO MyBatis 3 does not map booleans to integers
  <update id="updateProfile" parameterType="Account">
    UPDATE PROFILE SET
      LANGPREF = #{languagePreference},
      FAVCATEGORY = #{favouriteCategoryId},
      MYLISTOPT = #{listOption},
      BANNEROPT = #{bannerOption}
    WHERE USERID = #{username}
  </update>
  -->

  <update id="updateProfile" parameterType="Account">
    UPDATE PROFILE SET
      LANGPREF = #{languagePreference},
      FAVCATEGORY = #{favouriteCategoryId}
    WHERE USERID = #{username}
  </update>

  <!--  
  TODO MyBatis 3 does not map booleans to integers
  <insert id="insertProfile" parameterType="Account">
    INSERT INTO PROFILE (LANGPREF, FAVCATEGORY, MYLISTOPT, BANNEROPT, USERID)
    VALUES (#{languagePreference}, #{favouriteCategoryId}, #{listOption}, #{bannerOption}, #{username})
  </insert>
  -->

  <insert id="insertProfile" parameterType="Account">
    INSERT INTO PROFILE (LANGPREF, FAVCATEGORY, USERID)
    VALUES (#{languagePreference}, #{favouriteCategoryId}, #{username})
  </insert>

  <update id="updateSignon" parameterType="Account">
    UPDATE SIGNON SET PASSWORD = #{password}
    WHERE USERNAME = #{username}
  </update>

  <insert id="insertSignon" parameterType="Account">
    INSERT INTO SIGNON (PASSWORD,USERNAME)
    VALUES (#{password}, #{username})
  </insert>

</mapper>
```

** service: **
org.mybatis.jpetstore.service.AccountService

```java
package org.mybatis.jpetstore.service;

import org.mybatis.jpetstore.domain.Account;
import org.mybatis.jpetstore.persistence.AccountMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class AccountService {

  @Autowired
  private AccountMapper accountMapper;

  public Account getAccount(String username) {
    return accountMapper.getAccountByUsername(username);
  }

  public Account getAccount(String username, String password) {
    Account account = new Account();
    account.setUsername(username);
    account.setPassword(password);
    return accountMapper.getAccountByUsernameAndPassword(account);
  }

  @Transactional
  public void insertAccount(Account account) {
    accountMapper.insertAccount(account);
    accountMapper.insertProfile(account);
    accountMapper.insertSignon(account);
  }

  @Transactional
  public void updateAccount(Account account) {
    accountMapper.updateAccount(account);
    accountMapper.updateProfile(account);

    if (account.getPassword() != null && account.getPassword().length() > 0) {
      accountMapper.updateSignon(account);
    }
  }

}
```
