# 履修空き監視＆通知 構成図
<img width="786" height="380" alt="image" src="https://github.com/user-attachments/assets/c9ed9fc2-73e2-4ace-99f3-1cdd838d2f33" />

  
# 導入方法
## 0. TOKENをパソコンに送っておく
1. 別途通知したLINE_CHANNEL_ACCESS_TOKEN（謎の長い英数字記号の文字列）をパソコンで観れるメール（Gmailなど）に送っておく。

## 1. 通知用LINEの準備
1. 別途送ったQRコードを読み取って、「履修空き通知ﾈｺ」をお友達登録します。
2. 登録した履修空き通知ﾈｺに何か話しかけると、以下のメッセージが返ってきます。
   > ｼｬｰｰ！あなたのuserIdは：  
   > xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx （長い英数字の文字列）
   > です!これをline.envに設定するのよ!!
   - これはあなたのLINEの内部的なIDです。
   - `履修空き通知ﾈｺはこのuserIdに向けて履修空きの通知を送ってきます`。
   - このuserId(長い英数字の文字列)をコピーしてパソコンで見れるメール（Gmailなど）に送っておきます。（気合で記憶してもいいですが間違っていると通知されません。

## 2. 実行プログラムの準備
1. このページ右側の **「Releases」** のところから **Asheetの部分をクリック→「Risyu-kanshi_v2.0.0.zip」** をダウンロードしてzipファイルを展開します。(パスワードは別途教えます）
    <img width="1033" height="259" alt="image" src="https://github.com/user-attachments/assets/2155a3ef-3e4a-4b9a-ae17-0ee92953d9a1" />
   - Risyu-kanshi_v2.0.0.zip
     | ファイル名 | 用途 |
     | ----------| ---- |
     |env/line.env|LINE通知用の設定ファイル。環境によっては「line」表示されているかも|
     |enrollmentControl.exe|メインのプログラムファイル|
     |enrollmentControl_debug.exe|うまく動かないときの情報取得用。通常使いません。|
     |line_check.exe|LINE通知チェック用|
     |Run.bat|実行用ファイル。環境によっては「Run」とだけ表示されているかも|
       
      **`補足`**：
       こんな感じになっていればOK
       <img width="723" height="227" alt="image" src="https://github.com/user-attachments/assets/242fb85c-70e8-46b6-b36a-c705e39f7cf1" />

3. envフォルダ内のline.envをメモ帳などで開いて、手順0と手順1でパソコンのメールに送っておいたTOKENとuserIdを以下の部分にコピペします。
   > LINE_CHANNEL_ACCESS_TOKEN=ここに別途通知されたTOKENを貼り付け  
   > LINE_TARGET_USER_ID=ここに履修空き通知ﾈｺが教えてくれたuserIdを貼り付け
  
      **`補足`**：  
       LINE_CHANNEL_ACCESS_TOKENは長いのでこんな感じにメモ帳で勝手に改行されるけど気にしない
       <img width="697" height="131" alt="image" src="https://github.com/user-attachments/assets/f4acc6b1-baa4-4043-96a9-7ed423581ec1" />

4. LINEへの通知確認のため、line_check.exeを実行します。
   1. 履修空き通知ﾈｺから、LINEメッセージが通知されたら成功です。
   2. 通知されなかった場合、手順2-2の設定がおかしいかもしれません。見直してください。
5. Run.batをメモ帳などで開いて、以下「--tab_a_url "〇〇"」の〇〇を監視したいURLに書き換えます。IDとパスワードも自分のIDとパスに書き変えます。
    > @echo off  
    > cd /d "%~dp0"  
    >enrollmentControl.exe --tab-a-url "https://xxxxxxx/rishu/capacity_state/xxxxx"  --id "your_id" --password "your_password"  
   > pause 
    
   **`補足`**：  
   --tab-a-urlに指定するのは上のサンプルのように`「capacity_state/～～」というURLを指定`してください。空いている枠が表示されるあのページです。
    
6. これで準備完了です。

# 実行方法
1. Run.bat ダブルクリックすると、履修空きの監視が開始されます。
   1. 履修空き監視は、手順2-4で指定したページの授業の空き状況を5秒おきに監視します。
   2. 履修登録サイトが一定時間でログアウト警告をアピールしてくる、かつ、自動ログアウトしてくるので、それに対抗すべく、15分経つと自動でログアウトして、再度自動でログインし、指定した授業の空き状況を監視し続けます。
   3. 指定した授業の空きがでると、履修空き通知ﾈｺが空きが発生したことを通知してきます。急いでログインして、履修登録してください。監視画面は、空きを検出してから10分間は開きっぱになるので、監視の画面を操作して履修登録を完了させてもOKです。（10分経つと問答無用で閉じちゃいますので注意してください）

# 終了方法
1. 監視状況が表示されている画面を×ボタンで閉じていいです。
2. うまく閉じなかったら、自動で開いているブラウザや、黒い画面をすべて閉じてOKです。

# 注意事項
- 監視中のブラウザは閉じないようにしてください。（閉じても動くように作ってみましたが、解消しませんでした）
- 今回の仕様には、少々環境依存する仕組みが入っています。
  - いきなり監視対象のページが開かない場合があります。その時は教えてください。
  - はじめは動いていても、期間が開いて起動すると、突然動かなくなる場合があります。その時は教えてください。
- 別途通知した`**「LINE_CHANNEL_ACCESS_TOKEN」は大切に保管してください**`。ネット上には公開しないようにしてください。
  - このTOKENはLINE Businessの機能を利用するために使われます
  - LINE Businessの利用は一定数の無料枠がありますが、悪用されると、大量にLINEの機能を利用され無料枠を超えて高額請求されるなどの可能性があるため、大切に保管してください。
  - 場合によって、 LINE_CHANNEL_ACCESS_TOKEN は変更することがあります。その場合は新しい LINE_CHANNEL_ACCESS_TOKEN をお知らせしますので、手順2-2を読み返してTOKENの値を修正してください。
 
  

