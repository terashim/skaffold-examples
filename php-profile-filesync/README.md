Skaffold の利用例 - Profile と File Sync でイメージの頻繁な再ビルドを避ける
-----

## 目的

[Skaffold](https://skaffold.dev/)
を Web サイト開発等のローカル開発環境に使用するとき、
ソースファイルを少し修正するたびに Docker イメージをビルドして確認するのはリソースや時間の効率が悪い。

そこで
[File sync](https://skaffold.dev/docs/how-tos/filesync/) の仕組みを使ってイメージをビルドせずにコンテナ内のファイルを同期する。

**注意: File Sync はまだアルファ版の機能なので今後のバージョンで変更される可能性がある**

必要に応じてイメージをビルドしたときの挙動も確認したいので、
[Profile](https://skaffold.dev/docs/how-tos/profiles/)
を利用してファイル同期する設定とビルドする設定とを切り替える。

---

## 使い方

通常のローカル開発ではファイルを編集・保存するたび Docker イメージをビルドせずファイル同期だけ行う:

```sh
skaffold dev --profile dev-filesync
```

ときどき確認のため Docker イメージのビルドを実行する:

```sh
skaffold run
```

---

## 概略

ファイルの構成はこのようになっている:

```
.
├── README.md
├── k8s
│   └── manifest.yaml
├── php
│   ├── Dockerfile
│   └── src
│       └── index.php
└── skaffold.yaml
```

Docker イメージをビルドするときは `php/Dockerfile` 内の `COPY` コマンドによって、
ファイル同期するときは `skaffold.yaml` 内に記述された File Sync の設定によって、
それぞれ PHP ソースファイル `php/src/index.php` がコンテナの `/var/www/html/index.php` にコピーされる。
