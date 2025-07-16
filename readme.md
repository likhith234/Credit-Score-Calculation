                                        Credit Score Calculation Model

* Compute a transparent 0-1000 credit score for every wallet that has ever interacted with Aave V2.
* Here the code is totally based on function so that it would be more modular , better for debugging and understanding.

                                        Approach For the Solution for the Problem

* We treat every wallet as a tiny balance-sheet.  
* From its lifetime of Aave interactions we extract only six numbers that survive any schema wrinkles: total supplied, total borrowed, total repaid, liquidation strikes, token diversity, and account age.
* These six scalars are fed into a single, five-line arithmetic formula whose weights are hard-coded constants. 
* The formula is the entire model—no training, no black box, no retraining when new chains appear.
*  It is the minimal set of economically meaningful levers that let governance or community voters tweak trust in real time by editing five integers.

                                         Functions in the code

* safe_str(x) – converts any value into a string, default empty string.
* safe_float(x) – safely parse any value (scalar, dict, None) to float, default 0.0.
* load(path) – read user-wallet-transactions.json (list or NDJSON) and return a list of dicts.
* build_features(df) – auto-detect columns, flatten nested actionData, and aggregate per-wallet features.
* score(features) – convert the six computed features into a 0–1000 credit score via the open formula.
* run() – glue everything: load JSON → build features → score wallets → write scores.json.