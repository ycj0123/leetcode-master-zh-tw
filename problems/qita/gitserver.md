
# 一文手把手教你搭建 Git 私服

## 為什麼要搭建 Git 私服

很多同學都問文章、文件、資料怎麼備份啊，自己電腦和公司電腦怎麼隨時同步資料啊等等，這裡呢我寫一個搭建自己的 Git 私服的詳細教學

為什麼要搭建一個 Git 私服呢，而不是用 GitHub 免費的私有儲存庫（repository），有以下幾點：
* GitHub 私有儲存庫真的慢，檔案一旦多了，或者有圖片檔案，git pull 的時候半天拉不下來
* 自己的文件難免有自己個人資訊，放在 GitHub 心裡也是擔心的
* 想建幾個庫就建幾個，想幾個人合作開發都可以，不香嗎？

**網上可以搜到很多 Git 搭建教學，但是說得模稜兩可**，而且有的直接是在本機搭建 Git 服務，既然是備份，搭建在本機哪有備份的意義，一定要有一個遠端伺服器（server），而且自己的電腦和公司的電腦還要能同步自己的文章、文件和資料等等。


適合人群：想透過 Git 私服來備份自己的文章、Markdown，並做版本管理的同學
最後，寫好每篇 Chat 是對我的責任，也是對你的尊重。謝謝大家～

正文如下：

-----------------------------

## 如何找到可以外網存取的伺服器

有的同學問了，自己的電腦就不能作為伺服器嗎？

這裡要說一下，安裝家庭頻寬（bandwidth），電信業者預設（default）是不會給我們獨立分配公網 IP 的

一般情況下是一片區域共用一個公網 IP 池，所以外網是不能存取到在家裡我們使用的電腦的

除非我們自己去做映射（mapping），這其實非常麻煩而且公網 IP 池是不斷變化的

辛辛苦苦做了映射，電信業者把 IP 一換，我們的努力就白費了

那我們如何才能找到一個外網可以存取的伺服器呢，此時雲端運算（cloud computing）拯救了我們。

推薦大家選一家雲端廠商（阿里雲、騰訊雲、百度雲都可以）在上面買一台雲端伺服器

