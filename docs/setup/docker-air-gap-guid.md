# **Dockerエアギャップマシン作成**

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
    * airgap-1.0.3.tar.gz

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
    wget https://e-frontier.systems/cardano-tools/airgap-1.0.3.tar.gz -O airgap.tar.gz
    ```
    
    3-3. ダウンロードファイルのハッシュ値を確認
    !!! warn "ハッシュ値を確認する理由"
        アップロードされたファイルが改ざんされていないかを確認する必要があります！
    ```Bash    
    shasum -a 256 airgap.tar.gz
    ```
    > 104ca60f9b6ead5841dff02bcc70f2477f5abbc67c10a35c5bd8ccad6866bf5d
    
    
    3-4. ダウンロードファイルの解凍
    ```Bash
    tar xvf airgap.tar.gz
    ```


    3-5. 解凍したフォルダへ移動します
    ```Bash
    cd airgap
    ```
    
    
## **4- Dockerイメージのビルド**

4-1. エアギャップのビルド

```Bash
./build.sh
```


## **5- Dockerイメージの実行**

5-1. エアギャップの実行

```Bash
./boot.sh
```


## **6- Dockerイメージへログイン**

6-1. エアギャップへのログイン

```Bash
./login.sh
```


## **7- 共有フォルダなどについて**

7-1. 共有フォルダについて

!!! info "ホストOSとエアギャップ間の共有フォルダ"
    ``` mermaid
    graph LR
        A["./share"] <-->|共有フォルダ| B["/mnt/share"];
    ```



## **8- 各種操作について**

### **8-1. 報酬出金時トランザクションに署名する**

`./share`フォルダに`tx.raw`を貼り付けます。
```Bash
sign
```
`./share`フォルダに`tx.signed`が出力されます。

### **8-2. KESの更新**




