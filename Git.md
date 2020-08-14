## 分支管理

1. 添加分支

   ```powershell
   //本地
   git switch -c <branchname>
   //远程
   git push -u origin <branchname>
   ```

2. 删除分支

   ```powershell
   //本地
   git branch -d <branchname>
   //远程
   git push origin :<branchname>
   git push origin --delete <branchname>
   ```

   

## .gitignore

1. 格式规范

   > #为注释
   >
   > 可以使用shell所使用的正则表达式
   >
   > /结尾标识目录
   >
   > !取反

   例：

   ```
   *.a
   !lib.a
   #忽略所有.a结尾的文件，除了lib.a
   ```

2. 修改

   若存在文件在.gitgnore被修改前加入到版本管理(tracked)，则仅修改.gitignore无效，需要删除缓存区(cached)，将其变为untracked状态后提交

   ```powershell
   git rm -r --cached .
   git add .
   #也可以使用git rm -r --cached <filename>指定文件
   git commit -m "update .gitignore"
   ```
