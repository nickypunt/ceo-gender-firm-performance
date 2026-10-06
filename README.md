# Does CEO Gender Affect Firm Performance?
### Evidence from S&P 500 firms using propensity score matching

**Question:** Do U.S. S&P 500 companies led by female CEOs earn higher operating margins than comparable companies led by male CEOs?

**Answer:** Firms with female CEOs showed a **1.2 percentage point higher EBITDA margin**, but the difference is **not statistically significant** (p = 0.66). Firm size and industry predict margins far better than CEO gender. Leadership diversity shows **no financial cost**.

*Econometrics research project, Bucknell University (Spring 2026)*

---

## Why it matters
Only about 11% of U.S. companies have a female CEO, and boards, investors, and lawmakers (e.g., California SB 826, Norway's board quota) increasingly use performance evidence to justify diversity policies. This project tests whether that link holds at the very top of the firm.

## Data
- **Source:** S&P Capital IQ, pulled March 2026
- **Sample:** 476 U.S.-headquartered S&P 500 firms, **50 with female CEOs** (~10.5%); non-U.S. firms and missing values removed
- **Outcome:** EBITDA margin (EBITDA ÷ revenue, LTM), a size-neutral measure of operating efficiency

| Variable | Description |
|---|---|
| `ebitda_margin` | EBITDA divided by total revenue (outcome) |
| `female_ceo` | 1 if the current CEO is female (treatment) |
| `log(employees)` | Log of global headcount (firm size) |
| `financial_sector` | 1 if the firm's primary industry is financials |
| `california` | 1 if headquartered in California (subject to SB 826 board-diversity law) |

## Method
Female CEOs aren't randomly assigned, since they're more common in some industries and firm sizes, so a simple comparison of averages would be biased.

1. **Propensity score matching:** estimated each firm's likelihood of having a female CEO with a logit model, then matched each female-led firm to the two most similar male-led firms (2:1 nearest-neighbor matching).
2. **Balance check:** verified that matched groups are comparable using a love plot and propensity score distributions.
3. **Regression on the matched sample:** OLS of EBITDA margin on female CEO plus controls. A second model adds a *Female × Financial Sector* interaction.

<p align="center">
  <img src="images/love_plot.png" width="48%" alt="Covariate balance before and after matching">
  <img src="images/propensity_scores.png" width="48%" alt="Propensity score distributions before and after matching">
</p>

*Left: matching closes the propensity score gap between groups. Right: matched treated and control firms have similar propensity score distributions.*

## Results

| Variable | Model 1 | Model 2 (interaction) |
|---|---|---|
| Female CEO | 0.012 (0.027) | 0.002 (0.031) |
| Log(employees) | −0.050*** (0.011) | −0.052*** (0.012) |
| Financial sector | −0.074* (0.033) | −0.092* (0.040) |
| California | 0.010 (0.039) | 0.008 (0.039) |
| Financial sector × Female | | 0.054 (0.071) |
| Adjusted R² | 0.140 | 0.137 |

*Standard errors in parentheses. \*\*\* p < 0.001, \* p < 0.05*

**Key takeaways**
- The female CEO effect is **positive but not significant**, consistent with prior research (Shao & Liu).
- **Larger workforces** are associated with lower margins: a 10% increase in employees corresponds to about 0.5 pp lower EBITDA margin.
- **Financial-sector firms** have margins 7 to 9 pp lower than other sectors.
- Female CEOs in financial services show a positive (though insignificant) interaction effect, a possible area for further research.

## Limitations
- **Small treated group:** only 50 female CEOs limits statistical power.
- **Glass cliff effect:** women are more often appointed to struggling firms, which may bias the estimate downward.
- **Cross-sectional data:** one point in time, with no controls for CEO tenure or pre-appointment margins.
- Matching only controls for **observed** characteristics.

**Next steps:** panel data across multiple years, alternative performance measures (Tobin's Q, ROA), and controls for CEO tenure.

## Repository structure
```
├── code/
│   ├── 01_data_prep.Rmd        # Variable setup, summary statistics, distributions
│   └── 02_matching_model.Rmd   # Propensity score matching, balance checks, regression
├── images/                     # Charts used in this README
├── paper/
│   └── paper.pdf               # Full research paper
└── README.md
```

## Tools
**R:** `MatchIt` (propensity score matching), `cobalt` (balance diagnostics), `dplyr`, `readxl`

---
**Nicky Punt** · M.S. Business Analytics, Columbia University · [LinkedIn](https://linkedin.com/in/yourname)
