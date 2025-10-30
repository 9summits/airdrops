# FxSP Calculation Process

We started recording fxSP earning on **Oct-7-2025 09:01:23 PM UTC** by querying Pendle positions contracts.


## Linear Interpolation Process

[First deposit](https://etherscan.io/tx/0x73503d94cc6b261389169023ff8279ad26faeb52abeee1e5e765be073db05a9f) happened on **Sep-26-2025 08:48:47 AM UTC**. 

Because of missing data between **Sep-26-2025** and **Oct-7-2025** we interpolate hourly between those 2 points:

- Start: `Sep-26-2025 08:48:47 AM UTC` (timestamp: `1758876527`)
- First known point: `1759827683,5863.39`

1. **First Interpolation**:

   - We add a starting point: `1758876527,0`
   - Run command:
     ```bash
     bun run interpolate raw-fxsp.csv -f 1758876527 -t 1759827683 -o --frequency 3600
     ```
   - Output: `interpolated-fxsp.csv`

## User earned fxSP

We use the interpolated data as input for user points calculation:

```bash
bun run user-points 1:0x0d1ea8c0ed10a19b2c714cd7ea923ae4a636ee90 --points interpolated-fxsp.csv
```

Final output: `user-fxsp-distribution.csv`
