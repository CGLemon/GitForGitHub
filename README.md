# GitForGitHub

一些 git 和 GitHub 使用教學，幫助你快速上手 git

## 設定 config 和 Token

初次使用 git 前，記得先設定你的使用名稱和 email，這會與你的 git 和 GitHub 連動有關系，設定錯誤會無法正確推入你的專案，假設我的名稱是 ```CGLemon``` ，信箱是 ```myemail@gmail.com```，則輸入，

    $ git config --global user.name "CGLemon"
    $ git config --global myemail@gmail.com

想查看目前設定的訊息請輸入

    $ git config -l

現在在修改 GitHub 的 Repository 前，要先使用 Token，由於 Token 是必須的，開始前請先產生新的 Token，具體方法請看[這裡](https://dotblogs.com.tw/CYLcode/2020/06/15/102853)。因此 Token 過一段時間會失效，每隔一段時間就需要產生新的 Token。MacOS 預設會紀錄 Token 密碼而且不會自動更新，每次更新 Token 後都需要手動更新，具體方法請看[這裡](https://blog.myctw.cc/post/bd72.html)。

## 用 GitHub 上添加 Repository

在 GitHub 上添加專案並在本地（自己的電腦上）端建立相應的 git 專案有兩種方法，第一種作法是直接從 GitHub 遠端 clone 到本地，git 可以通過在 GitHub上相應的網址下載 Repository，例如此文檔的網址為 ```https://github.com/CGLemon/GitForGitHub``` ，輸入下列指令即可下載

    $ git clone https://github.com/CGLemon/GitForGitHub

這樣不只能下載的檔案，還會自動建立 git 系統和添加遠端連接，使用這個方法建議此 Repository 至少有一條 commit ，你可以在創建 Repository 之初時點選 ```Add a README file``` 這一項，這樣至少保證有一條 commit 存在。第二個方法是從本地端添加遠端再推入，這才是比較正確的使用方式，這個方法之後再說明。

## 推入（push）

首先你先創建自己的 Repository，你可以在自己主頁的左上角看見綠色按鈕 New，點下添加新的 Repository。假設新的 Repository 叫做 MyRepository，輸入以下指令 clone 到本地端

    $ git clone https://github.com/User/MyRepository

將 README.md 打開進行一些修改，接者準備將修改推入 GitHub 上，每次推入前，你必須先添加你修改過的文件。這一步並沒有真的紀錄改變的狀態，是可以重複或分多次執行的。

    $ git add README.md

如果你不確定哪些你修改的文件還沒添加，或哪些文件以被被添，輸入 status 可以查看

    $ git status

之後紀錄修改內容的摘要。注意，這一步是真的寫入改變紀錄。

    $ git commit -m "my first commit"

最後一步就推入你改變的內容。

    $ git push

之後 GitHub 會要求你輸入你的帳號和 Token ，完成後就會看到上傳畫面。你可以到 GitHub 上看到你推入的修改。

## 拉入（pull）同步

如果你有多台電腦 clone 同一份 Repository，當你在 A 電腦上完成修改並 push 到 GitHub 上後，轉到 B 電腦繼續工作，工作前你得先同步，一個間單方法為全部刪除再整個 clone，但你應該不會希望每次同步都要這樣做（而且這樣同時會刪除很多 git 資料），正確的方法是使用 pull 功能將遠端的最新進度同步到本地端上，輸入

    $ git pull

如此可以保證 B 和 A 電腦還有 GitHub 的 Repository 狀態是一致的

## 用分之（branch）保護你的程式碼

如果你想對程式做一些實驗性的改變，你應該不會希望在沒有備份的情況下修改，不然修改失敗後可能就難以回復了，branch 是一個特殊備份方式，可以幫助你回復你的修改。這裡我們講一下 git 的運作方式，git 的文件是由多個 commit 組合而成，每一個 commit 我們可以看成一個節點，通過節點的前進或倒退到可以到任何時間的狀態，而每串由尾巴（第一個 commit） 到頭 （最新的 commit） 的節點稱為一個 branch，用 branch 系統的好處在於，多個 branch 可以同時分享同一棵樹而且彼此修改獨立，必要時 branch 可以相互合併。我們以之前的 MyRepository 為例，首先我們先看當前 branch 的所有 commits ，輸入 log 查看，應該可以看到你第一次修改的 commit

    $ git log

看完後按 q 退出，接下來準備添加新的 branch，輸入 branch 指令可以將當前 branch 的 commit 整個複製到新 branch 中，假設添加的 branch 名為 test-branch

    $ git branch test-branch

輸入 -l 指令可以看到你添加的 branch 名稱（也可以看到所有存在的 branch，包括從 GitHub 載下來的）

    $ git branch -l

接者轉到 test-branch

    $ git checkout test-branch

我們新增一個文件叫做 test.txt，並添加之，添加完後我們再輸入 log 看看有甚麼變，可以看到除了你剛剛添加 commit ，還有原本 branch 的 commit

    $ git add test.txt
    $ git commit -m "add test.txt"
    $ git log

我們可以切回原本的 branch ，假設你的原本的 branch 是 main（或 master，看 Repository 擁有者的設定）

    $ git checkout main

你可以看到你的修改回復了，test.txt 這個檔案消失了，用 log 查看也不存在，當然它並沒有真的消失，修改的部份保存在 test-branch 裡。你可以將你添加的 test-branch 推入 GitHub 上，輸入以下指令推入

    $ git push origin test-branch

完成推入後，到 GitHub 上可以看到你推入的新 branch 。這裡先提一下 origin 是 GitHub 預設的遠端，前面 ```$ git push``` 和遠端有關聯，只不過它是省略寫法，它功能是推入主 branch，下方是比較完整的寫法，下方寫法和省略寫法功能一致。

    $ git branch origin main

如果你修改完後，確認功能無誤，可以將 test-branch 和主 branch 合併，首先回到主 branch ，再輸入指令 merge，主 branch 就會合併指定的 branch

    $ git checkout main
    $ git merge test-branch

完成合併，你的修改也會出現在主 branch 上。

## Pull Request

當你學會上述指令後，就做好給別人的 Repository 做出貢獻了的準備了，由於你沒有辦法直接 push 你的修改到別人的 Repository，因此要經歷以下步驟

1. fork 別人的 Repository，到別人 Repository 主頁右上角，就可以看到此按鈕，按下即可 fork
2. clone 你自己 fork 的 Repository 到本地
3. 從主 branch 添加新的 branch ，之後的修改都要在新的 branch 上，完成修改後請不要併入主 branch，直接將新 branch 推入 GitHub
4. 用修改的 branch 發出 Pull Request 請求，Pull Request 可到你 fork 的 Repository 上找到

整個過程中，都必須確保主 branch 沒有被修改到，這是因為爾後原作者更新後，我們可以直接 pull 更新而不影響。

## 添加遠端

如果你一份代碼同時有兩個遠端要推入，比如 GitHub 和 Gitee，那要怎做呢？我們可以透過指定遠端的方式，決定要推入那個 Repository。因為要申請 Gitee 帳號會比較麻煩，我們直接在 GitHub 添加新的 Repository 當作第二個遠端，一樣先創建新的 Repository 名叫 MyRemoteRepository ，但不需要點選 ```Add a README file``` 這一項。現在的目標是，將前面創建的 MyRepository 推入 MyRemoteRepository，首先 clone 先前的 MyRepository

    $ git clone https://github.com/User/MyRepository

推入前我們先看一下 MyRepository 目前有哪些遠端。

    $ git remote

沒有意外的話，可以看到僅有 origin 這個遠端，origin 是 GitHub 預設的遠端名稱。接下來為 MyRemoteRepository 添加新的遠端，添加的方式很間單，每個 GitHub 的 Repository 都會有一個 xxx.git 的檔案，我們添加的目標就是它，輸入下列指令，添加新的遠端名為 newremote

    $ git remote add newremote https://github.com/User/MyRemoteRepository.git

接者就可以直接推入了，輸入下列指令後，可以看到在 GitHub 的 MyRemoteRepository 裡，被推入和 MyRepository 一模一樣的 commits。

    $ git push newremote main

<br>
<br>
（努力施工中...）
