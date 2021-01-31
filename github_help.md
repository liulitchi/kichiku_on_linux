## 上传本地代码

- 第一步：去github上创建自己的 Repository

- 第二步：测试

 > echo "# Test" >> README.md

- 第三步：建立git仓库

 > git init

- 第四步：将项目的所有文件添加到仓库中

 > git add .

- 第五步：

 > git add README.md

- 第六步：提交到仓库

 > git commit -m "注释语句"

- 第七步：将本地的仓库关联到GitHub，后面的https改成刚刚自己的地址，上面的红框处

 > git remote add origin https://github.com/??????/Test.git


## 更新代码

- 第一步：查看当前的git仓库状态，可以使用git status

 > git status

- 第二步：更新全部

 > git add *

- 第三步：接着输入git commit -m "更新说明"

> git commit -m "更新说明"

- 第四步：先git pull,拉取当前分支最新代码

 > git pull

- 第五步：push到远程master分支上

 > git push -u origin master
 
## 修改项目名称重新链接本地
 
 > git remote rm origin

 > git remote add origin git@github.com:someuser/newprojectname.git
 
 ## 版本回退
 
 `git log` 查看版本号
 
 `git reset --hard abcdefg` 回退对应版本
 
 `git push -f` 提交更改

