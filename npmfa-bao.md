一.  在npm上申请账号, 并且通过邮箱验证:[https://www.npmjs.com/](https://www.npmjs.com/)
二. 准备好要发包的项目
   ```
   npm login // 登录账号, 密码, 邮箱
   ```
   ![](/assets/npmlogin.png)
登录成功后出现: Logged in as ‘名称’ on https://registry.npmjs.org/

三. 发布: npm publish
```
package.json必填: name, version, repository, main, author
```

四. 更新包: 修改 package.json 的 version 然后在:  npm publish

### 注意事项:
1. 登录成功后一定要出现 on **https://registry.npmjs.org/**
2. npm adduser 创建的账号可能发包不成功, 建议网上直接申请账号


