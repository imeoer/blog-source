title: 简洁的自动化任务构建工具
date: 2015-05-17 22:00:00
author: me
tags:
    - 开发
    - GO语言
draft: false
preview: Build是一个使用GO语言编写的自动化任务构建工具，可根据描述配置执行任务。类似于JavaScript中的Grunt与Gulp工具，但更加简单，不需要编写复杂的逻辑代码

---

## 简介

Build是一个使用GO语言编写的自动化任务构建工具，可根据描述配置执行任务。类似于JavaScript中的Grunt与Gulp工具，但更加简单，不需要编写复杂的逻辑代码。

## 简单示例

以下例子描述了当web目录(/path/web)下任意.go文件发生变化时，自动执行名为build的任务，即切换到web目录下执行"go build"命令。

``` yaml
variable:
    web: "/path/web"
task:
    build:
        - "cd ${web} && go build"
watch:
    ${web}/*.go: "${build}"
```

## 介绍

Build使用YAML格式的配置文件来描述任务，分为**变量**(variable)，**任务**(task)，**监控**(watch)三个字段。

### 变量(variable)

变量定义了需要在任务或监控字段中可替换的字符串，变量中也可引用其他变量。

### 任务(task)

任务是按顺序执行的Shell命令，或引用的其他任务。若在引用的任务名前加```#```，表示该任务是非阻塞的，Build不会等待该任务完成，而是继续执行后面的任务。

### 监控(watch)

当任意文件发生变化时，执行任务中定义的任务，其中可以引用变量。

## 复杂示例

定义了两个变量，其中path1是/a/b，path2则是/a/b/c

定义了三个任务，其中default是默认任务，```build```命令会执行该任务，而```build develop```与```build commit```命令则分别执行develop与commit任务。default任务中，会并发执行develop与commit任务

定义了一个监控，当/a/b/c/*.go文件发生变化时，执行develop任务

``` yaml
variable:
    path1: "/a/b"
    path2: "${path1}/c"
task:
    default:
        - "${#develop}"
        - "${commit}"
    develop:
        - "cd ${path1} && go build"
        - "${path1}/program"
    commit:
        - "cd ${path1} && git push origin"
watch:
    ${path1}/*.go: "${develop}"
```

## 使用命令

``` shell
build [-c build.yml] [-s] [-k] [task name]
```

```-c```指定配置文件目录，若不指定，则使用当前目录的build.yml

```-s```指定在执行任务时，不输出日志

```-k```指定在监控到文件变化后，不清空输出的日志

最后指定任务名称，若不指定将执行default任务

## 源码

[https://github.com/InkProject/build.go](https://github.com/InkProject/build.go)

Clone代码后，使用```go build build.go```命令编译，获得名为```build```的可执行文件
