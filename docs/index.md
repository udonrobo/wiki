---
hide:
  - navigation
---

# このページについて

mkdocs と mkdocs-material でページを生成し、GitHub Pages でページを配信しています。

内容はマークダウン形式で、[udonrobo/Wiki](https://github.com/udonrobo/Wiki) レポジトリで管理しています。

!!! note "mkdocs とは"

    マークダウンからホームページを生成する静的サイトジェネレーターです。[mkdocs ドキュメント](https://www.mkdocs.org/)

!!! note "mkdocs-material とは"

    mkdocs の拡張テーマです。 [mkdocs-material ドキュメント](https://squidfunk.github.io/mkdocs-material)

## 環境構築

mkdocs インストール

```sh
pip install mkdocs
pip install mkdocs-material
```

## 編集

VSCode 等で本レポジトリを開きます。本レポジトリのディレクトリ下で以下コマンドを実行すると、ローカルサーバーとブラウザが起動します。

```sh
python -m mkdocs serve --open
```

マークダウンファイルを編集し保存すると、自動的に変更が反映されます。

## 公開

GitHub へプッシュすると GitHub Actions によって自動的に公開されます。
