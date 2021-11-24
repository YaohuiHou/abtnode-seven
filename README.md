<!--
 * @Author: yaohui.hou
 * @Date: 2021-11-20 17:38:27
 * @LastEditTime: 2021-11-23 15:36:07
 * @Description: 项目描述
-->

# 项目介绍

使用官方[ Create-Blocklet ](https://github.com/blocklet/create-blocklet)构建项目。

## 环境

```
node version >= 14.0.0
abtnode version 1.5.14
blocklet version 1.5.14
```

## 框架介绍

vue3 + vite

## 项目启动

1. 安装[abtnode](https://docs.arcblock.io/abtnode/en/introduction/abtnode-setup)，并初始化

```
// 安装 ABT 节点
npm install -g @abtnode/cli
// 初始化ABT节点
abtnode init
```

2. 启动 ABT 节点

```
// 启动
abtnode start

// 结束abt节点命令
abtnode stop
```

3. 进入当前目录下，启动项目

```
cd [blocklet name]

npm install
// or
yarn

blocklet dev
```

4. 打包

```
npm run bundle
```
