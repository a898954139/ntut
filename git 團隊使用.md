# Git 團隊使用手冊
###### tags: `Git`
---

[TOC]


---
## 存取模型
- Git 是個分散是版本控制系統，也就是說使用者想要記錄個人的更動時，不需要使用同樣的地方。每個使用者可以在集中的程式碼之外獨立工作，對自己本機上儲存庫副本所建立的紀錄 (commit) 負責。這表示其他開發人員的更動不會被強制加入你的工作成果，而是由開發人員自己決定納入其他人工作進度的時機，也由開發人員自己決定分享工作成果的時機。

## 團隊開發
### 工程版本控管
&emsp;&emsp;對於單人團隊，可能會覺得使用工單系統有些過頭，也許只需要使用紙本的筆記本就夠了，但如果每次為新的想法建立brach，這些分支最後常常會深陷在大量的過期分支中。所以養成寫下結語的習慣很重要，記下預計想要為軟體完成的功能，至少能夠提供建立分支時使用的編號，幫助追蹤程式碼的狀況。

- 一旦能夠追蹤你的想法以後，作業程序應該要依照以下的步驟 : 
    1. 在工單追蹤系統建立新的工單，記下工單的編號。
    2. 在本機儲存庫以**工單編號-描述**的格式建立新的分支
    3. 進行工單上描述的工作(而且只做工單上的內容)
    4. 測試工作成果，確認工作完成且正確無誤，確保在開發 環境能夠通過工單上描述的QA測試內容。
    5. 現在有個 「dirty」 的工作目錄，包含了新增與修改過的檔案，將檔案加入本機儲存庫的整備區(staging area)。
    6. 將整備區的更動記錄到儲存庫(repository)。
    7. 將更動推送到備分主機，一般也就是紀錄工單的主機，例如 GitLab 或 GitHub，依據工單系統的不同，工單可能會標記為「已解決」(resolved)，但還不是「已關閉」(closed)。
    8. 一旦對成果完全滿意，將工單分支合併回主分支(通常是master)，並將更新
    9. 再次測試工作成果，確保沒有任何的後續問題。
    10. 更新工單系統，關閉工單。

### 建立本機儲存庫
在 Git 建立新的儲存庫，通常是從以下三種不同的情況開始:
- 從複製已經存在的儲存庫
- 從現有的檔案夾下的未追蹤 (untracked) 檔案
- 從空的目錄

這邊將會介紹以上三種不同情況下建立儲存庫的方式。
先建立一個檔案夾，接下來會在這個檔案夾儲存所有的範例儲存庫。

