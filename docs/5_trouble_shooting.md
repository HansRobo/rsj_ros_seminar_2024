# トラブルシューティング

## ネットワーク

### ロボットを新しいネットワークに接続したい

/etc/netplan/50-cloud-init.yamlを編集してSSIDとパスワードを変更してください。
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

### ロボットのIPアドレスを知りたい

1, MACアドレスを調べる

ロボット内で以下を実行

```bash
ip addr show
```

`link/ether`の後にあるMACアドレスを控えておいてください。
(`02:42:1e:fa:a9:e8`のようにコロンで英数字が区切られています)


2. nmapを使ってIPアドレスを調べる
```bash
sudo apt install nmap
sudo nmap -sn 192.168.20.0/24 | grep <MACアドレス> -n2
```

以下のように表示されるときの一番先頭のIPアドレスがロボットのIPアドレスです。
```
35-Nmap scan report for 192.168.10.124
36-Host is up (0.026s latency).
37:MAC Address: D8:3A:DD:5A:E9:7C (Unknown)
38-Nmap scan report for 192.168.10.126
39-Host is up (0.026s latency).
```


## ロボットが動かない

### モーターが接続されていない

電源を入れたときに車輪を手で回してみてください。車輪が回る場合は、モーターが接続されていない可能性があります。モーターの接続を確認してください。

### 