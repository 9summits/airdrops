# FxSP Calculation Process

190 fxn have to be distributed linearly among vault depositor. We interpolate the amount distributed through by doing and interpolation then we proportionnaly distribute those fxn among shares holders.


## Linear Interpolation Process

[First deposit](https://etherscan.io/tx/0x73503d94cc6b261389169023ff8279ad26faeb52abeee1e5e765be073db05a9f) happened on **Sep-26-2025 08:48:47 AM UTC**. 

This deposit will be our start date and end date will be the expiry of the pendle market.

- Start: `Sep-26-2025 08:48:47 AM UTC` (timestamp: `1758876527`)
- End point: `1761782400,190`

1. **First Interpolation**:

   - We add a starting point: `1758876528,0` (+1 to avoid issue with cli)
   - Run command:
     ```bash
     bun run interpolate raw-fxn.csv -f 1758876528 -t 1761782400 -o --frequency 3600 -p 3
     ```
   - Output: `interpolated-fxn.csv`

## User earned fxSP

We use the interpolated data as input for user fxn distribution:

```bash
bun run user-points 1:0x0d1ea8c0ed10a19b2c714cd7ea923ae4a636ee90 --points interpolated-fxn.csv
```

Final output: `user-fxn-distribution.csv`

The final file contains for each user how many fxn they have earned.

## Edit of 14 Nov 2025
We made a mistake in the selection of the start date of distribution. A fairer start date of distribution would be the timestamp of the first communication about the vault on x.com from fx procotol.
This date is 1 Oct 2025 at 10:46:00 AM UTC. The timestamp is `1759315607`.
[X post](https://x.com/protocol_fx/status/1973384091434360975?s=20)

This leads to the a `raw-fxn-fix.csv` file.

We interpolate the data.
```bash
     bun run interpolate raw-fxn-fix.csv -f 1759315607 -t 1761782400 -o --frequency 3600 -p 3
```

Based on this data we compute the fair distribution of fxn among users.
```bash
bun run user-points 1:0x0d1ea8c0ed10a19b2c714cd7ea923ae4a636ee90 --points interpolated-fxn-fix.csv
```

Final output: `user-fxn-distribution-fix.csv`

The final file contains for each user how many fxn they have earned in the `actual` column and the amount of they should have earned in the `expected` column and the difference in the `diff` column.

The users who got a negative difference have been credited with the difference amount.
