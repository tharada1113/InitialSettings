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
- RSAアルゴリズム
- 4096ビット

# 秘密鍵管理
```ruby
Include */config
```

~/.ssh/git