# git的简单使用

## 一、如何打开git窗口进行使用：

1. 在安装好后，在一个空白的文件夹点击右键会出现一个`Git Bush`的选项，点开就好。
2. 开始+r，输入cmd回车，然后输入git，手动CD到指定的文件夹
3. 第一次使用的时候需要使用下面的指令输入用户名和邮箱

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

4. 使用git时，记住，没有反应就是最好的反应

---

## 二、如何创建自己的库：

1. 在指定的位置输入`git init`,要确认是否出现需要在对应的文件夹看到有一个.gif的文件夹，如果没看到，试着打开显示隐藏文件，如果看到了，说明已经在这个文件夹创建了一个git仓库

2. 把文件添加到仓库里：
    1. `$ git add 文件名`
    2. `$ git commit -m "描述内容"`用于说明你本次上传做了什么改动
    3. 在这个过程中可以使用`$ git status`查看文件状态

---

## 三、时光穿梭机

1. 用`$ git status`查看是否进行了修改
2. 用`$ git diff`查看在哪些地方进行了修改
3. **版本回退：**
    1. 使用命令`$ git log`我们可以查看之前修改过的版本的编号，作者，时间，描述
    2. 如果觉得输出的信息太多可以使用`$ git log --pretty=oneline`会显示出编号和描述
    3. 回退到上一个版本可以使用`$ git reset --hard HEAD^`
        * HEAD表示当前版本，HEAD^表示上一个版本，同理HEAD^^表示上上个版本，当然也并非如果要回退很多要输入对应个数的^，只需要HEAD~100
    4. 啊，不想要上一个版本了，我想回到原来最新版本怎么办？
        * 如果你没有退出控制框，并且已经输入过log指令，我们可以返回到上面找到最新的版本编号，然后使用指令`$ git reset --hard 版本指令前几位`就能回到最新版本
    5. `$ git reflog`记录你每一次命令

---

## 四、工作区和暂缓区：

1. 工作区：我们用来写文件的空白文件夹，就是工作区
2. 版本库：工作区的隐藏文件.git。这个就是暂缓区，然后里面有自动创建的第一个分支master以及指向master的一个指针HEAD
    * 我们git add添加到的就只这个暂缓区，git commit就是将文件提交到暂缓区的当前分支上
3. 如果每次进行修改不进行add，就不能commit，因此每次修改后都要add才能commit
4. 指令`$ git checkout -- 文件名`可以撤销修改
    * 使用这个命令的意思就是把这个文件的修改全部撤销，如果修改后还没有放到暂缓区，就返回到和版本库一样，如果已经添加到暂缓区了，就撤销修改回到添加到暂缓区后的状态（就是说回到最近的一次状态）

---

## 五、远程仓库

1. 如果这台电脑是你第一次使用git上传到github:
    * 输入指令`$ ssh-keygen -t rsa -C "youremail@example.com"`
    * 邮箱要填自己账号绑定的邮箱
    * 然后一路回车
    * 这时在用户的目录下会有一个.ssh的文件夹，点开，复制id_rsa.pub文件的内容
    * 进入github，点开自己的头像的setting选项，选择SSH and GPC keys
    然后新建一个newSSH，输入title，然后粘贴到key的输入框中即可

2. 创建一个仓库，然后复制他的SSH
    * 在本地文件夹中使用指令`$ git remote add origin +SSH`
    * 然后再使用指令`$ git push -u origin master`就能推送到github上了
    * 如果上传失败，不妨试试，`$ git push -f origin master`强制上传，但是本文件夹的内容将完全取代远程仓库找那个的内容
    * 如果把远程仓库clone下来，应该不会出现上传失败的情况

3. 复制远程仓库：`$ git clone + SHH`就能在本地文件夹创建一个一模一样的文件夹了

4. 如果想删除文件咋办呢？
    * 用指令`$ git rm 文件名`删除
    * 你可以直接在文件（文件夹）删除，然后上传到远程仓库
    * 如果没有反应。你可以用-f硬核删除（覆盖）

## 六、创建与合并分支

1. 使用`$ git checkout -b dev`创建dev分支，这里的-b表示创建并切换
2. 使用`$ git branch`会列出所有的分支，然后在当前的分支上打一个*号
3. 在dec分支工作完成后，使用指令`$ git checkout master`切换到主分支
4. 然后使用指令`$ git merge dev`进行合并
5. 当我们确认没有问题后就可以放心地吧dev分支删除了`$ git branch -d dev`
6. 新版的git中含有git switch命令来切换分支：

    ```html
    <!-- 用于创建并切换到dev分支 -->
    $ git switch -c dev
    <!-- 切换到主分支 -->
    $ git switch master
    ```

7. 当存在冲突时，必须手动解决冲突然后再上传才可以

## 八、分支管理策略

1. master分支应该非常稳定，仅用来发布新版本
2. dev分支上用来干活，因此dev分支很不稳定
3. `git merge --no-ff -m "merge with no-ff" dev`禁用了Fast forward,使得在删除分支后不会丢失分支信息
4. `git stash` 把要修复的bug存储起来
5. 从哪个分支修复BUG就在哪个分支上创建临时分支
6. 修复完成后合并到master上然后删除临时分支
7. 如何恢复删除的临时分支呢？
    * 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除
    * 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了
8. 开发一个新feature，最好新建一个分支，如果要丢弃一个没有被合并过的分支，可以通过`git branch -D 名字`强行删除
9. 查看远程信息：`git remote`  -v可以查看更详细的信息
10. 推送分支：`git remote 分支`
11. 推送失败可以使用`git pull`试图合并

## 九、标签管理

1. 打进一个新的标签：`git tag 名称`
2. `git tag`可以查看所有标签
3. 如果本应该在三天前打的标签，忘记打了怎么办？
    * 先使用`git log --pretty=oneline --abbrev-commit`找到以往commit的id
    * 然后使用`git tag 名称 ID`打入标签
4. `git show 标签名称`可以查看标签信息
5. 删除标签：`git tag -d 标签名称`
6. 标签推送到远程：`git push origin 标签名称`
7. 一次性推送所有标签：`git push origin --tags`
8. 如果想删除远程标签，就先删除本地标签，在用`git push origin :refs/tags/标签名称`在远端删除
