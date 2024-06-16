# Paradigm CTF

このリポジトリにはParadigm社が提供するCTF(Capture The Flag)の2021年版から2023年版までが収録されています。

## 利用手順

### Codespacesを利用する場合

このリポジトリをテンプレートとしてCodespacesを作成します。

2023年版のhello-worldチャレンジに挑戦に回答する場合の流れについて書いていきます。

今回はCodespaces上にプリインストールされていないnetcat(nc)コマンドを導入します。

```bash
sudo apt update
sudo apt install netcat
```

1つ目のターミナルを起動し、paradigmctf.pyディレクトリでDockerコンテナを起動します。

```bash
cd paradigmctf.py
docker compose up
```

2つ目のターミナルを起動し、paradigm-ctf-2023内にある課題の中から1つ選んでそのプロジェクトディレクトリでDockerコンテナを起動します。

```bash
cd paradigm-ctf-2023/helo-world/project
docker compose up
```

3つ目のターミナルを起動し、ncコマンドでチャレンジに接続すると各種環境情報が表示されるので、その情報を用いて問題に答えます。

```bash
nc localhost 1337
```

```
1 - launch new instance
2 - kill instance
3 - get flag
action? 1
```
1のlaunch new instanceを選択すると、ローカルブロックチェーンのRPCエンドポイントと利用可能なウォレットの秘密鍵と問題を提供するコントラクトのアドレスが提供されます。このインスタンスには制限時間があり、10分程度で機能停止する仕様となっています。

```
creating private blockchain...
deploying challenge...

your private blockchain has been set up
it will automatically terminate in 1440 minutes
---
rpc endpoints:
    - http://127.0.0.1:8545/ANLniBJcMMoIbciXHlCIwPoN/main
private key:        0x7818c7a1f504de5e8f8fee2cbc347f40be2ba64bd6a50fabc400d013064cc676
challenge contract: 0xd060675a4E3452A7515559cEB2CeB7d0899D73dF
```

とりあえずの動作確認という事で用意された回答のステップをなぞってみたいと思います。

まず、これらの情報を扱いやすくする為に、環境変数として出力します。

```
export RPC_URL=http://127.0.0.1:8545/ANLniBJcMMoIbciXHlCIwPoN/main
export PRIVATE_KEY=0x7818c7a1f504de5e8f8fee2cbc347f40be2ba64bd6a50fabc400d013064cc676
export CHALLENGE=0xd060675a4E3452A7515559cEB2CeB7d0899D73dF
```

回答の際に重要となるのは、lib/forge-ctf/src/CTFSolver.solとscript/Solve.s.solの2つのファイルです。

lib/forge-ctf/src/CTFSolver.solには秘密鍵がハードコードされており、この情報を変更する必要があります。

```
...
function run() external {
        uint256 playerPrivateKey = vm.envOr("PLAYER", uint256("この部分を秘密鍵のアドレスに変更する"));
        address challenge = vm.envAddress("CHALLENGE");
...
```

script/Solve.s.solを見ると、回答を行ってくれるコントラクトと関数が既に用意されています。従って、これを呼び出すだけです。

```
forge script script/Solve.s.sol:Solve --rpc-url $RPC_URL --broadcast --private-key $PRIVATE_KEY
```

この手順の後にもう一度`nc localhost 1337`でチャレンジに接続し3のget flagを選択すると、
```
PCTF{flag}
```

これがフラグをゲットです。おめでとうございます。

その他の回答の手順についてはそれぞれの年の問題のREADMEを参考にしてください。

### Local環境(Linux)を利用する場合

仮想化されたコンテナ環境の代表的アプリケーションであるDockerがインストールされている必要があります。

Dockerのインストール手順は以下の通りです。

`
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
`

`
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
`

`
sudo apt-key fingerprint 0EBFCD88
`

`
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
`

`
sudo apt-get update
`

`
sudo apt-get install docker-ce docker-ce-cli containerd.io
`

権限の問題でエラーが出る場合は、端末を再起動してみてください。

参考URL:https://qiita.com/kutinasi_hobby/items/f5dedb8cd1942fb281d7

残りの手順はCodespacesの場合と同じです。
