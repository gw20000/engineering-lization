# npm源管理
因为我的项目之前使用的npm源设置了淘宝镜像，所以开始使用的时候报错，npm发包需要使用npm的源镜像

1、查看npm源：npm config get registry
2、镜像切换：
淘宝镜像：npm config set registry https://registry.npm.taobao.org
npm镜像：npm config set registry https://registry.npmjs.org
3、临时使用淘宝镜像：npm --registry https://registry.npm.taobao.org install lodash
在npm发包之前需要把镜像切换到npm镜像，执行npm config set registry https://registry.npmjs.org

# npm 发包

## 首次发包

1. register a npm account 

2. create a npm pkg:

npm init 

3.  npm login 


4. npm  publish

## 修改包

npm version patch：1.0.0会变成1.0.1
npm version major：1.0.0会变成2.0.0
npm version minor：1.0.0会变成1.1.0

## 删除包
```
<pre> npm deprecate <pkg-name>[@<version>] <msg> </pre>

```
