# Block-chain Network (Minimal Version)

## 登場するクラスの概要

ブロックチェーンとは何かを一言で言うと、「分散していながら、改竄が困難でデータの信頼性が担保された追記型データベース」と理解している。
これは、User、Transaction、Verifier、Network、Blockという５つのクラスが、相互に作用することによって実現されている。

### User
ブロックチェーンネットワークの参加者を表すクラス。
他のUserとメッセージのやり取りをする。

### Transaction
User間のやり取りを表すクラス。

Transactionには下記の３種類がある。

- MessageTransaction: User間のメッセージのやり取りを表す
- FeeTransaction: User間の過去のメッセージのやり取りを認証するTransaction
- SignedTransaction: MessageTransactionにSignしたもの。普通はPKIなどで暗号化するらしいがここでは省略。

FeeTransactionでは、他のUser間のMessageTransactionを認証することによってFeeを得ることができる。(ビットコインで言うところの採掘(マイニング))

### Verifier

- User間のやり取りを認証するクラス。
- UserはVerifierを継承しているので、全てのUserはVerifierでもある。
- 未認証のTransactionを保持し、Userの誰かがFeeTransactionを起こすとBlockを生成する。

### Network

- ブロックチェーンネットワークそのもの。
- ネットワークに参加しているユーザーにトランザクションの発生をアナウンスする機能を持つ。

### Block

幾つかのTransactionをまとめたデータ。
User(Verifier)が他のUserのTransactionを認証することによって生成される。
Blockを生成するには、計算能力が必要なタスク(ここではcreate_id)をこなす必要がある。これがいわゆる`Proof of Work`。
Blockは一つ前のBlockを指し示すprev_blockという属性を持ち、全Transactionデータはチェーン状に連なったBlockの集まりとして保持される。

## 参考資料
1. [Bitcoinの論文](https://bitcoin.org/bitcoin.pdf)
2. [Minimum Viable Block Chain](https://www.igvita.com/2014/05/05/minimum-viable-block-chain/)
	なぜブロックチェーンが今のような仕組みになっているのか解説した記事。
