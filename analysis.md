                                                        Analysis

1) Load:
        Reads user-wallet-transactions.json once—accepts either a JSON array or newline-delimited JSON—and returns a list of raw dicts.
2) Normalize:
        Builds a pandas DataFrame, then iterates row-by-row.  For every row it flattens any nested actionData dict (so "actionData.amount" becomes a top-level key) and auto-detects which column holds the wallet address, action type, numeric amount, and asset symbol.
3) Aggregate:
        Maintains a running counter per wallet:
           * Total deposited, total borrowed, total repaid, liquidation count, first & last timestamp, unique assets touched.
4) Feature vector:
        After the pass, each wallet is reduced to six floats:
           * Ratio, repay_ratio, liq_penalty, avg_dep, diversity, age_days.
5) Score: 
        Those six numbers are dropped into a single arithmetic block:
           400 · ratio + 300 · repay_ratio + 100 · (diversity/5) + 100 · (age_days/365) + 100 · (ln(avg_dep)/10) – liq_penalty, clamped 0-1000.
6) Emit:
        Writes the {"0x…": score} map to scores.json and prints a one-line summary