Examples from our [RPC](https://docs.near.org/docs/develop/front-end/rpc) are used. If you want to look at information about archival nodes (more than 2 days ago), please use `archival` prefix in your URL (e.g. `https://archival-rpc.mainnet.near.org`)
In this article, we always use `POST` methods to the same URL `https://archival-rpc.mainnet.near.org`, so all examples contain only body of the request.

Every [account](https://docs.near.org/docs/develop/front-end/rpc#view-account) has liquid balance (called `amount`) and stake (called `locked`).
All calculation in tables are in yoctonear. 

## Why does my balance change?

There are different scenarios why the amount of tokens could change on the account. Let's look at it on real examples.

#### Transfer of tokens

https://explorer.mainnet.near.org/transactions/8uxq5BAC4zZL4Set7gtraqZSWduYXRUiNggpKhWmBnzK

`nf-finance.near` sent 100.0 tokens to `faucet.paras.near`

```
{
  "jsonrpc": "2.0",
  "id": "dontcare",
  "method": "EXPERIMENTAL_tx_status",
  "params": ["8uxq5BAC4zZL4Set7gtraqZSWduYXRUiNggpKhWmBnzK", "nf-finance.near"]
}
```

| block height | nf-finance.near amount | faucet.paras.near amount | explanation |
| - | - | - | - |
| 32435148 | 17985294763885898017800000000 | 95956091138973410800000000 | initial state |
| 32435149 | 7985294718579837830300000000 | 95956091138973410800000000 | nf-finance.near lost some tokens for transferring and commission (part of commission will be returned) |
| 32435150 | 7985294718579837830300000000 | 10095956091138973410800000000 | faucet.paras.near received exactly 100 tokens |
| 32435151 | 7985294719249385517800000000 | 10095956091138973410800000000 | nf-finance.near received a change |
| 32435152 | 7985294719249385517800000000 | 10095956091138973410800000000 | final state |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

We also have receipt for returning a change: it is made without comission.

For this example, final cost of transferring tokens was `22318256250000000000 * 2 // 10^24 = 0.0000446365125` Near Tokens.

#### Creating the account

https://explorer.mainnet.near.org/transactions/Hant2anxPJHm3sh8ncc5AWn5VpXTmwq6iyEumVoQcUJP

`nearcrowd.near` created account `app.nearcrowd.near`, gave the access key, transfer some tokens for that account. These 3 actions are usually go together.

| block height | nearcrowd.near amount | app.nearcrowd.near amount | explanation |
| - | - | - | - |
| 32396638 | 117975002547050142700000000 | - | initial state (app.nearcrowd.near does not exist) |
| 32396639 | 17974916362372455200000000 | - | nearcrowd.near spent tokens on transfer and comissions |
| 32396640 | 17974916362372455200000000 | 100000000000000000000000000 | app.nearcrowd.near was created with initial amount of tokens |
| 32396641 | 17974917636037642700000000 | 100000000000000000000000000 | nearcrowd.near received a change for commissions |
| 32396642 | 17974917636037642700000000 | 100000000000000000000000000 | final state |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

We also have receipt for returning a change: it is made without comission.
For this example, final cost of creating account and transferring tokens were `42455506250000000000 * 2 // 10^24 = 0.0000849110125` Near Tokens.

#### Deleting the account

https://explorer.mainnet.near.org/transactions/8SXzb2R7rtQN73nTCM7HYiQZ8dx5jyfS1MTX46RPrXiu

It's not possible to delete not your account: user is always deleting their own account. User should provide beneficiary account that receive all tokens. User could provide the account that does not exist, in such case the tokens will burn.

`sarasvati.near` deleted their account with beneficiary `sarasvati.paras.near`

| block height | sarasvati.near amount | sarasvati.paras.near amount | explanation |
| - | - | - | - |
| 32276109 | 1497673397950572400000000 | 4982993773072756177000000000 | initial state |
| 32276110 | - | 4982993773072756177000000000 | sarasvati.near was deleted |
| 32276111 | - | 4984491395361006749400000000 | sarasvati.paras.near received tokens (minus commission) |
| 32276111 | - | 4984491395361006749400000000 | final state |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

Note: we do not have the change.
For this example, final cost of deleting account was `25554850000000000000 * 2 // 10^24 = 0.0000511097` Near Tokens.
