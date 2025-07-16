# Points Calculation Process

Vault started earning points on **Apr-14-2025 04:04:11 PM UTC** when it deposited in 9s TAC ETH vault.  
[View transaction on Etherscan](https://etherscan.io/tx/0x4e87b7d8b11960d07f46df471b1a1cb3e3b8e5119dcc3f3aeec1cb09839c7696)

## Linear Interpolation Process

Because of missing data between **Apr-14-2025** and **May-23-2025** we interpolate hourly between those 2 points:

- Start: `Apr-14-2025 04:04:11 PM UTC` (timestamp: `1744603451`)
- First known point: `1748015999,2942569.56`

1. **First Interpolation**:

   - Add a starting point: `1747820207,0,tac-period1-9s-fs-eth`
   - Run command:
     ```bash
     bun run interpolate raw-points.csv -f 1744603451 -t 1748015999 -o --frequency 3600
     ```
   - Output: `interpolated-points-1.csv`

2. **Second Interpolation** (for better accuracy):
   - Interpolate between:
     - First known point: `1748015999`
     - Most recent point: `1749139187`
   - Run command:
     ```bash
     bun run interpolate interpolated-points-1.csv -f 1748015999 -t 1749139187 -o --frequency 3600
     ```
   - Output: `interpolated-points-2.csv`  
     _(Note: This works because the evolution between these points is linear)_

## User Points Calculation

Use the interpolated data as input for user points calculation:

```bash
bun run user-points 1:0x07ed467acD4ffd13023046968b0859781cb90D9B --points interpolated-points-2.csv
```

Final output: `user-distribution.csv`
