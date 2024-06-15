# Paradigm CTF

このリポジトリにはParadigm社が提供するCTF(Capture The Flag)の2021年版から2023年版までが収録されています。

## 利用手順

### Codespacesを利用する場合

このリポジトリをテンプレートとしてCodespacesを作成します。

2023年版のhello-worldチャレンジに挑戦する場合の流れについて書いていきます。

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

3つ目のターミナルを起動すると各種環境情報が表示されるので、その情報を用いて問題に答えます。

```bash
nc 127.0.0.1 1337
```

回答の手順についてはそれぞれの年のREADMEを参考にしてください。

### Local環境(Linux)を利用する場合

仮想化されたコンテナ環境の代表的アプリケーションであるDockerがインストールされている必要があります。

Dockerのインストール手順は以下の通りです。

```
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```
sudo apt-key fingerprint 0EBFCD88
```

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

権限の問題でエラーが出る場合は、端末を再起動してみてください。

参考URL:https://qiita.com/kutinasi_hobby/items/f5dedb8cd1942fb281d7

残りの手順はCodespacesの場合と同じです。
