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

## (4)

3時間20分

35分

1/18 15:10~15:35, 16:45~16:55

1時間(復習+続き、を含む)

1/28 17:35~18:00,19:55~20:30

1時間

1/29 11:00~11:30,17:00~17:30

45分

2/1 18:45~19:05,20:05~20:20

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

## (4) [チュートリアル2](https://docs.djangoproject.com/ja/3.1/intro/tutorial02/)

### [0] チュートリアル2の前に

secret keyをGitHubにアップロードしないようにした。

具体的には、settings_local.pyを作り、.gitignoreでアップロードしないようにした。

### [1] Databaseの設定(各種設定の確認)

settings.pyの`DATABASES`に設定が書かれている。デフォルトではSQLite3であるが、**本番環境で使用するにはより拡張のしやすいデータベースを使う**。詳しくは[これ](https://docs.djangoproject.com/ja/3.1/ref/settings/#databases)を参照する。タイムゾーンも変えておく。

`INSTALLED_APPS`はこのDjangoプロジェクトで有効なアプリの一覧。これらのアプリはデータベースを必要とする。以下のコマンドでデータベースを作成できる(後々確認)。

```
python manage.py migrate
```

`DATABASES`や`INSTALLED_APPS`の設定を参照して作成されるらしい。

### [2] モデルの作成

<!--　ここ誤植あった　-->

モデルとは、格納したいデータに必要なデータの属性と挙動を定義する。migrateはモデルを順にアップデートしていく？？(後々確認)

pollsアプリケーションにはQuestionとChoiceの二つのモデルがある。それぞれは属性を持っている。また、それぞれのChoiceはQuestionに関連づけられる。それぞれのモデルは`django.db.models.Model`のサブクラス。それぞれのクラスはクラス変数を持つ。クラス変数はデータベースのフィールドを表現する。

各フィールドはFieldクラスのインスタンスとして表現される。`CharField`は文字のフィールドで`DateTimeField`は日時のフィールドとなる。また、各フィールドにどのようなデータ型を記憶させるかを与える。

列へのアクセスにはこのクラス変数が使われる。人間が可読可能なフィールド名としてFieldクラスのコンストラクタの最初の位置引数に文字列を指定することも可能。これはドキュメントの役割も果たす。

Fieldには`CharField`の`max_length`のような必須の引数や`IntegerField`の`default`のようなオプション引数もある。

`ForeignKey`は関連づけがあることを示す。これにより、それぞれのChoiceはQuestionに関連づけることがわかる。

### [3] モデルの有効化

1. データベーススキーマ(要復習,諸々を定めたもの)を作成できる ← CREATE TABLEを実行する
2. Pythonからそれぞれのモデルのオブジェクトにアクセスできる

この時点では**アプリケーションはプロジェクトに含まれていない**ので、先にプロジェクトに含める必要がある

設定クラスへの参照を`INSTALLED_APPS`に追加する。ここでは、`polls/apps.py`にある。よって、`settings.py`に`'polls.apps.PollsConfig'`を加えれば良い。

この状態でマイグレーションを行う。

```zsh
% python manage.py makemigrations polls
Migrations for 'polls':
  polls/migrations/0001_initial.py
    - Create model Question
    - Create model Choice
```

マイグレーションとは、Djangoがモデル(データベーススキーま)の変更を保存する方法のこと。`polls/migrations`に可読なpythonファイルとして保存。

migrateコマンドでデータベーススキーマは自動で管理できる。ここではマイグレーションでどんなSQLを実行するのかを確認する。ここではSQLite3を用いた時の出力。

```zsh
% python manage.py sqlmigrate polls 0001
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" integer NOT NULL REFERENCES "polls_question" ("id") DEFERRABLE INITIALLY DEFERRED);
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");
COMMIT;
```

#### 注意

- テーブルやキーの名前は変更可能
- sqlmigrateはDjangoの必要とするSQLの確認用のコマンド

実際にマイグレーションを適用する。

```shell
% python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Applying polls.0001_initial... OK
```

モデルに対して行った変更はデータベースのスキーマに同期する。

#### マイグレーションの流れ

プロジェクトの更新に合わせてモデルを変更することができ、データを失わずにデータベースの更新を行うことができる。とりあえずは以下の流れ

1. モデルを変更する(`models.py`)
2. 変更のためのマイグレーションの作成(`python manage.py makemigrations`)
3. データベースにマイグレーションを適用する(` python manage.py migrate`)

manage.pyの他の機能は[これ](https://docs.djangoproject.com/ja/3.1/ref/django-admin/)


### [4] APIで遊ぶ

Pythonシェルの起動(管理用のコマンドシェル)し、APIを叩いてみると以下のようになる。

```shell
% python manage.py shell
Python 3.9.1 (default, Dec 24 2020, 16:23:16) 
[Clang 12.0.0 (clang-1200.0.32.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from polls.models import Choice, Question
>>> Question.objects.all()
<QuerySet []>
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2021, 1, 29, 2, 2, 6, 432093, tzinfo=<UTC>)
>>> q.question_text = "What's up?"
>>> q.save()
>>> q.question_text
"What's up?"
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
```

ここで、`Question.objects.all()`の出力を変えるには、モデルを定義したクラスの`__str__`を変えれば良い。クラスメソッドを追加することも可能。

```txt
% python manage.py shell
>>> from polls.models import Choice, Question
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>
# filterをかけることも可能
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>
>>> from django.utils import timezone
>>> current_year = timezone.now().year
# filterではなくgetでも可能
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>
# id=2は存在しない
>>> Question.objects.get(id=2)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/usr/local/lib/python3.9/site-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/usr/local/lib/python3.9/site-packages/django/db/models/query.py", line 429, in get
    raise self.model.DoesNotExist(
polls.models.Question.DoesNotExist: Question matching query does not exist.
#今回はprimary_keyがidなので同じこと
>>> Question.objects.get(pk=1)
<Question: What's up?>
>>> q = Question.objects.get(pk=1)
#メソッドを呼ぶことも可能
>>> q.was_published_recently()
False
#Questionのオブジェクトをqに渡す
>>> q = Question.objects.get(pk=1)
#choiceの全オブジェクトをqに対応させる
#この時点ではchoiceは空
>>> q.choice_set.all()
<QuerySet []>
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
#cに対応するquestionもとってこれる
>>> c.question
<Question: What's up?>
>>> q.choice_set.count()
3
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
(1, {'polls.Choice': 1})
```

データベースの検索方法について、"フィールド名__演算子=比較値"になるので注意
その他にも__の表記はあるみたい→[フィールドルックアップ](https://docs.djangoproject.com/ja/3.1/topics/db/queries/#field-lookups-intro)

### [4] Django Admin の紹介

#### 管理ユーザーの作成

```python
% python manage.py createsuperuser
```

上記のコマンドを打つと、Username,Email address,Passwordを決めるように要求される。

#### 開発サーバーの起動

adminのサイトはデフォルトで有効化されるので、以下のコマンドで起動してadminのサイトにアクセスする。ここでは、ローカルなので、http://127.0.0.1:8000/admin/ にアクセスすれば良い.

```shell
% python manage.py runserver
```

### adminサイトへのログイン

ログインするだけ

### Pollアプリをadmin上で編集できるようにする

polls/admin.pyで編集して"adminインターフェース"を持つことを伝える。(コードをコピペ)

### admin の機能を探究してみる

先程の操作により、adminのページにQuestionの編集画面が出てくる。

この編集画面はQuestionモデルから自動で生成される。また、それぞれのフィールドは異なるHTML入力ウィジェットに対応するようになっている。

面白いね！ということがわかる。GUIでモデルへの変更が可能。
