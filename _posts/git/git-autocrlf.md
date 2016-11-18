title: git-autocrlf
categories:
  - git
date: 2015-08-20 16:36:44
tags:
---
## LF和CRLF

### Windows \r\n是回车换行 CRLF
### Linux \n是换行 LF

## AutoCRLF

### 提交时转换为LF，检出时转换为CRLF

```
git config --global core.autocrlf true   
```
### 提交时转换为LF，检出时不转换

```
git config --global core.autocrlf input   
```

### 提交检出均不转换

```
git config --global core.autocrlf false
```

## SafeCRLF

### 拒绝提交包含混合换行符的文件

```
git config --global core.safecrlf true 
```

### 允许提交包含混合换行符的文件

```
git config --global core.safecrlf false   
```

### 提交包含混合换行符的文件时给出警告

```
git config --global core.safecrlf warn
```