# {DRAFT}
_# Simple Payment Verification (SPV)_
# 简单支付验证（SPV）

---

_## What is SPV?_
## 什么是SPV？ 

_Simple Payment Verification (SPV) allows the use of a Decred wallet without having to download the entire Decred blockchain. A wallet operating in SPV mode only needs to download full blocks containing transactions relevant to it (i.e. transactions involving the wallet’s addresses). In a typical case, this means downloading tens of megabytes, rather than multiple gigabytes. This reduces the wallet's hardware requirements and greatly reduces the initial load time for new wallets._
简单支付验证（SPV）允许无需下载整个Decred区块链的情况下使用Decred钱包而。SPV模式下操作的钱包仅需要下载包含与其相关交易的完整块（即涉及钱包地址的交易）。在一般情况下，这意味着下载的是数十兆字节而不是数千兆字节。这大大减少了钱包的硬件要求和新钱包的初始加载时间。

_SPV has been built directly into the dcrwallet CLI tool — what Decredition and other official wallets use behind the scenes — so all users of official wallets are able to enable SPV._
SPV已直接内置于dcrwallet命令行界面中 - Decredition和其他官方钱包则在后台使用 - 因此所有官方钱包的用户都能够启用SPV。


_## Why was SPV added to `dcrwallet`?_
## 为什么要在`dcrwallet`中增加SPV？ 

_The hardware requirements to run a Decred wallet are drastically reduced when operating in SPV mode. Storage and download requirements are reduced because rather than downloading the whole blockchain, an SPV wallet will only download the blocks which contain relevant transactions, and for all of the other blocks in the chain only the headers are downloaded. The amount of processing power required is reduced because a device running an SPV wallet will only validate the proof-of-work and the header chain rather than validating every transaction in every block and ensuring the block contents are consistent with the headers._
在SPV模式下运行时，运行Decred钱包的硬件要求会大幅降低。由于SPV钱包只下载含钱包相关交易的区块而不需要下载整个区块链，存储和下载要求都下降了许多。而其他区块链中的区块，钱包只需要下载其区块头。由于运行SPV钱包的设备仅验证工作量证明和区块头链，而不需验证每个区块中的每项交易并确保区块内容与区块头内容一致，因此降低了设备所需的处理能力。


_As a result of these decreased requirements, Decred wallets can operate on a wider set of devices - particularly mobile devices. Smartphones and tablets are typically limited in at least one of CPU power, storage or download capacity, and mobile operating systems limit the amount of background work processing each application can perform, which makes running a full node either impractical or impossible. _
由于这些需求的降低，Decred钱包可以在更广泛的设备上运行 - 特别是移动设备。智能手机和平板电脑通常受限于至少以下两个的其中一种限制 - 存储/下载容量或移动操作系统限制每个应用程序可以执行的后台工作量。这使得运行全节点非常不可行或几乎不可能。

_Another benefit offered by SPV is an extreme reduction in the time required for a brand new wallet to become operational, offering a huge improvement to the user experience._
SPV的另一个好处是可以大大减少全新钱包从设立到可以使用所需的时间，从而大大的改善了用户体验。

_## How does SPV work?_
## SPV 到底怎么运作？

