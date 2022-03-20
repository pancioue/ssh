傳輸行為
===========
當A要傳送給B時
1. A用自己的私鑰簽名，再用B的公鑰加密 (此時連A自己也無法解開)
2. A傳送給B
3. B收到訊息後先用自己的私鑰解密，再用A的公鑰確認是否為A的簽名

* 伺服器接收到用戶的請求之後，伺服器便會將公鑰檔案傳給用戶端此時是明碼傳送，反正公鑰本來就是要給大家使用的
* 輸入帳密模式，用戶端的金鑰是隨機運算產生於本次連線當中的
* SSH 協定，在預設的狀態中，本身就提供兩個伺服器功能：
  1. 一個就是類似 telnet 的遠端連線使用 shell 的伺服器，亦即是俗稱的 ssh
  2. 另一個就是類似 FTP 服務的 sftp-server ！提供更安全的 FTP 服務。
    在不考慮圖形介面的情況下，它已經可以取代FTP了
* 目前SSH多半是採用RSA加密機制，不過也可以使用DSA或Diffie-Hellman等加密機制

金鑰指紋:
由public key 雜湊加密而來，用來驗證使用者
第一次驗證時server會傳來server的金鑰指紋，
此時要比對server的公鑰指紋使否正確，
確定建立連線後，便會在known_hosts紀錄遠端ip和公鑰指紋
下次登入時就會直接在known_hosts對應ip的公鑰指紋


參考: https://codecharms.me/posts/security-ssh  
鳥哥ssh參考: https://linux.vbird.org/linux_server/centos6/0310telnetssh.php#ssh_connect

針對不同domain設定不同ssh key
===========
__~/.ssh/config__
```
Host github.com
  IdentityFile ~/.ssh/pankey
```

可以在最後面加上default值
```
Host github.com
  IdentityFile ~/.ssh/pankey
  
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
```
* ssh會按照順序往下搜尋，*代表所有host都通，但不會覆蓋掉已經設定的值  
參考:https://www.tecmint.com/configure-custom-ssh-connection-in-linux/

ssh-agent: 自動記住金鑰(passphrase)
==========

* 啟動ssh-agent後，加入金鑰
```
ssh-add $HOME/.ssh/your_file_name
```

* 設定`~/.ssh/config`，
```
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/pankey
```
或
```
Host github.com
  IdentityFile ~/.ssh/pankey

Host *
	AddKeysToAgent yes
	IdentitiesOnly yes
```

* 設定.gitconfig  
  使用github測試的話，這是最關鍵，較想不到的步驟  

```
git config --global core.sshCommand C:/Windows/System32/OpenSSH/ssh.exe
```

參考: https://gist.github.com/danieldogeanu/16c61e9b80345c5837b9e5045a701c99
