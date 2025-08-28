# 📊 Adaptive Anchored VWAP Breakout with Swing-Pattern Slope Confirmation

## 📌 Strategy Description

This trading strategy combines **Anchored Volume-Weighted Average Price (VWAP)** with **swing-pattern slope confirmation** to identify high-probability uptrend breakouts. By blending institutional anchor levels with macro swing structure, it filters noise and ensures only structurally strong breakouts are traded.

### 🔹 Steps Followed

1. **Trend Detection**

   * Sliding window of closing prices classified into:

     * `1` → Uptrend
     * `0` → Downtrend
     * `-1` → Neutral/Noise

2. **Macro Breakpoint Identification**

   * `get_macro_uptrend_lows()` extracts swing lows.
   * `get_macro_uptrend_highs()` extracts swing highs.
   * Valid uptrend requires: **L1 < H1 < L2 < H2 < L3** (higher highs + higher lows).

3. **Anchored VWAP Calculation**

   * Anchor forms when price dips below the **highest low band**.
   * Anchored VWAP:

     ```
     VWAP = (Σ Price × Volume) / (Σ Volume)
     ```
   * Breakout confirmed if price closes **> 1.05 × Anchored VWAP**.

4. **Buy Signal Logic**

   * **Above Anchor Zone:**

     * Price > Anchored VWAP breakout
     * Pattern: L1 < H1 < L2 < H2 < L3
     * Rising slope check: slope2 ≥ 0.8 × slope1
   * **Within Band Zone:**

     * Same structural checks applied inside VWAP band.
   * **Fallback Case:**

     * No band yet but price > avg buy price → deploy **3% capital probe**.

5. **Capital Allocation**

   * Price within **3% below avg buy** → allocate **30% capital**.
   * Otherwise → allocate **20% capital**.

6. **Stop Loss Placement**

   * Stop = max(8% below entry, last swing low).

7. **Sell Signal Logic**

   * **Slope Weakening:**

     * Exit if slope of last two highs weakens (slope2 < 0.7 × slope1).
   * **Profit Protection:**

     * Exit if price > 1.08 × buy price.
   * **Macro Low Failure:**

     * If low count drops sharply (−2 vs last peak), exit (trend breakdown).

---

## 📈 Trading Interpretations

* **Anchored VWAP** → Captures institutional benchmarks for breakout confirmation.
* **Swing Structure** → Confirms higher highs + higher lows before entry.
* **Slope Analysis** → Ensures trend acceleration, not stagnation.
* **Capital Scaling** → Dynamic risk sizing based on cost basis proximity.
* **Stop Loss Anchoring** → Uses natural swing levels for robust protection.
* **Exit Logic** → Protects profit and exits on early structural weakness.

---

## 🛠️ Libraries Used

* **`AlgoAPI`** → Market data, event handling, order placement.
* **`datetime`** → Timestamp management for slope calculations.
* **`timedelta`** → Time-based slope computations.




