## POSTMORTEM

On December 2, 2020, we noticed [transaction hash collisions on Testnet](https://github.com/near/nearcore/security/advisories/GHSA-2v6r-g342-282f).

It was possible to apply the same transaction two or more times.
Such transactions are logically different objects, but they have same hash.
It means that, in general case, transaction hash does not fully identify the transaction. 

We observe some limitations because of that:
- It's not possible to get non-first appearance of transaction via [RPC](https://docs.near.org/docs/api/rpc#transaction-status).
- There is no information about non-first appearance of transaction and all related enitites in the [Indexer DB](https://github.com/near/near-indexer-for-explorer).
- As the result, [Near Explorer](https://explorer.near.org) also does not show non-first appearances of transactions, which may look confusing.

Unfortunately, we also have appearances of this issue on Mainnet:
- ...
- ...

**TODO** we should collect and report here about all such cases on Mainnet

The existance of this issue does not mean that the blockchain is corrupted somehow or there are problems with balances.
Since we've fixed the issue, the only problem now is the data representation.

[The fix](https://github.com/near/nearcore/pull/4064) was merged 11 of March 2021 and released in [1.19.0](https://github.com/near/nearcore/releases/tag/1.19.0).

We are confident that such problem will never repeat again.
Sorry for the inconvenience caused.
