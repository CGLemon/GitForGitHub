# GitForGitHub

一些 git 和 GitHub 使用教學，幫助你快速上手 git

## 設定 config 和 Token

初次使用 git 前，記得先設定你的使用名稱和 email，這會與你的 git 和 GitHub 連動有關系，設定錯誤會無法正確推入你的專案 ，假設我的名稱是 ```CGLemon``` ，信箱是 ```myemail@gmail.com```，則輸入，

    $ git config --global user.name "CGLemon"
    $ git config --global myemail@gmail.com

想查看目前設定的訊息請輸入

    $ git config -l

現在要修改 GitHub 的 Repository 前，要先使用 Token，因為 Token 是必須的，開始前請先產生新的 Token，具體方法請看[這裡](https://dotblogs.com.tw/CYLcode/2020/06/15/102853)。由於 Token 過一段時間會失效，每隔一段時間就需要產生新的 Token，MacOS 預設會紀錄 Token 密碼而且不會自動更新，每次更新 Token 後都需要手動更新，具體方法請看[這裡](https://blog.myctw.cc/post/bd72.html)。

## 用 GitHub 上添加 Repository

在 GitHub 上添加專案並在本地（自己的電腦上）端建立相應的 git 專案有兩種方法，第一種作法是直接從 GitHub 遠端 clone 到本地，git 可以通過 Repository 在 GitHub上相應的網址下載之，例如此文檔的網址為 https://github.com/CGLemon/GitForGitHub，輸入下列指令即可下載

    $ git clone https://github.com/CGLemon/GitForGitHub

這樣不只是下載的檔案，還會自動建立 git 系統和添加遠端連接，但使用這個方法建議此 Repository 至少有一條 commit ，你可以在創建 Repository 時點選 ```Add a README file``` 這一項，這樣至少保證有一條 commit 存在。第二個方法是從本地端添加遠端再推入，這才是比較正確的使用方式，這個方法之後再說明。

## 推入（push）

首先你先創建你自己的 Repository，你可以在自己主頁的左上角看見綠色按鈕 New，點下添加新的 Repository。假設你新的 Repository 叫做 MyRepository，輸入以下指令 clone 到本地端

    $ git clone https://github.com/User/MyRepository

將 README.md 打開進行一些修改，準備將修改推入 GitHub 上，每次推入前，你必須先紀錄你修改哪些文件

    $ git add README.md

如果你不確定你修改哪些文件，數入 status 可以查看

    $ git status

之後紀錄修改內容摘要

    $ git commit -m "my first commit"

最後一步就推入你改變的內容。

    $ git push

之後會要求你輸入你的帳號和 Token ，完成後就會看到上傳畫面。

## 拉入（pull）同步

如果你有多台電腦 clone 同一份 Repository，當你在 A 電腦上完成修改並 push ，轉到 B 電腦上時工作時就要先同步，你應該不會希望每次同步都要重新 clone 一遍，你可以用 pull 將遠端的最新進度同步到本地端上，輸入

    $ git pull

就可以保證 B 和 A 電腦 Repository 狀態適一致的

## 用分之（branch）保護你的程式

如果你想對程式做一些實驗性質的改變，你應該不會希望再沒有備份的情況下修改，不然有後修改失敗記難以回覆了，branch 是一個特殊備份方式，可以幫助你回覆你的修改。這裡我們講一下 git 的運作方式，git 的文件是由多個 commit 組合而成，每一個 commit 我們可以看成一個節點，通過節點的前進或倒退到任何時間的狀態，而每串由尾巴（第一個 commit） 到頭 （最新的 commit） 稱為一個 branch，而  branch 的好處在於，多個 branch 可以同時分享同一棵數而且彼此修改獨立，必要時 branch 可以合併。我們以之前的 Repository 為例，首先我們先看當前 branch 的所有 commits ，輸入 log 查看

    $ git log

看完後案 q 退出，接下來添加新的 branch

    $ git branch test-branch

輸入 -l 可以看到你添加的 branch

    $ git branch -l

接者轉到 test-branch

    $ git checkout test-branch

我們新增一個文件叫做 test.txt，並添加之

    $ git add test.txt
    $ git commit -m "add test.txt"

我們可以切回原本 branch ，假設你的原本 branch 是 main（或 master）

    $ git checkout main

你可以看到你的修該回復了，當然修改並沒有消逝，你可以回到 test-branch 繼續修改。你可以將你添加的 test-branch 推入 GitHub 上，輸入以下指令推入

    $ git push origin test-branch

到 GitHub 上可以看到你推入的新 branch 。這裡先提一下 origin 是 GitHub 的遠端，和前面 ```$ git push``` 有關，只不過它是省略寫法，功能是推入主 branch，比較完整的寫法是下方，下方寫法和省略寫法功能一致。

    $ git branch origin main

如果你想要修改完後，確認功能無誤，可以將 test-branch 和主 branch 合併，收先回到主 branch 再輸入指令 merge，主 branch 就會合併其它 branch

    $ git checkout main
    $ git merge test-branch

完成合併。

## Pull Request

當你學會上述指令後，就可以給別人的 Repository 做出貢獻了，由於你沒有辦法直接 push 你的修改到別人的 Repository，因此要經歷以下步驟

1. fork 別人的 Repository，到別人 Repository 右上角，就可以看到此按鈕
2. clone 你自己已經 fork 的 Repository 到本地
3. 添加新的 branch ，修改新的 branch ，完成後請不要併入主 branch，直接新 branch 推入 GitHub
4. 用修改的 branch 發出 Pull Request 請求，Pull Request 可到 fork 的 Repository 上找到

整個過程中，都必須確保主 branch 沒有被修改到，這是因為爾後原作者更新後，我們可以直接 pull 更新而不影響。

<br>
<br>
（努力施工中...）
