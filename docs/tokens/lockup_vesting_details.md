## How the vesting/unlocking works?

Read more about it [here](https://github.com/telezhnaya/core-contracts/tree/master/lockup#lockup--vesting-contract). Let's look at the examples.

### Vesting
<img width="583" alt="Screenshot 2021-05-05 at 22 27 47" src="https://user-images.githubusercontent.com/11246099/117198203-664d5600-adf1-11eb-9b1f-30dbd35a55cf.png">

`start`, `cliff`, `end` are the variables from [`vesting_schedule`](https://github.com/telezhnaya/core-contracts/blob/master/lockup/src/lib.rs#L188) from initializing lockup function (they are `start_date`, `cliff_date`, `end_date` there).

Here and in all graphs: X is the time, Y is the amount of unlocked tokens.

### Lockup
<img width="704" alt="Screenshot 2021-05-05 at 22 34 36" src="https://user-images.githubusercontent.com/11246099/117198787-1d49d180-adf2-11eb-86f3-047152e54877.png">

`lockup_timestamp`, `lockup_duration`, `release_duration` are also the [initializing parameters](https://github.com/telezhnaya/core-contracts/blob/master/lockup/src/lib.rs#L183) (names are exactly same there).

`lockup_timestamp` is the start of the lockup.

`lockup_duration` is the interval between Phase 2 launch (October 14th) and the start of the lockup.

Is it true that we could have only one variable for that? **Yes.**

`release_duration` is the length of the lockup.

As you see, there is no cliff.

### Combinations

To make my life easier, I use `v_start`, `v_cliff`, `v_end` for vesting parameters; `l_start`, `l_end` for lockup parameters. It could be trivially transformed into initializing parameters described above.

<img width="1063" alt="Screenshot 2021-05-05 at 22 41 26" src="https://user-images.githubusercontent.com/11246099/117199603-0ce62680-adf3-11eb-9bfc-d24bec8907d3.png">

Main conclusion: the resulting graph of unlocking the tokens is always the minimum between lockup and vesting.

### Termination

Please ignore thin lines made by pencil. I finished school long time ago, and I have no kids, so it's not allowed to have the rubber in my house.

<img width="1180" alt="Screenshot 2021-05-05 at 22 47 41" src="https://user-images.githubusercontent.com/11246099/117200230-ec6a9c00-adf3-11eb-93da-6841990db910.png">

Vesting could be terminated. Vesting could also start before Phase 2 launch.

At the moment of termination, we fix the amount of tokens that should be finally unlocked at the end. We continue to unlock the tokens as we normally do that by getting the minimum between lockup and vesting graph. We stop when we reach the desired amount of tokens.
