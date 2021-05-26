
# 準備

## 必要なパッケージのインストール

ansibleに必要な関連パッケージは全てpip(pip3)でインストールした。

```
pip3 install ansible openstacksdk
```

## Ansible GalaxyからOpenStackのCollectionをインストール

```
# collectionsをインストールするディレクトリを先に用意する。
mkdir collections

# requirements.ymlに記載しているcollectionsを./collectsions/にインストールする
ansible-galaxy collection install -r requirements.yml -p collections/
```

## FJcloud-oの認証情報(clouds.yml)を用意する

`clouds.yml.sample`をコピーするなどして、`clouds.yml`を作成する。
> こちらを参考にしてください
> [openstack | Configuration](https://docs.openstack.org/python-openstackclient/pike/configuration/index.html)

なお、`clouds.yml.sample`のようにクラウドを指定する `default:` は変更できますが、次のような名前はそのまま使用できないのでダブルクォーテーションで囲む必要があります。
- 数字から始まる(例: 3hands) => `"\"3hands\""`
- 記号が含まれる(例: jp-east-3) => `"\"jp-east-3\""`

また、playbookを実行する環境のclouds.ymlに複数のクラウドの認証情報を登録している場合や、`default:`以外の名前を指定した場合は、extra_varsで`os_cloud:`パラメータでクラウド名を指定してください。
  ```
  os_cloud: hogehoge
  ```

## このplaybookで作成できるリソース
* Networking
  * Network
  * Subnet
  * Router
  * Security Group(未完成)
  * LoadBalancer(未完成)
* Compute
  * Keypair 
  * Server(未完成)
* API
  * Firewall
  * Firewall Policy
  * Firewall Rule 


## Playbookの実行
```
# デフォルト値のまま実行する場合
ansible-playbook example.yml

# extra_varsを使用する場合
ansible-playbook example.yml -e @extra_vars.yml

```


# 各ロールに必要な変数と戻り値

ロールを単体で使用する場合に、defaults/main.yml以外で指定する必要がある変数を以下で説明する。

## opensatck_networking
### 戻り値
- `router_id` (UUID)

## openstack_compute
### Key Pair

## API Common
### 戻り値
- `x_auth_token` (トークン)

## API Firewall
### 必須変数
- `x_auth_token` (トークン)
- `router_id` (Firewallを適用するルータID)