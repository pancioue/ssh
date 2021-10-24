針對不同domain設定不同ssh key
-----------
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
----------

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

* 這是最關鍵的步驟  
設定.gitconfig
```
git config --global core.sshCommand C:/Windows/System32/OpenSSH/ssh.exe
```

參考: https://gist.github.com/danieldogeanu/16c61e9b80345c5837b9e5045a701c99
