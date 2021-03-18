Examples from our [RPC](https://docs.near.org/docs/develop/front-end/rpc) are used. If you want to look at information about archival nodes (more than 2 days ago), please use `archival` prefix in your URL (e.g. `https://archival-rpc.mainnet.near.org`)
In this article, we always use `POST` methods to the same URL `https://archival-rpc.mainnet.near.org`.
Body:
```
{
  "jsonrpc": "2.0",
  "id": "dontcare",
  "method": "EXPERIMENTAL_tx_status",
  "params": ["8uxq5BAC4zZL4Set7gtraqZSWduYXRUiNggpKhWmBnzK", "nf-finance.near"]
}
```

Every [account](https://docs.near.org/docs/develop/front-end/rpc#view-account) has liquid balance (called `amount`) and stake (called `locked`).
All calculation in tables are in yoctonear. 

## Why does my balance change?

There are different scenarios why the amount of tokens could change on the account. Let's look at it on real examples.

### Transfer of tokens

https://explorer.mainnet.near.org/transactions/8uxq5BAC4zZL4Set7gtraqZSWduYXRUiNggpKhWmBnzK

`nf-finance.near` sent 100.0 tokens to `faucet.paras.near`

| block height | nf-finance.near amount | faucet.paras.near amount | explanation |
| - | - | - | - |
| 32435148 | 17985294763885898017800000000 | 95956091138973410800000000 | initial state |
| 32435149 | 7985294718579837830300000000 | 95956091138973410800000000 | nf-finance.near lost some tokens for transferring and commission (part of commission will be returned) |
| 32435150 | 7985294718579837830300000000 | 10095956091138973410800000000 | faucet.paras.near received exactly 100 tokens |
| 32435151 | 7985294719249385517800000000 | 10095956091138973410800000000 | nf-finance.near received a change |
| 32435152 | 7985294719249385517800000000 | 10095956091138973410800000000 | final state |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

We also have receipt for returning a change: it is made without comission.

For this example, final cost of transferring tokens was `22318256250000000000 * 2 / 10^24 = 0.0000446365125` Near Tokens.

### Creating the account

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
For this example, final cost of creating account and transferring tokens were `42455506250000000000 * 2 / 10^24 = 0.0000849110125` Near Tokens.

### Deleting the account

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
For this example, final cost of deleting account was `25554850000000000000 * 2 / 10^24 = 0.0000511097` Near Tokens.

### Calling a function

https://explorer.mainnet.near.org/transactions/DuGWWTK2sAxjffifJiJBv6hs3Hc8MdhpHnzgRJryV4Be

`relayer.bridge.near` invoked a function in the contract of `client.bridge.near`

| block height | relayer.bridge.near amount | client.bridge.near amount | explanation |
| - | - | - | - |
| 32451005 | 927695372457390994900000000 | 1018709186646522709500000000 | initial state |
| 32451006 | 927676238135400182753029324 | 1018709186646522709500000000 | the function is invoked, commission is gone |
| 32451007 | 927676238135400182753029324 | 1018710176966066945800000000 | author of the function gets the reward |
| 32451008 | 927691569761639596000000000 | 1018710176966066945800000000 | relayer.bridge.near received a change for commissions |
| 32451009 | 927691569761639596000000000 | 1018710176966066945800000000 | final state |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 
Receiver gets part of the commission as a reward for a possibility to invoke their function.

For this example, final cost of invoking the function was `(250815301972200000000 + 3551880449426700000000) / 10^24 = 0.0038026957513989` Near Tokens.
Reward was `0.0009903195442363` Near Tokens.

#### The example of calling a function could be more complex.

https://explorer.mainnet.near.org/transactions/DuGWWTK2sAxjffifJiJBv6hs3Hc8MdhpHnzgRJryV4Be

`lulukuang.near` invokes a function in a protocol of `berryclub.ek.near`. It triggers `berryclub.ek.near` to send the money to third account, `farm.berryclub.ek.near`. So we need to analyze 3 accounts in this scenario.

| block height | lulukuang.near amount | berryclub.ek.near amount | farm.berryclub.ek.near amount |
| - | - | - | - |
| 32448898 | 13565822561282329011544599 | 468266310630308773724199697 | 1795104370794561556009708395 |
| 32448899 | 13553154814244264373824919 | 468266310630308773724199697 | 1795104370794561556009708395 |
| 32448900 | 13553154814244264373824919 | 468011594500695874498002337 | 1795104370794561556009708395 |
| 32448901 | 13563348945268324922643655 | 468011594500695874498002337 | 1795359347718068910135905755 |

### Adding the key

https://explorer.mainnet.near.org/transactions/A9767GbmRCLdeCpZYfKTQUqe17KyAu5JrZHanoRChMSV

`slavon.near` added the key to their own account.

| block height | slavon.near amount |
| - | - |
| 32452281 | 4110095852838015413724607 |
| 32452282 | 4110053774694109613724607 |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

Note: we do not have the change.
For this example, final cost of adding the key was `21039071952900000000 * 2 / 10^24 = 0.0000420781439058` Near Tokens.

### Deleting the key

https://explorer.mainnet.near.org/transactions/CfuXcVPy7vZNVabYKzemSwDdNF9fnhbMLaaDKjAMyw8j

`77yen.near` deleted the key from their own account.

| block height | 77yen.near amount |
| - | - |
| 32429428 | 350892032360972200000000 |
| 32429429 | 350851431135972200000000 |

For such kind of transactions, sender pays 2 commissions: for making receipt from transaction; for executing receipt. 

Note: we do not have the change.
For this example, final cost of adding the key was `20300612500000000000 * 2 / 10^24 = 0.000040601225` Near Tokens.

