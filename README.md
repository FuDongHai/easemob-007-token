# Chat App Server

## Introduction

This repository contains sample project for chat app server, which include user registration and login.
When you plan to use agora chat service, you may need mapping your user profile with agora chat account and generate token for chat account. This project demonstrate how to create chat account and map to your own user profile and generate the token for chat account


* workflow for create account

<img width="871" alt="register" src="https://user-images.githubusercontent.com/15087647/171544887-39f2a52e-0c3d-400c-b764-79977c73054a.png">
---

* workflow for login

<img width="871" alt="login" src="https://user-images.githubusercontent.com/15087647/171544896-080971fd-81aa-42ad-bc30-35e9bf7cb78e.png">
---

## Features

- App Server support user registration and will create a chat account and map it to the user, will generate an agoraUid at the same time for possible [RTC service](https://docs.agora.io/cn/Voice/landing-page).
- App Server support user login and generate a token for chat service(via server SDK with aogra appId, appcert, chat account).
- App Server support store user information with database, which include user ID, user password, chat account(with uuid) and agora Uid.


## Technical

This project developed based on Spring Boot.

* [Spring Boot](https://spring.io/projects/spring-boot)

## Component

* [Server SDK](https://docs-im.easemob.com/ccim/rest/javaserversdk#java_server_sdk)
* MySQL

## Prepare

Before start, you need prepare agora chat appkey, agora AppId and agora AppCert.

* Setup aogra chat and get the AppKey：
  - Please login agora developer console, you can reference the link for detail. [Here](https://docs-preprod.agora.io/en/agora-chat/enable_agora_chat?platform=RESTful)

* You need setup your auth mechanism for your own user profile.

## Configure

Configure the below file with appkey, AppId and AppCert you get from the above steps.

* Configure file is：[application.properties](./agora-app-server/src/main/resources/application.properties)

  ```
      ## configure with your own appkey
      application.appkey=xxx
      
      ## configure with your own appid
      application.agoraAppId=xxx
      ## config with your own appcert
      application.agoraCert=xxx
      ## token valid duration(suggest not over one day)
      agora.token.expire.period.seconds=86400
      
      ## data source
      spring.datasource.driver-class-name=com.mysql.jdbc.Driver
      spring.datasource.url=jdbc:mysql://127.0.0.1:3306/app_server?useSSL=false&useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true
      spring.datasource.username=root
      spring.datasource.password=123456
      spring.datasource.hikari.maximum-pool-size=50
      spring.datasource.hikari.minimum-idle=20
  
      ## jpa
      spring.jpa.show_sql=false
      spring.jpa.properties.hibernate.format_sql=true
      spring.jpa.properties.hibernate.generate_statistics=false
      spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL57Dialect
      spring.jpa.hibernate.ddl-auto=validate
      
  ```

* Update config params in [ApplicationConfig](./src/main/java/com/easemob/agora/config/ApplicationConfig.java) file.

## Run

When you finish the configure, you can just run this app server.

## API

### register user


This api is used to register a user for your app. User name and password is used in this sample project, you can use any other format for your user account ,such as phone number.

**Path:** `http://localhost:8080/app/user/register`

**HTTP Method:** `POST`

**Request Headers:** 

| Param        | description      |
| ------------ | ---------------- |
| Content-Type | application/json |

**Request Body example:** 
{"userAccount":"jack", "userPassword":"123"}

**Request Body params:** 

| Param        | Data Type | description   |
| ------------ | --------- | ------------- |
| userAccount  | String    | user account  |
| userPassword | String    | user password |


**request example:**

```
curl -X POST -H 'Content-Type: application/json' 'http://localhost:8080/app/user/register' -d '{"userAccount": "jack","userPassword":"123"}'
```

**Response Parameters:**

| Param           | Data Type | description          |
| --------------- | --------- | -------------------- |
| code            | String    | response status code |
| easemobUserName | String    | chat user id         |
| agoraUid        | Integer   | agora uid            |

**response example:**

```json
{
    "code": "RES_OK",
    "easemobUserName": "em1792190072",
    "agoraUid": "1792190072"
}
```

---

### User Login

User login on your app server and get a agora token for chat service.

**Path:** `http://localhost:8080/app/user/login/{userAccount}`

You need setup {userAccount} value in your url.

**HTTP Method:** `POST`

**request example:**

```
curl -X POST 'http://localhost:8080/app/user/login/jack'
```

**Response Parameters:**

| Param           | Data Type | description                |
| --------------- | --------- | -------------------------- |
| code            | String    | response status code       |
| accessToken     | String    | token                      |
| expireTimestamp | Long      | timestamp for token expire |
| easemobUserName | String    | chat user id               |
| agoraUid        | Integer   | agora uid                  |

**response example:**

```json
{
    "code": "RES_OK",
    "accessToken": "xxx",
    "expireTimestamp": 1628245967857,
    "easemobUserName": "em1792190072",
    "agoraUid": "1792190072"
}
```
