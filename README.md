## DjangoのDockerテンプレート
### これはなに？

PythonのWebフレームワークである`Django`をDockerを使い環境を構築できるテンプレートです。
基本的には[Docker公式に記載されているもの](https://docs.docker.jp/compose/django.html)を参考にしています。

### 使い方
#### Dockerのインストール

Dockerをインストールしてください。
[公式サイト](https://docs.docker.com/get-docker/)から使用されているOSのDockerをインストールしてください

#### テンプレートのダウンロード

`Git`をお使いの場合は以下のどちらかのコマンドで本テンプレートをダウンロードしてください。

```bash
# HTTPSの場合
git clone https://github.com/S-H-GAMELINKS/django-docker-template.git

# SSHの場合
git clone git@github.com:S-H-GAMELINKS/django-docker-template.git
```

`Git`を使われていない場合はZIP形式でダウンロードしてください。

#### Dockerコンテナのイメージをビルド

ダウンロードしてきた`django-docker-template`ディレクトリまでターミナルで移動し、`docker-compose build`コマンドでコンテナのイメージをビルドします。

```bash
docker-compose build
```

エラーなどなく終了すればOKです。

#### Djangoのアプリを作成

`docker-compose run web django-admin startproject <アプリ名> .`とターミナルで実行して`Django`のアプリを作成します。
アプリ名の部分には`helloworld`や`blog`のようにアプリの名前に置き換えましょう。

```bash
docker-compose run web django-admin startproject <アプリ名> .
```

こちらもエラーなどなく終了すればOKです。

アプリが作成されると`django-docker-template`ディレクトリ内にしていしたアプリの名前のディレクトリが作成されていると思います。(ex: `helloworld`や`blog`など)

その作成されているアプリのソースコードを編集できるように`sudo chown -R $USER:$USER .`を実行します。
Windowsの場合はWSLを使って実行しましょう。

```bash
sudo chown -R $USER:$USER .
```

これでアプリの作成は完了です。

#### データベースへの接続設定

先ほど作成したアプリ名のディレクトリ内にある`settings.py`を編集し、Dockerで作成したデータベースコンテナに接続できるようにします。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

上記の部分を下記のように差し替えます。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

これでデータベースへ接続ができるようになりました。

#### アプリを起動してみる。

最後に`docker-compose up`をターミナルで実行して、アプリを起動してみましょう。

```bash
docker-compose up
```

起動後、`localhost:8000`とブラウザに入力してDjangoの画面が表示されていればOKです。

## 参考

ref: [クィックスタート: Compose と Django](https://docs.docker.jp/compose/django.html)
ref: [django クイックインストールガイド](https://docs.djangoproject.com/ja/3.2/intro/install/)
ref: [django.db.utils.OperationalError: fe_sendauth: no password supplied
](https://stackoverflow.com/questions/36214127/django-db-utils-operationalerror-fe-sendauth-no-password-supplied)