### 複製現有的專案
&emsp;&emsp;在 GitHub 或任何程式碼代管系統，在專案網頁上一般可以選擇以 .zip 格式下載包含所有檔案的壓縮檔，或是建立儲存庫的副本。
![](https://i.imgur.com/QWIrVsq.png)

要下載專案副本使用的命令是clone，使用方式如下 :
```shell=
$ git clone git@github.com:a898954139/PB.git
```
你的終端應該會出現以下訊息 :
```
remote: Enumerating objects: 768, done.
remote: Counting objects: 100% (768/768), done.
remote: Compressing objects: 100% (423/423), done.
remote: Total 768 (delta 323), reused 722 (delta 302), pack-reused 0
Receiving objects: 100% (768/768), 168.48 MiB | 2.16 MiB/s, done.

Resolving deltas: 100% (323/323), done.
Updating files: 100% (612/612), done.
```

這樣就成功clone了第一個 Git 儲存庫

### 轉移現有專案到 Git
#### 初始化目錄進入版本控管
```shell=
$ git init
```
會看到以下訊息輸出:
```
Initialized empty Git repository in C:/Users/a8989/OneDrive/桌面/BackBranch/.git/
```
檔案不會立即加入repository ，這是個功能，因為Git 允許開發人員忽略特定檔案，所以需要等使用者告知那些檔案確實需要納入紀錄，對於所有合理的後續動作，Git 幾乎都會在狀態訊息裡提供適當的建議，你應該要養成習慣經常使用 status 命令的習慣，就像是使用文書軟體時要經常存檔一樣， status 命令不會儲存工作進度，而是讓使用者知道當時儲存庫的狀態。

#### 檢查儲存庫的狀態

```shell=
$ git status
```

Git 會建議接下來應該要加入想要追蹤的檔案，因為目前儲存庫才剛做完初始化 :


```shell
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        PB.sln
        PB/

nothing added to commit but untracked files present (use "git add" to track)
```

將檔案加入 Git 需要兩個步驟，能讓開發人員在工作區一次進行多個不相關的更動。更動可以分組記錄到索引，每一組會有不同的紀錄訊息，因為目前是第一次匯入檔案，所以接下來要加入工作目錄裡所有的東西。

```shell=
$ git add --all
```
#### 初始化空專案
1. 建立新的、空的檔案夾 : 
```shell=
$ mkdir empty-repository
```
2. 切換工作目錄到新建立的檔案夾 :
```shell=
$ cd empty-repository
```
3. 執行 Git 初始化命令 :
```shell=
$ git init
```
4. 檢查是否建立了隱藏的儲存庫目錄 :
```shell=
$ ls -al
```
#### 檢視歷史紀錄
在儲存庫建立第一筆紀錄之後，就可以開始檢視歷史紀錄了 (history) 。使用 log 命令就能夠檢視專案中包含的更動，這個命令能讓使用者檢視本機儲存庫目前簽出的分支裡的紀錄訊息以及各紀錄的作者資訊。
```shell=
git log
```
log 命令會以相反順序列出repos所有的紀錄訊息。
```shell=
commit 19d5c2290689fafc76434f1078026cf857a8c85c (HEAD -> master)
Author: Warz <a898954139@gmail.com>
Date:   Fri Feb 11 21:27:51 2022 +0800

    Initial import of all project files
```
要是使用的是更完整的程式碼，顯示的訊息就會多上許多，可能會多到讓人難以瀏覽，你可以加上 --oneline 參數，讓輸出的結果只顯示訊息的第一行，按 q 就能跳出顯示狀態。
```shell=
git log --oneline
```
### 使用分支
在版本控制裡分支是用來分隔不同的構想，有許多不同的使用方式，開發人員可以用分支來代表軟體的不同版本，也可能針對處裡的臭蟲修正建立短期的分支，或是只用較長期的分支測試一些新的想法。
#### 列出分支
要列出所有的分支，可以單獨使用 branch 命令，也可以加上 --list 參數。
預設會將master分支複製到本機儲存庫，讓開發人員可以在該分支作業，除了這個分支之外，也會同時下載遠端repos上已建立的其他分支，這些遠端 repos branch 是作為參考之用，但必須為遠端分支建ㄌ
```shell=
git branch --all
```
如果你在先前複製的repos的本機複本中使用這個命令，應該會同時在列表中看到本機分支以及遠端分支，其中 * 表示目前工作目錄顯示(或簽出)的分支，接下來幾行都是以 remotes/origin 開頭， remotes 表示不在本機，而origin則是用來表示「本機複本複製來源」的預設名稱，最後則是分支的名稱(master) :
```shell=
* master
  remotes/origin/master
```
這樣的呈現方式可能會造成誤會，遠端分支的名稱實際上並不包含remotes這個字，這只是用表示分支類型的資訊，必須要 --remotes 參數，顯示可以使用的遠端分支名稱列表 :
```shell=
git branch --remotes
```
這個命令只會列出遠端的分支(使用真正的名稱) : 
```shell=
origin/master
```
#### 更新遠端分支的列表
遠端分支不會自動維持在最新的狀態，所以一段時間之後就會與遠端repos的現況不符，可以使用 fetch 命令更新遠端分支的列表
```shell=
git fetch
```
#### 使用不同的分支
```shell=
git checkout branch-name
```
簽出分支時，會將系統(工作樹)的可視檔案更新為與repos 中儲存的版本相符，檔案的切換完全由 checkout 命令完成，簽出程序與 Subversion 等集中式版本控制系統 (Version control system, VCS) 稍有不同，在集中式VCS裡，因為分支不是儲存在本機，使用前必須先行下載
#### 建立新的分支
剛建立新的分支時，會與建立時的分叉點有相同的歷史紀錄，使用 log 命令檢視新分支的歷史紀錄，會同時顯示其親帶分支的紀錄。

既然採用了工單式版本控制，分支名稱就要反映出處裡的工單，例如，要是議題是「1:在README 加上程序說明」(Add process notes to README)，分支就應該命名為 1-process_notes，新分支的歷史紀錄會包含分叉點之前的所有紀錄，所以請確認在正確的起始點建立新分支。可以先利用 checkout 命令移動到正確的分支，或是在分支命令中指明想要使用的親代分支。

先簽出想要作為起點的分支 :
```shell=
git checkout master
```
接著，建立新的分支
```shell=
git branch 1-process_notes
```
最後，簽出新分支
```shell=
git checkout 1-process_notes
```
也可以這樣做
```shell=
git checkout --track -b 1-process_notes master
```
#### 在儲存庫加入更動
每次更動工作目錄時，都需另外明確的將更動儲存到Git repos，這包含了兩個步驟
``` mermaid
graph RL;
工作區(可編輯的檔案)--> add --> 整備區 --> commit-->local_repos(本機儲存庫) 
```
先前在建立新 repos 一次匯入了許多檔案，但實際上不需要一次加入所有的檔案，這個功能特別適用於必須透過不同的紀錄，儲存不相關的編輯結果。要是想把更動分別存放在不同的紀錄，就必須把之前使用的 --all 參數改變成想要放入整備區的檔案名稱，一次可以放入一個或多個檔案，檔案名稱不需要是相同類型。
```shell=
git add README.md process-diagram.png
```
我大部分是一次加入一個檔案到備整區，這種作法能夠避免意外的檔案，在命令列只需要輸入檔案名稱的前幾個字元，再按下 Tab 鍵，就會自動補齊剩下的檔案(這稱為 Tab 補齊)。要是需要加入許多的檔案，而且都放置在相同的目錄，也可以使用萬用字元(wildcard)表示子目錄下的所有檔案或是類似名稱的檔案。

```shell=
#特定路徑遞迴加入多個檔案
git add <目錄名稱>/*
#加入所有副檔名為 .svg 的檔案
git add *.svg
```
也可以完全省略檔案名稱，依據檔案是否已納入Git 追蹤來嫁入整備區，透過 --update 參數可以將上次紀錄後所有納入 Git 追蹤且編輯(或更新)過的檔案加入整備區:
```shell=
git add --update
```
或者加入所有有更新的檔案
```shell=
git status
git add --all
```
一旦加入整備區之後，就必須要記錄下來，如果開發人員繼續編輯已經加入整備區的檔案，接下來執行commit 命令的時候，只有之前加入整備區中的更動會被記錄下來。
#### 將檔案的部分更動加入Repos
```shell=
git add --patch 檔案名稱
```
#### 將已提交的檔案更動移出整備區
```shell=
git reset HEAD <file>
```
#### 寫下明確的紀錄訊息
1. 使用簡略的單行訊息將更動記錄到repos
2. 修正 (amend) 紀錄，詳細說明更動時的思考過程。

```shell=
git add --all
git commit -m "say something"
git commit --amend
```