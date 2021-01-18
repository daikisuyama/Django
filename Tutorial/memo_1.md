# 全体の目標

自分のPortfoioサイトを作成する。
できればそこから自分のブログを作成し、趣味についても投稿できるようにする

# 1日目の目標

Djangoの概要を把握する。
ローカルでサンプルが動くようにする

# 時間

## (1),(2)

30分

1/17 18:25~18:55 

## (3)

1時間10分

1/17 20:10~20:40

1/18 7:50~8:15, 9:10~8:25 

# やったことのメモ

まずは、[公式のイントロダクション](https://docs.djangoproject.com/ja/3.1/intro/)を読みながら進める。

## (1) [概要](https://docs.djangoproject.com/ja/3.1/intro/overview/)

データモデルの存在
APIを使ってデータにアクセスできること
動的なウェブサイトを作成できること
URLやビューも自作できること
ビューにPythonコードを埋めることができることを把握

→モヤモヤしていたので、より詳しいものを調べようとしたが、良い記事がなかったので断念

## (2) [インストール](https://docs.djangoproject.com/ja/3.1/intro/install/)

Pythonのインストール→済(python 3.9.1)

PythonにはSQliteが含まれているので、データセットの設定はいらない
→Sqlite：ファイル単位で管理するデータベース、[wikipedia](https://ja.wikipedia.org/wiki/SQLite)を参考にした。

Django→最新版(3.1.5)をインストールする
pipでインストールするだけ

## (3) [チュートリアル1](https://docs.djangoproject.com/ja/3.1/intro/tutorial01/)

投票アプリの作成

### [1]プロジェクトの作成

mysiteという名称のプロジェクトをカレントディレクトリに作成する。(Tutorial配下のpollに作成)

```python
django-admin startproject mysite
```

#### ・注意

- プロジェクト名は定義済みの名称との衝突を避ける。
- コードはドキュメントルートは以下には置かない。

#### ・What's in the project ?

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

- manage.py 
    - Djangoのプロジェクトへの操作を行うコマンドラインユーティリティ
    - 後々把握する
- mysiteディレクトリ
    - このプロジェクトのPythonパッケージ
    - \_\_init\_\_.py
        - Pythonパッケージであることを知らせるファイル
    - settings.py
        - Djangoプロジェクトの設定ファイル
    - urls.py
        - DjangoプロジェクトのURL宣言
    - asgi.py,wsgi.py
        - ASGI(WSGI)互換Webサーバーとのエントリーポイント
        - 後々把握する

### [2]サーバーを立てる

```python
python manage.py runserver
```

上のコマンドを実行して http://127.0.0.1:8000/ にアクセスする。

#### 注意

- データベースの問題でエラーが出てる
- 開発サーバーで運用サーバーには使わない
- サーバーのポート番号も変えることができる→[参考](https://docs.djangoproject.com/ja/3.1/ref/django-admin/#django-admin-runserver)

### [3]Pollsアプリケーションの作成

以下でpollsというアプリケーションが作成される。

```python
python manage.py startapp polls
```

ディレクトリの構成は以下になる。pollsの全容。

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

#### 用語

- アプリ：何かを行うWebアプリケーション
- プロジェクト：Webサイトの構成と複数のアプリ

### [4]ビューの作成

`polls/views.py`にコードを貼り付ける。

ビューを呼ぶにはURLを対応づけるには`polls/urls.py`で設定する。これはviewsとかの動的なものを呼び出すっぽい。

このとき、プロジェクトのルートでも設定が必要。path関数で設定し、include関数を呼ぶことで(プロジェクトのルートでのURL)+(アプリでのURL)として分離することができる。URLパターンのインクルードではincludeを必ず使う。ただし、`admin.site.urls`は例外(らしい)。

これで以下のコマンドを打って http://localhost:8000/polls/ にアクセスすると、"Hello, world. You're at the polls index."が表示される。

#### path関数の引数

- route
    - URLパターンを含む文字列
    - リクエストはリストに一致するものがあるかを順に見ていく
    - 通信のGETやPOSTのパラメータやドメイン名を見ない
        - https://www.example.com/myapp/?page=3 では、myapp/をみる
- view 
    - routeがマッチしたときに呼び出されるビュー関数
    - 第一引数にHttpRequestオブジェクト
    - キーワード引数にキャプチャされた値
    - 後々把握する
- kwards
    - キーワード引数を対象のビューに渡せる
    - 後々把握する
- name
    - どこからでも明確に参照することができる

ここまででリクエストで呼び出されることがなんとなく把握することができれば良い

## (3) [チュートリアル2](https://docs.djangoproject.com/ja/3.1/intro/tutorial02/)

