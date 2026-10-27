# Template for m0t0k1ch1-go

m0t0k1ch1-go 配下の Go リポジトリへ配布する設定ファイル。

## 構成

- `template/`：各 Go リポジトリへ配布するファイル。消費側リポジトリのルートの鏡。
  - `.husky/commit-msg`：commitlint を実行する `commit-msg` フック。
  - `commitlint.config.ts`：commitlint の設定。
  - `package.json`：開発に使う npm パッケージの定義。
  - `pnpm-lock.yaml`：`package.json` のロックファイル。
  - `staticcheck.conf`：staticcheck の設定。
  - `template.mk`：共有する make ターゲット。
- その他：本リポジトリ自身の開発用設定。

## 使い方

消費側リポジトリでは、**同期** の項の手順で `template/` の内容を取り込み、初回は加えて以下を行う。

- `Makefile` に `include template.mk` を書き、リポジトリ固有のターゲットを続ける。`template.mk` にあるターゲット名は再定義しない。`lint` と `test` は必ず定義する。

## 同期

`template/` を更新したら、各消費側リポジトリごとに以下の手順で同期を行う。

1. 本リポジトリの main ブランチをクローンする（例：`git clone --depth 1 https://github.com/m0t0k1ch1-go/template /tmp/template`）。
2. 消費側リポジトリのルートで `cp -RL /tmp/template/template/. .` を実行する（シンボリックリンクは実体化される）。
3. `template/` から削除されたファイルがあれば、消費側リポジトリからも削除する。
4. `git status` で、変更されたファイルが `template/` にあるもの（削除されたものを含む）だけであることを確認する。
5. `chore: sync template` のようなコミットを作り、PR を出す。
