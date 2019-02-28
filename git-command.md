# 本地仓库与远程仓库操作

## 从远程仓库克隆本地仓库

```
git clone [URL]
```

## 建立连接

```
git remote add origin [URL]
```

## 断开连接

```
git remote rm origin
```

## 查看连接

```
git remote -v
```

# 文件操作

## 查看状态

```
git status
```

## 新建文件夹

```
mkdir [foldername]
```

## 新建文件

``` 
touch [filename]
```

```
vi/edit(自定义) [filename]
```

## 删除工作区文件

```
rm [filename]
```

**注意删除也是一种改变, 要 : **

1. `git rm [filename]`删除版本库(暂存区)的对应文件
2. `git commit -m "delete filename"`

**如果误删 : **

`git checkout -- [filename]` : 使用暂存区的文件替换工作区的文件

## 初始化文件夹为本地仓库

```
git init
```

## 返回上一级文件夹

```
cd ..
```

## 提交文件到暂存区

* 提交单个文件

  ```
  git add filename
  ```

* 提交该目录下所有文件

  ```
  git add -A
  ```

* 删除误`add`的文件

  ```
  // 仅删除在暂存区的文件,保存本地文件
  git rm --cache [filename]
  
  // 同时删除暂存区与本地的文件
  git rm -f [filename]
  ```

  ## 从暂存区commit改动

  **先用`git status`查看改动是否正确再提交**

  ```
  git commit -m "message"
  ```

* 如果提交错误

  1. 使用`git log`获得上一次commit的id,复制这个id
  2. 执行`git reset [commit id]`,则这次commit的内容会回到本地,要重新`add`到暂存区,再commit

## 本地仓库推送远端仓库

* **`git push master origin`**

# `.gitignore`设置

* `*.temp` : 忽略所有以temp结尾的文件
* /[文件夹名] : 忽略整个文件夹

