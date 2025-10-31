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
     bun run interpolate raw-fxn.csv -f 1758876528 -t 1759827683 -o --frequency 3600 -p 3
     ```
   - Output: `interpolated-fxn.csv`

## User earned fxSP

We use the interpolated data as input for user fxn distribution:

```bash
bun run user-points 1:0x0d1ea8c0ed10a19b2c714cd7ea923ae4a636ee90 --points interpolated-fxn.csv
```

Final output: `user-fxn-distribution.csv`

The final file contains for each user how many fxn they have earned.