_At the start of every block added to the Decred blockchain is 180 bytes of data called the [block header](../advanced/block-header-specifications.md). The block header describes key information about the block including the hash of the block, the merkle root (the sum of all the transaction hashes in the block), and the nonce calculated by the proof-of-work miners. A predetermined filter is also created for every block, based on the all of the transactions within the block. _
在Decred区块链中，每个区块的开头是称为[区块头](https://docs.decred.org/advanced/block-header-specifications/)的180字节的数据。区块头描述关于该区块的关键信息，包括区块的哈希值，二进制哈希树根“Merkle root”(即区块中所有交易哈希的总和)，以及由工作量证明矿工计算出来的随机数“nonce”。 另外也基于区块内所有交易为每个区块创建一个预设的过滤器。

_When an SPV wallet initialises it will connect to the Decred network using peer-to-peer connections, and it will download the full set of headers and filters. It will then validate the header chain to ensure that the chain and its proof-of-work are valid. Once this is complete, the wallet will use the filters to locally identify which blocks contain owned transactions without uploading any private data to remote nodes. The wallet can then use the peer-to-peer network to download these blocks, scan them for relevant transactions and select these to update personal transaction history and balance. _
当SPV钱包初始化时，它将使用P2P（peer-to-peer）连接到Decred网络并下载完整的区块头和过滤器。 然后它将验证区块头链以确保区块链及其工作量证明是合格有效的。完成后，钱包将使用过滤器在本地识别哪些区块包含所拥有的交易数据，而无需将任何私有数据上载到远程节点。然后钱包可以使用P2P网络下载这些区块，并扫描以查找相关交易然后选择这些以更新个人交易历史和余额信息。

_## How is this different from a "light" wallet?_
## 这和轻钱包有什么区别？

"Light" wallets such as [Exodus](https://www.exodus.io/) or [Atomic](https://atomicwallet.io/) already bring some of the benefits of an SPV wallet - for example these wallets allow sending and receiving Decred transactions with a minimal start-up time and without downloading the full chain. Light wallets certainly provide a level of convenience but there are often subtle consequences because they depend upon an API provided by a centralised service:

- Light wallets depend on a remote service to watch owned addresses and provide notifications when transactions are received by the addresses. Notifications could be missed or incorrect.

- Users will often be required to upload an extended public key to the central server so it is aware of all of owned addresses, potentially compromising the users privacy.

- Light wallets do not validate the information they receive by checking the blockchain directly - they have to blindly trust the information provided by the central server.

These concerns do not apply to SPV wallets because they connect directly to the decentralised, peer-to-peer Decred network. They upload no private data to remote nodes and they discover their own transactions by inspecting the blockchain directly.


_## Are there any disadvantages?_
## 它的缺点是什么？

- SPV does not support voting wallets. Voting wallets have the responsibility to vote on the validity of the last block, and a wallet cannot be sure of the validity unless it fully validates the whole blockchain leading up to that block. It is possible however to purchase tickets and allocate the voting rights to a [Voting Service Provider](../proof-of-stake/how-to-stake.md#pos-using-a-voting-service-provider-vsp).

- SPV wallets only download blocks which have transactions related to their owned addresses, which could potentially reveal more information about the wallet than if it downloaded every single block. This only presents a very minor decrease in privacy, but it is a decrease nonetheless. This can be mitigated by downloading blocks from multiple peers so no single peer is able to see the full list of blocks downloaded by a wallet. Even if a passive observer on the network is able to see which blocks are downloaded by a wallet, they are not able to identify which transactions in those blocks are relevant.

- Wallets operating in SPV mode are only able to validate the block headers they download and not the filters. This makes a "false-negative" attack possible, whereby a malicious peer that knows a wallet is waiting for a particular transaction could send the wallet a fake filter which does not include the transaction, resulting in the wallet not downloading the block and so not becoming aware of the transactions existence. This transaction would still be visible to all fully validating nodes and wallets, and it will still appear in the [block explorer](../getting-started/using-the-block-explorer.md). One way to prevent this vulnerability is to add the hash of the filter into the part of the block header that is PoW validated, enabling SPV wallets to easily check the validity of the filters without having to download their blocks. A proposal to make this change has [already been suggested](https://github.com/decred/dcrd/issues/971), however a hardfork will be required to make the required change to the block header format.  A "false-positive" scenario is not possible. If a malicious node provides a fake filter which includes a non-existent transaction, the wallet will simply download the full block, compare it to the filter and discover that the filter is not genuine.


_## How do I use SPV?_
## 我可以怎么使用SPV ？

_#### `dcrwallet` CLI_
#### `dcrwallet` 命令行界面

To enable SPV mode in `dcrwallet` simply provide the `--spv` flag when starting the process. There is also an optional `--spvconnect` flag which disables DNS seeding and allows you to specify the IP of a full node you wish to sync from. `--spvconnect` can be specified multiple times to sync from multiple peers.

_#### Decrediton_
#### Decrediton

If Decrediton is being started for the first time, the file `config.json` needs to be modified in order to enable SPV. Locate the file in the [Decrediton data directory](decrediton/decrediton-troubleshooting.md#location-of-data-and-log-files), and update the setting `spv_mode` to `true`.

If Decrediton is already synced with the network it is possible to change to SPV mode through the GUI. Open a wallet, and locate the "SPV" option in the setting tab. There is also a "SPV Connect" setting on this tab, which will allow you to specify the IP address of a full node you wish to sync from.

---

_# SPV: DCR vs BTC_
# DCR与BTC的SPV实现对比总结

_### Decred implementation_
### Decred 实现


* Client-side compact filters proposed by lightning labs developers and contributed to by several bitcoin core devs. Also updates to bips 157 158 - changes we are hoping to pull in later.
* The wallet client downloads the full set of filters, and locally matches which blocks contain owned transactions without uploading any private data. 
* Removes need to trust the remote node
* Not revealing your filter makes it harder for observers to identify your addresses/tx/coins


_### Bitcoin implementation_
### Bitcoin 实现

* Server-side matching with bloom filters. The filter contains hashes for all of your addresses and scripts. Uploaded to a bitcoin node which has a full copy of the blockchain, it selects all transactions which match the filter and sends them back. 
* Makes it possible for a passive observer to infer which addresses you are watching, and therefore which transactions and which coins are yours.
* Requests for filters have to be updated and handled differently for different clients, which can be expensive for the node serving the requests. Opens a DoS attack window

_#### Bloom filters vs GCS filters_
#### Bloom 过滤器 和 GCS 过滤器 的对比

* bloom filters use more network traffic.

# 关于作者

简单支付验证：原文参照[Decred Documentation](https://docs.decred.org/)中[SPV](https://docs.decred.org/wallets/spv/#what-is-spv)页面。
DCR与BTC的SPV对比：原文参照[SPV: DCR vs BTC](https://github.com/decred/dcrdocs/issues/727)

翻译 ：@Guang </br>
[Medium](https://medium.com/@guang.dcr)</br>
Telegram: @GuangGuang</br>
Matrix: @guang:decred.org

欢迎反馈至[Github](https://github.com/Guang168)或联系作者

