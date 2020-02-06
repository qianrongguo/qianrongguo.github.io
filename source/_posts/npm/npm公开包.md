 `403 Forbidden - PUT https://registry.npm.taobao.org/myproject - [no_perms] Private mode enable, only admin can publish this module`
 
 提示镜像源错误：   `npm config set registry http://www.npmjs.org`


https://juejin.im/post/5c5012926fb9a049d37f81e1

### 创建项目
`mkdir my-test-project`

`git init`

`git remote add origin git@github.com:usename//my-test-project.git`
 
`git push -u origin master`

4.在项目根目录下即my-test-project目录下执行，npm init


### 发布

`npm adduser // or npm login
 Username: npm-user-name
 Password:
 Email: your-email`
 
 
### 执行
`npm publish`


### 关于测试
 
### 更新已发布的包

`修改版本号
npm publish`


### 撤销发布

` npm unpublish`
