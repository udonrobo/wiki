# コマンド

Windows の場合、コマンドはターミナル上で実行します。

今までエクスプローラーで行っていたファイル操作を、コマンドでもできます。

![alt text](terminal.png)

ターミナルを起動すると、今いるディレクトリが表示されます。

```sh
~/Desktop/dev
$
```

!!! note

    エクスプローラーからも起動できます。(開きたいディレクトリを右クリック)

    ディレクトリに移動した状態で起動してくれます！

    ![alt text](open_from_explorer.png)

## 環境設定

コマンドを実行する環境は PowerShell や Bash などがあります。ここでは Bash を使います。

Bash を Windows で使う理由は、Linux のシェル環境と互換性があり、学習リソースが豊富なためです。実際の開発や運用にも役立つと思います。Git Bash を使うと Windows でも Bash を使えます。

ターミナルを起動後、次のコマンドを入力し Git Bash をインストールします。バージョン管理ツール Git と一緒にインストールできます。

```sh
winget install --id Git.Git -e --source winget
```

新しいプロファイルを追加します。

![alt text](image-2.png){ width="500" }

![alt text](image.png)

各設定項目を入力し、プロファイルの設定をします。

名前(任意)

```txt
Bash
```

コマンドライン

```txt
C:\Program Files\Git\bin\bash.exe --login -i
```

起動ディレクトリ(任意)

```txt
C:\Users\ユーザー名\Desktop
```

アイコン(任意)

```txt
C:\Program Files\Git\mingw64\share\git\git-for-windows.ico
```

![alt text](image-1.png)

作成したプロファイルを既定のプロファイルに設定します。これでターミナルを起動すると Bash が起動します。保存を押して一旦ターミナルを閉じます。

![alt text](image-3.png)

再度ターミナルを起動すると、次のように Bash が起動するはずです。

![alt text](image-4.png)

## 使えると熱いコマンドたち

- 🌟 ls
- 🌟 cd
- 🌟 start
- 🌟 code
- 🌟 winget
- 🌟 git
- mv (移動、名前変更)
- cp (コピー)
- touch (ファイル作成)
- mkdir (ディレクトリ作成)
- ...

使いつつ覚えていけば大丈夫です。エクスプローラーでの操作に対応したコマンドがあることが分かれば、グーグルニキが教えてくれます！

### 🌟 **ls コマンド**

ディレクトリ内のファイル、ディレクトリを列挙する

```sh
ls
```

結果

```sh
~/Desktop/dev
ls

AA/                   ConsoleApplication3/  Harurobo2023-Ateam/  OpenSiv3D_0.6.141/  rust-test/         WeatherApp-1/
addlinenum/           ConsoleApplication4/  IRRemoteControl/     OpenSiv3D_0.6.142/  SortAlgorithm/     Website/
apptest/              ControllerUnit/       ...
```

### 🌟 **cd コマンド**

ディレクトリ間を移動する

```sh
cd <ディレクトリ名>
```

結果

```sh
~/Desktop
cd dev/

~/Desktop/dev
```

`cd ..` で一つ上のディレクトリへ移動できます。

```sh
~/Desktop/dev
cd ..

~/Desktop
```

`cd -` でもと居たディレクトリに戻れます。

```sh
~/Desktop/dev/Robocon2024-Bteam
cd ../../

~/Desktop
cd -

~/Desktop/dev/Robocon2024-Bteam
```

!!! note

    ディレクトリ名は全て入力する必要はありません。途中まで打ち Tab キーを押すことで補完できます。

    ```sh
    cd robo
    ```

    ↓ Tab キー

    ```sh
    cd Robocon2024-Bteam/
    ```

### 🌟 **特殊なディレクトリ**

```sh
~/Desktop/dev
ls -la

total 130022
drwxr-xr-x 1 user 197609         0 12月 15 23:25 ./     ← コレ
drwxr-xr-x 1 user 197609         0 12月 15 01:14 ../    ← コレ
drwxr-xr-x 1 user 197609         0 12月 15 23:14 AA/
```

`.` 今いるディレクトリ

`..` 親ディレクトリ 🌟

`~` ホームディレクトリ (C:/Users/ユーザー名)

`/` ルートディレクトリ (C:/)

### 🌟 **start コマンド**

ファイルやディレクトリを既定のプログラムで開く

エクスプローラー上でクリックするのと同じです。結構使います！

```sh
start <ファイル名 or ディレクトリ名>
```

![alt text](start.png)

### 🌟 **code コマンド**

VSCode を開くコマンド

```sh
~/Desktop/dev/Robocon2024-Bteam
code .
```

結果

![alt text](code.png)

### 🌟 **winget コマンド**

Window 用パッケージマネージャーです。

コマンド一つでアプリをインストールできるので超楽です！

例) Git のインストール <https://git-scm.com/downloads/win>

```sh
winget install --id Git.Git -e --source winget
```
