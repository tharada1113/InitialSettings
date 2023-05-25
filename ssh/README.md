# キー作成
```
ssh-keygen
```
- デフォルト下記で作成
    - RSAアルゴリズム
    - 2048ビット
    - RFC4716形式(他に"RFC4716","PEM"等がある)
# キー作成オプション例
```
ssh-keygen -t rsa -b 4096
ssh-keygen -m PEM -t rsa -b 4096
```
- RSAは歴史のある公開鍵暗号方式なのでいまだに広く使われているが、徐々によりセキュアなEdDSAが普及
    - 実装や互換性を重視する場合は公開鍵の鍵長が2048bitもしくは4096bitのRSA
    - パフォーマンスやセキュリティを重視する場合はEdDSAを推奨らしい

EdDSAの例
```
ssh-keygen -t ed25519
```

# 秘密鍵管理
- 鍵を前と同じ場所に作成すると上書きされる
    - 何も考えずに作ると上書きされる鍵`~/.ssh/id_rsa`
- ということで鍵の管理を階層型にする
    - `~/.ssh/config`にInclude設定をして各ディレクトリ毎にconfigと鍵を配置する

~/.ssh/config
```
Host *

Include */config
```
- 未調査（害はない)
    - UseKeychain

階層構成
```
~/.ssh/
├── config ... すべてのサーバに共通した設定を書くのと、下位のconfigをIncludeする
├── id_rsa... よく使う鍵
├── id_rsa.pub ... よく使う鍵
├── known_hosts
├── github ... プロジェクトごとにディレクトリを作り
│   ├── config ... プロジェクトごとにconfigファイルを置く
│   ├── id_ed25519 ... GitHubはEd25519(EdDSAの一種)を推奨
│   └── id_ed25519.pub
├── azurevm
│   ├── config
│   ├── id_rsa
│   └── id_rsa.pub
```
~/.ssh/github/config
```
Host github github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github/id_ed25519
```
- `User git`は固定値