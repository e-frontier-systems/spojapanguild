# **エアギャップマシン作成(Docker版)**

!!! summary "エアギャップマシンとは？"
    プール運営で使用するウォレットの秘密鍵やプール運営の秘密鍵をオフライン上で管理し、エアギャップオフラインマシンでCLIを起動しトランザクションファイルに署名する作業に使用します。
    
    * ウォレットの秘密鍵やプール運営の秘密鍵をプール運営のオンラインマシン上に保管すると、ハッキングなどの際に資金盗難のリスクがあります。

!!! summary "Dockerとは？"
    コンテナという技術を使用して...

## **インストール要件**

!!! info "動作環境"
    **[Docker Desktop](https://www.docker.com/ja-jp/products/docker-desktop/) などDockerが動作する環境**

    * macOS (Intel チップ)
    * macOS (Apple Silicon)
    * Windows 11 Home, Pro
    
    ディスク容量 1GB程度

!!! info "ダウンロード/インストール"
    
    * Docker Desktop
    * airgap-1.0.5.tar.gz

## **1- Docker Desktop のダウンロード**

1-1. 下の画像の赤い四角のところをクリックしてダウンロードします。

* [Docker Desktop の入手](https://www.docker.com/ja-jp/products/docker-desktop/)


## **2- Docker Desktop のインストール**

=== "Windows" 
    
    2-1. ダウンロードした、`Docker Desktop` のインストーラーをダブルクリックして起動する。
    
    | ファイル名                     |
    |------------------------------|
    | Docker Desktop Installer.exe |
    
    
    2-2. 
    
=== "macOS"

    2-1. ダウンロードした、`Docker Desktop` のインストーラーをダブルクリックして起動する。

    | ファイル名                     |
    |------------------------------|
    | Docker Desktop Installer.dmg |

    2-2. 




## **3- Dockerイメージのダウンロード**

=== "Windows"

    3-1. コンソールを開きます
    
    

=== "macOS"

    3-1. ターミナルを開きます

    
    
    3-2. Docker(-compose)定義のダウンロード
    ```Bash
    wget https://e-frontier.systems/cardano-tools/airgap-1.0.5.tar.gz -O airgap.tar.gz
    ```
    
    3-3. ダウンロードファイルのハッシュ値を確認
    !!! warn "ハッシュ値を確認する理由"
        アップロードされたファイルが改ざんされていないかを確認する必要があります！
    ```Bash    
    shasum -a 256 airgap.tar.gz
    ```
    > 9914f544f326e8c50919d013cab202452b92b71f03d0914c5906fe3fec2a6ae8
    
    
    3-4. ダウンロードファイルの解凍
    ```Bash
    tar xvf airgap.tar.gz
    ```


    3-5. 解凍したフォルダへ移動します
    ```Bash
    cd airgap
    ```
    
    
## **4- エアギャップの初期設定**

### 4-1. ビルド

```Bash
./build.sh
```
!!! warning "エラーになる場合"
    ```Bash
    chmod +x *.sh
    ```
    それでもエラーになる場合は、`ls`コマンドで`Dockerfile`などが見えるか確認してください！<br />
    `airgap`ディレクトリ内で実行をする必要があります。


### 4-2. 起動

```Bash
./start.sh
```


### 4-3. ログイン

```Bash
./login.sh
```
コンソールの表記が変わります。

> To run a command as administrator (user "root"), use "sudo <command>".
> See "man sudo_root" for details.
> 
> cardano@a48d8df1282e:~$


試しに、`cardano-cli`のバージョンを確認してみましょう！
```Bash
cardano-cli version
```


おめでとうございます🎉
これでエアギャップが構築されました！


### 4-4. コールドキーを準備

`airgap`フォルダ内の`share`フォルダ内に、`cold-keys`フォルダと、`cnode`フォルダがあらかじめ準備されています。
各フォルダ内には空のファイルが用意されていますので、既存のエアギャップより同じファイル名のファイルを上書きコピーしてください。



### 4-5. コールドキーをエアギャップに取り込む

先ほど上書きしたファイルをエアギャップに取り込みます。
先ほどエアギャップにログインしたコンソール上で以下のコマンドを実行してください。
```Bash
copy-keys
```

!!! admonition "注意"
    正常にコピーされた事を確認した後、先ほど上書きコピーした`share`フォルダ内の`cold-keys`フォルダと、`cnode`フォルダを**必ずUSBメモリ等にバックアップしたうえで削除してください！**



## **6- ログアウトと停止**

### 6-1. ログアウト

エアギャップからログアウトするには`exit`コマンドを実行するだけです。
```Bash
exit
```


### 6-2. 停止

ログアウトしたままではエアギャップは動作し続けています。停止させるには以下のコマンドを更に実行してください。
```Bash
./stop.sh
```



## **7- 共有フォルダについて**

7-1. 共有フォルダについて

!!! info "ホストOSとエアギャップ間の共有フォルダ"
    ``` mermaid
    graph LR
        A["./share"] <-->|共有フォルダ| B["/mnt/share"];
    ```



## **8- 各種操作について**

### **8-1. cardano-cliのバージョンアップ**

1. `./share`フォルダに新しいバージョンの`cardano-cli`をUSBメモリなどで持ってきて貼り付けます。
2. エアギャップにログインして、以下のコマンドを実行します。
   ```Bash
   sudo mv /mnt/share/cardano-cli /usr/local/bin/cardano-cli
   ```
3. エアギャップにて、以下のコマンドを実行しバージョンを確認します。
   ```Bash
   cardano-cli version
   ```


### **8-2. 報酬出金時トランザクションに署名する**

`./share`フォルダに`tx.raw`を貼り付けます。
```Bash
sign
```
`./share`フォルダに`tx.signed`が出力されます。

### **8-3. KESの更新**








## **9- 複数プールへの対応**

9-1. 複数プールへの対応も可能です

複数のプールを運営されている方も1台のマシンで複数のエアギャップを運用することが可能です。

