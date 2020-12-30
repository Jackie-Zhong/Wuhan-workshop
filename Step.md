# three validators & two parachain !!!

- 视频: https://www.bilibili.com/video/BV1CV411h7cR

## 1. 获取Raw文件

```js
git clone https://github.com/ParityAsia/Wuhan-workshop.git获取rococo-3.json文件
```

## 2. 启动验证节点

```js
./target/release/polkadot --tmp --ws-port 9944 --port 30333 --rpc-port 8866 --chain rococo-3.json --alice --validator

./target/release/polkadot --tmp --ws-port 9945 --port 30334 --rpc-port 8867 --chain rococo-3.json --bob --validator

./target/release/polkadot --tmp --ws-port 9946 --port 30335 --rpc-port 8868 --chain rococo-3.json --charlie --validator

# 视频里的命令
./target/release/polkadot --chain rococo-3.json --tmp --ws-port 9944 --port 30333 --alice
```

## 3.  启动平行链

```js
./target/release/parachain-collator export-genesis-state --parachain-id 200 > para-200-genesis
./target/release/parachain-collator export-genesis-state --parachain-id 300 > para-300-genesis
./target/release/parachain-collator export-genesis-wasm > para-wasm

./target/release/parachain-collator --ws-port 9947 --port 30336 --tmp --parachain-id 200 --validator -- --chain ../polkadot/rococo-3.json

./target/release/parachain-collator --ws-port 9948 --port 30337 --tmp --parachain-id 300 --validator -- --chain ../polkadot/rococo-3.json
```

## 4.  配置浏览器

```js
# Developer -->  extrinsics 快速注册平行链。如果是线上的话就要拍卖。我们这里用sudo来演示。

使用已选的账户
ALICE

提交下面的外部消息
sudo    sudo(call)

call:Call
registrar registerParathread(id,genesis_head,validation_code)

id:ParaId
200

validation_code:ValidationCode
para_wasm(472,398bytes)

genesis_head:HeadData
para-200-genesis(98bytes)

# Settings --> Developer 前端配置 （用这个workshop，这里必须配置）
{
  "Address":"Accountid",
  "LookupSource":"Accountid"
}
```

## 5. 跨链

```js
# HMP(Horizontal Message Passing)
在平行链之间传递的消息

# VMP(Vertical Message Passing)
在 Relay Chain 和 Parachain 之间传递的消息
- DMP(Relay Chain 转给 Parachain)
- UMP(Parachain 转给 Relay Chain)
```

> DMP 中继链给平行链传消息，现在中继链只有线上治理，转账DOT,KSM
>
> UMP 平行链给中继链传消息

### 5.1 跨链转账（DMP）

```js
# Parachain 9947 中继链转给平行链
# Parachains --> transfer to chain 
transfer to chain

send from account
ALICE

send to address
ALICE

the parachain to transfer to
200

amount
300
```

### 5.2  跨链转账（UMP）

```js
# Parachain 9947 平行链转给中继链
# Parachains --> transfer to chain 
transfer to chain

send from account
ALICE_STASH

send to address
ALICE_STASH

amout
300
```

### 5.3 平行链之间转账(Step 1)

```js
# 在 parachain 9948 先给 parachain 9947 转账
# Account --> transfer
send from account
ALICE_STASH

send to address
12D3KooWPBdkNoLdSrGZPZb5hKdBNo5973ZNkG3p3vfjuHdvX8Fw // parachain 9947 的 accountID

amount
3000
```

### 5.3 平行链之间转账(Step 2)

```js
# parachain 9948 
# Parachains --> transfer to chain 
send from account
BOB

send to address
BOB

the parachain to transfer to
300

amount
2000
```
