## Circulating supply

Financial forecast numbers are taken [here](https://github.com/near/near-explorer/blob/master/frontend/src/pages/api/circulating-supply.js#L4) 

<img width="616" alt="Screenshot 2021-05-17 at 18 16 13" src="https://user-images.githubusercontent.com/11246099/118513080-05504700-b73c-11eb-9d35-cf9353b181a5.png">

### How circulating supply was calculated?

Circulating supply is computed monthly (each 14th of the month), we are taking total supply subtracted locked tokens.

Locked tokens consists of:
- `locked_amount` of each lockup contract (the code is taken from [the lockup contract implementation](https://github.com/near/core-contracts/blob/master/lockup/src/getters.rs#L64)).
  That logic takes into account ended lockups (locked amount would be zero), terminated lockups (locked amount is calculated specificially for that) and non-started lockups (the whole sum on the account would be locked);
- `lockup.near` amount
- `contributors.near` amount


date|locked tokens from lockup accounts|lockup.near|contributors.near|total locked|total circulating|forecast|total supply|
-|-|-|-|-|-|-|-
10/14/2020|`562139746`|`802936`|`201999999`|`764942681`|`235057243`|`157503033`|`999999924`
11/14/2020|`635687215`|`366195`|`163874999`|`799928409`|`204127204`|`189443528`|`1004055613`
12/14/2020|`637992868`|`245533`|`142562499`|`780800900`|`227306254`|`221413208`|`1008107154`
01/14/2021|`634814911`|`222027`|`122895838`|`757932776`|`254035392`|`253434736`|`1011968168`
02/14/2021|`616874905`|`20090`|`120874510`|`737769505`|`277981655`|`285584565`|`1015751160`
03/14/2021|`622097485`|`20303`|`97462172`|`719579960`|`299634810`|`317530390`|`1019214770`
04/14/2021|`604336746`|`2361`|`93796508`|`698135615`|`325405443`|`349452304`|`1023541058`
