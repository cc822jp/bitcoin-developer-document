# Block Chain

The block chain provides Bitcoin’s public ledger, an ordered and timestamped record of transactions. This system is used
to protect against double spending and modification of previous transaction records.

ブロックチェーンは、ビットコインの公開台帳であり、取引の順序とタイムスタンプの記録を提供します。このシステムは 二重支出や過去の取引記録の改ざんを防ぐためです。

## Introduction

Each full node in the Bitcoin network independently stores a block chain containing only blocks validated by that node.
When several nodes all have the same blocks in their block chain, they are considered to be in consensus. The validation
rules these nodes follow to maintain consensus are called consensus rules. This section describes many of the consensus
rules used by Bitcoin Core.

ビットコインネットワークの各フルノードは、独立して、そのノードによって検証されたブロックのみを含むブロックチェーンを保存します。
複数のノードがすべて同じブロックをブロックチェーンに持つとき、そのノードはコンセンサスに達しているとみなされる。検証
これらのノードがコンセンサスを維持するために従うルールをコンセンサスルールと呼びます。このセクションでは、多くのコンセンサス は、Bitcoin Core で使用されるルールです。

![Simplified Bitcoin Block Chain](../assets/images/en-blockchain-overview.svg)

The illustration above shows a simplified version of a block chain. A block of one or more new transactions is collected
into the transaction data part of a block. Copies of each transaction are hashed, and the hashes are then paired,
hashed, paired again, and hashed again until a single hash remains, the merkle root of a merkle tree.

上の図は、ブロックチェーンを簡略化したものである。1つまたは複数の新しい取引のブロックが収集され を1ブロックの取引データ部に格納する。各取引のコピーはハッシュ化され、そのハッシュは対にされる。
ハッシュ化、ペア化、ハッシュ化を繰り返し、一つのハッシュが残り、これがメルクル木のメルクル根となる。

The merkle root is stored in the block header. Each block also stores the hash of the previous block’s header, chaining
the blocks together. This ensures a transaction cannot be modified without modifying the block that records it and all
following blocks.

メルクルルートはブロックヘッダに格納される。各ブロックには、前のブロックのヘッダーのハッシュも格納され、連鎖的に を使用します。これにより、ある取引が、それを記録したブロックとすべての のブロックに続く。

Transactions are also chained together. Bitcoin wallet software gives the impression that satoshis are sent from and to
wallets, but bitcoins really move from transaction to transaction. Each transaction spends the satoshis previously
received in one or more earlier transactions, so the input of one transaction is the output of a previous transaction.

また、取引は連鎖的に行われる。ビットコインのウォレットソフトウェアは サトシを送受信しているように見せますが しかし、ビットコインは実際には取引から取引へと移動します。各取引は、以前に使用されたサトシを使用します。
そのため、ある取引の入力は、前の取引の出力となる。

A single transaction can create multiple outputs, as would be the case when sending to multiple addresses, but each
output of a particular transaction can only be used as an input once in the block chain. Any subsequent reference is a
forbidden double spend—an attempt to spend the same satoshis twice.

複数のアドレスに送信する場合のように、1つの取引で複数のアウトプットを作成することができますが、それぞれの の出力は、ブロックチェーン上で一度だけ入力として使用することができます。それ以降の参照は
同じサトシを二度使うことは禁じられています。

Outputs are tied to transaction identifiers (TXIDs), which are the hashes of signed transactions.

出力は、署名されたトランザクションのハッシュであるトランザクション識別子（TXID）に結びつけられる。

Because each output of a particular transaction can only be spent once, the outputs of all transactions included in the
block chain can be categorized as either Unspent Transaction Outputs (UTXOs) or spent transaction outputs. For a payment
to be valid, it must only use UTXOs as inputs.

特定の取引の各アウトプットは一度しか使うことができないため、その取引に含まれるすべての取引のアウトプッ ブロックチェーンは、未消費取引出力（UTXO）と消費取引出力のいずれかに分類されます。支払い
が有効であるためには、入力としてUTXOのみを使用しなければならない。

Ignoring coinbase transactions (described later), if the value of a transaction’s outputs exceed its inputs, the
transaction will be rejected—but if the inputs exceed the value of the outputs, any difference in value may be claimed
as a transaction fee by the Bitcoin miner who creates the block containing that transaction. For example, in the
illustration above, each transaction spends 10,000 satoshis fewer than it receives from its combined inputs, effectively
paying a 10,000 satoshi transaction fee.

コインベース取引（後述）を除き、ある取引のアウトプットの値がインプットを上回った場合、その取引は成立しない。 しかし、入力が出力の値を上回っている場合、その差分は請求することができる。
そのトランザクションを含むブロックを作成したビットコインの採掘者がトランザクション手数料として 例えば、この例では 上の図では、各トランザクションは、その合計入力から受け取るよりも少ない10,000サトシを費やし、実質的に
10,000satoshiの取引手数料を支払う。