* [阿里雲活動期間伺服器購買](https://www.aliyun.com/minisite/goods?taskCode=shareNew2205&recordId=3641992&userCode=roof0wob)
* [騰訊雲活動期間伺服器購買](https://curl.qcloud.com/EiaMXllu)

雲端廠商經常做活動，如果從來沒有買過雲端伺服器的帳號更便宜，低配一年一百塊左右的樣子，強烈推薦一次買個三年。

買雲端伺服器的時候推薦直接安裝 CentOS 系統。

這裡要說一下，有了自己的雲端伺服器之後，不僅僅可以用來做 Git 私服

**同時還可以做網站、做程式後台、跑程式、做測試**（這樣我們自己的電腦就不會因為各種搭建環境、下載各種套件（package）而搞得亂七八糟），等等等。

有自己雲端伺服器和一個公網 IP 真的是一件非常非常幸福的事情，能體驗到自己的服務隨時可以部署上去提供給所有人使用的喜悅。

外網可以存取的伺服器解決了，接下來就要部署 Git 服務了

本文將採用 CentOS 系統來部署 Git 私服

## 伺服器端安裝 Git

切換至 root 帳號

```
su root
```

看一下伺服器有沒有安裝 Git，如果出現下面資訊就說明是有 Git 的
```
[root@instance-5fcyjde7 ~]# git
usage: git [--version] [--help] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

The most commonly used git commands are:
   add        Add file contents to the index
   bisect     Find by binary search the change that introduced a bug
   branch     List, create, or delete branches
   checkout   Checkout a branch or paths to the working tree
   clone      Clone a repository into a new directory
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   fetch      Download objects and refs from another repository
   grep       Print lines matching a pattern
   init       Create an empty Git repository or reinitialize an existing one
   log        Show commit logs
   merge      Join two or more development histories together
   mv         Move or rename a file, a directory, or a symlink
   pull       Fetch from and merge with another repository or a local branch
   push       Update remote refs along with associated objects
   rebase     Forward-port local commits to the updated upstream head
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index
   show       Show various types of objects
   status     Show the working tree status
   tag        Create, list, delete or verify a tag object signed with GPG

'git help -a' and 'git help -g' lists available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

如果沒有 Git，就安裝一下，yum 安裝的版本預設是 `1.8.3.1`

```
yum install git
```

安裝成功之後，看一下自己安裝的版本

```
git --version
```

## 伺服器端設定 Git 帳號

建立一個 git 的 Linux 帳號，這個帳號只做 Git 私服的操作，也是為了安全起見

如果不新建一個 Linux 帳號，在自己常用的 Linux 帳號下建立的話，哪天手抖來一個 `rm -rf *` 操作，資料可全沒了

**這裡 Linux git 帳號的密碼設定得盡量複雜一些，我這裡為了演示，就設定成為 'gitpassword'**
```
adduser git
passwd gitpassword
```

然後就要切換成 git 帳號，進行後面的操作了
```
[root@instance-5fcyjde7 ~]# su - git
```

看一下自己所在的目錄（directory），是不是在 git 目錄下面

```
[git@instance-5fcyjde7 ~]$ pwd
/home/git
```

## 伺服器端金鑰管理

建立 `.ssh` 目錄，如果 `.ssh` 已經存在了，可以忽略這一項

為什麼要組態（configuration）SSH 公鑰（public key）呢，同學們記不記得我們使用 GitHub 上傳程式碼的時候也要把自己的公鑰組態上傳到 GitHub 上

這也是方便每次操作 Git 儲存庫的時候不用再去輸入密碼

```
cd ~/
mkdir .ssh
```

進入 `.ssh` 資料夾下，建立一個 `authorized_keys` 檔案，這個檔案後面就是要放我們客戶端（client）的公鑰

```
cd ~/.ssh
touch authorized_keys
```

別忘了給 `authorized_keys` 設定權限，很多同學發現自己不能免密碼登入，都是因為忘記了給 `authorized_keys` 設定權限

```
chmod 700 /home/git/.ssh
chmod 600 /home/git/.ssh/authorized_keys
```

接下來我們要把客戶端的公鑰放在 Git 伺服器上，我們再回到客戶端，建立一個公鑰

在我們自己的電腦上，有公鑰和私鑰（private key），兩個檔案分別是：`id_rsa` 和 `id_rsa.pub`

如果是 `Windows` 系統，公鑰私鑰的目錄在 `C:\Users\使用者名稱\.ssh` 下

如果是 Mac 或者 Linux，公鑰和私鑰的目錄在這裡 `cd ~/.ssh/`，如果發現自己的電腦上沒有公鑰私鑰，那就自己建立一個

建立金鑰的命令

```
ssh-keygen -t rsa
```

建立金鑰的過程中，一路按 Enter 鍵就可以了。不需要填任何東西

把公鑰複製到 Git 伺服器上，將我們剛剛產生的 `id_rsa.pub`，複製到 Git 伺服器的 `/home/git/.ssh/` 目錄

在 Git 伺服器上，將公鑰新增到 `authorized_keys` 檔案中

```
cd /home/git/.ssh/
cat id_rsa.pub >> authorized_keys
```

如何看我們組態的金鑰是否成功呢，在客戶端直接登入 Git 伺服器，看看是否是免密碼登入
```
ssh git@git伺服器ip
```

例如：

```
ssh git@127.0.0.1
```

如果可以免密碼登入，那就說明伺服器端金鑰組態成功了

## 伺服器端部署 Git 儲存庫

我們再登入到 Git 伺服器端，切換成 git 帳號

如果是 root 帳號切換成 git 帳號
```
su - git
```

如果是其他帳號切換為 git 帳號
```
sudo su - git
```

進入 git 目錄下
```
cd ~/git
```

建立我們的第一個 Git 私服的儲存庫，我們叫它為 world 儲存庫

那麼首先建立一個資料夾名為：`world.git`，然後進入這個目錄

有同學問，為什麼資料夾名字後面要放 `.git`，其實不這樣命名也是可以的

但是細心的同學可能注意到，我們平時在 GitHub 上 `git clone` 其他人的儲存庫的時候，儲存庫名字後面都是加上 `.git` 的

例如下面這個例子，其實就是 GitHub 對儲存庫名稱的一個命名規則，所以我們也遵守 GitHub 的命名規則。

```
git clone https://github.com/youngyangyang04/NoSQLAttack.git
```

所以我們的操作是
```
[git@localhost git]# mkdir world.git
[git@localhost git]# cd world.git
```

初始化（initialization）我們的 `world` 儲存庫

```
git init --bare

```

**如果我們想建立多個儲存庫，就在這裡建立多個資料夾並初始化就可以了，和 world 儲存庫的操作過程是一模一樣的**

現在我們服務端的 Git 儲存庫就部署完了，接下來就看看客戶端如何使用這個儲存庫呢

## 客戶端連接遠端儲存庫

我們在自己的電腦上建立一個資料夾，也叫做 `world` 吧

其實這裡命名是隨意的，但是我們為了和 Git 服務端的儲存庫名稱保持同步。這樣更直觀我們操作的是哪一個儲存庫。

```
mkdir world
cd world
```

進入 world 資料夾，並初始化操作

```
cd world
git init
```

在 world 目錄上建立一個測試檔案，並且將其新增到 Git 版本管理中

```
touch test
git add test
git commit -m "add test file"
```

將此儲存庫和遠端儲存庫同步

```
git remote add origin git@git伺服器端的ip:world.git
git push -u origin master
```

此時這個 test 測試檔案就已經提交到我們的 Git 遠端私服上了

## Git 私服安全問題

這裡有兩點安全問題

### Linux git 的密碼不要洩漏出去

否則，別人可以透過 `ssh git@git伺服器IP` 來登入到你的 Git 私服伺服器上

當然了，這裡同學們如果買的是雲端廠商的雲端伺服器的話

如果有人惡意想透過嘗試不同密碼連接的方式來連接你的伺服器，重試三次以上

這個客戶端的 IP 就會被封鎖掉，同時郵件通知我們這個 IP 來自哪裡

所以大可放心，密碼只要我們不洩漏出去，基本上不會有人能透過不斷嘗試密碼的方式來登上我們的 Git 私服伺服器

### 私鑰檔案 `id_rsa` 不要給別人

如果有人得到了這個私鑰，就可以免密碼登入我們的 Git 私服上了，我相信大家也不至於把自己的私鑰主動給別人吧

## 總結

這裡就是整個 Git 私服搭建的全過程，安全問題我也給大家列舉出來了，接下來好好享受自己的 Git 私服吧

**enjoy!**

