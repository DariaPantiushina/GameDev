# A/B-test difficulty curve impact in mobile Match-3 game

In a Match-3 game, each level has its own difficulty. The difficulty curve describes how the level difficulty changes throughout the sequence of levels. The game development team decided to test some changes to this curve and evaluate the following **hypotheses**:

● Lowering the difficulty of early levels will reduce player churn but may decrease monetization at these levels, as players will have less need to make purchases.

● Gradually increasing the difficulty of later levels will help compensate for the monetization drop.

The team ran an **A/B test on new players** (the date a player entered the test matches the date they installed the game). 

The **duration of the test** was **60 days**.

# Retention Analysis

1. **Methodology**

I measured day-level retention based on user cohorts defined by install date and experimental group (control vs test). For each day d (D1, D3, D7), I computed the share of users who were active on day d among all users who installed on day 0.

To assess statistical significance of retention differences, I used a **two-proportion Z-test**:

Each user either returned on day d (1) or did not (0) → binary outcome.
Sample sizes in both groups are large → normal approximation for proportions is valid.

The test directly compares probabilities, which matches the definition of retention.

2. **Key results**

1) The test group shows higher D7 retention compared to control (≈ +Δ pp).

For day D7, the Z-test yields a p-value below 0.05.

Early-day retention (D1/D3) is at least not worse in the test group and remains within an acceptable corridor.

3. **Product interpretation**

The new difficulty curve with easier early levels does not hurt onboarding and actually improves mid-term retention (D7). Players are more likely to stay in the game long enough to reach the core progression and monetization layers.

# Monetization & LTV analysis

1. **Methodology**

For the monetization analysis I computed:

- **ARPDAU** = average revenue per daily active user;
- **ARPPU** = average revenue per paying user;
- **Pay rate** = share of users who made at least one purchase;
- **Cumulative ARPU (LTV proxy)** = cumulative revenue per user over the first 30 days since install

To compare **LTV per user** between groups, I used **bootstrap confidence intervals** on player-level revenue:

- Revenue in free-to-play games is **highly skewed** (most players pay 0, a few "whales" pay a lot). This violates the normality assumptions of parametric tests (t-test/Z-test for means);

- Bootstrap does not assume a specific distribution form and is robust to heavy tails

- I resample users with replacement within each group and estimate the distribution of the mean difference (*test - control*).

2. **Key results**

- ARPDAU and ARPPU curves show that the **test group monetizes better** on later days, even if early monetization is slightly lower;

- The cumulative ARPU curve for the test group lies consistently above the control curve over the first 30 days;

- The 95% bootstrap confidence interval for the difference in player-level revenue (*test – control*) is fully above zero, indicating **a statistically significant uplift in LTV** for the test group.

3. **Product interpretation**

The new difficulty curve 1) reduces early friction (onboarding is smoother), 2) shifts part of the monetization to later stages, where engaged players are more willing to pay, 3) increases LTV per user without sacrificing scale (no drop in overall payer conversion).

This supports rolling out the new difficulty curve from **a revenue and retention perspective**.

# UX & difficulty dignals (progression, boosters, rage quits) analysis

1. **Methodology**

To understand *how exactly* the new difficulty curve affects player experience, I considered the following UX features:

- **max_level_reached** - depth of progression;
- **boosters_used** - count of help/booster consumptions;
- **max_fail_streak** - longest streak of failed attempts (proxy for perceived challenge);
- **rage_quit** - a binary flag indicating frustration-driven churn

For these metrics I used:

- **Mann–Whitney U test** for continuous/count metrics (progression depth, boosters, fail streak): 

- 1) distributions are non-normal and discrete; 
- 2) Mann–Whitney does not assume normality and compares distributions rather than means;
- 3) it's important to note that results were interpreted as a stochastic shift ("values in test tend to be higher")

- **Proportion Z-test** for rage_quit (binary): each user either triggered a rage-quit signal (1) or not (0)

2. **Key results**

- The distribution of **max_level_reached** in the test group is significantly shifted to the right (Mann-Whitney U, p < 0.001), meaning that test users are more likely to reach deeper levels;

- The distribution of **boosters_used** is also significantly higher in the test group, indicating increased engagement with monetization-related mechanics;

- At the same time, the **rage-quit rate** is lower in the test group (proportion Z-test, p < 0.05), suggesting that added difficulty later in the game **does not increase frustration**.

3. **Product interpretation**

The test difficulty curve:

1) encourages players to progress further into the game;
2) increases engagement with **boosters** and other monetization hooks;
3) **does not create a spike in frustration-driven churn** (rage quits are actually lower)

In other words, LTV improvement comes from **healthy challenge and deeper progression**, not from making the game unfair or punishing
