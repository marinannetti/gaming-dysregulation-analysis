# gaming-dysregulation-analysis
# Gaming Dysregulation and Psychological Needs

An independent data analysis of adolescent gaming behavior in the United Kingdom, based on Przybylski & Weinstein (2019).

> Gaming disorder makes headlines every few years, but most of the conversation fixates on screen time, which turns out to be a surprisingly weak predictor of who actually struggles. This analysis digs into a nationally representative dataset of British teenagers to ask a different question: what's going on in their offline lives? The answer points to something more fundamental than hours logged, and more actionable than a device curfew.

---

## The Question

What actually predicts dysregulated gaming in adolescents, and is it what we assume it is?

Everyone has an opinion. Parents worry about screen time, health organizations debate diagnostic criteria, headlines oscillate between alarm and reassurance. This project cuts through the noise and looks at the data directly.

---

## The Short Answer

Hours played is not the best predictor; psychological need frustration is a substantially stronger one.

Adolescents whose daily needs for autonomy, competence, and social connection go unmet show consistently higher rates of gaming dysregulation, and higher rates of real-world behavioral and emotional difficulties. The pattern is consistent with the idea that dysregulated gaming is a symptom of something deeper, though the study design cannot establish causation.

---

## The Data

- **Source:** Przybylski, A. K., & Weinstein, N. (2019). Investigating the Motivational and Psychosocial Dynamics of Dysregulated Gaming: Evidence From a Preregistered Cohort Study. *Clinical Psychological Science, 7*(6), 1257–1265. https://doi.org/10.1177/2167702619859341  
- **Data deposit:** https://osf.io/5fbac/  
- **Sample:** 1,004 British adolescents (ages 14–15) and their caregivers, recruited using a nationally representative methodology based on 2011 UK Census data. Of these, 525 reported playing internet-based games and completed the full measure battery.

**Why this dataset:**
- Preregistered: hypotheses and analysis plan were committed to before data collection began
- Nationally representative: not a convenience sample
- Peer-reviewed and published in a top clinical psychology journal
- Fully open and traceable to its source

---

## Key Variables

| Variable | Description |
|---|---|
| `need_deprivation` | Daily psychological need frustration (autonomy, competence, relatedness) |
| `need_satisfaction` | Daily psychological need satisfaction |
| `game_satisfaction` | Need satisfaction experienced within games specifically |
| `igd_count` | Dysregulated gaming symptom count (0–9) |
| `internet_gaming_time` | Average daily hours spent playing internet-based games |
| `externalising_problems` | Parent-reported hyperactivity and conduct difficulties |
| `internalising_problems` | Parent-reported emotional and peer difficulties |

---

## Key Findings

**1. Most adolescent gamers show no signs of dysregulation.**  
44% of internet gamers reported zero IGD symptoms. Severe dysregulation affected a small minority of players, consistent with a 2021 meta-analysis estimating gaming disorder prevalence at ~3–4% under stringent criteria.

**2. Psychological need frustration is the strongest predictor of dysregulated gaming.**  
Adolescents who felt controlled, ineffective, and isolated in daily life showed consistently higher dysregulation scores, accounting for ~13% of variance in IGD symptoms.

**3. Time spent gaming is a weak predictor.**  
Daily hours played showed only a modest positive association with dysregulation (r ≈ 0.18). Two adolescents playing the same number of hours can show completely different symptom profiles depending on their psychological need experiences.

**4. Need frustration and satisfaction are not symmetric predictors.**  
Need satisfaction showed a weaker negative association with dysregulation than need frustration showed a positive one, consistent with the well-documented negativity bias in human stress-response systems.

**5. Need frustration predicts real-world functioning problems independently of gaming.**  
Parent-reported externalising and internalising problems both increased with need frustration. Dysregulated gaming itself accounted for very little additional variance once need frustration was controlled for.

**6. Gender moderates the relationship between need frustration and dysregulation.**  
Male adolescents showed a stronger association between need frustration and IGD symptoms (slope = 0.70) than female adolescents (slope = 0.48). This is an exploratory finding not examined in the source paper, treated as hypothesis-generating, not conclusive, given the smaller female subsample (n = 164 vs. 359).

---

## What This Analysis Adds

**Gender moderation.** The original paper controls for gender but never examines it as a moderating variable. This analysis does, finding a meaningful difference in slope between male and female adolescents.

**Neuroscience framing.** The source paper is written for clinical researchers. This analysis connects the findings to a broader neuroscientific framework including SDT, negativity bias, and the neurological basis of behavioral self-regulation.

**Interpretive honesty.** All findings are treated as associations within the limits of a cross-sectional design. Causal claims are not made where the data does not support them.

---

## Project Structure

```
gaming-dysregulation-analysis/
├── data/
│   └── dataset_cpx.csv
├── notebooks/
│   └── 01_exploration.ipynb
└── README.md
```

---

## Tools

Python 3.12 · pandas · numpy · matplotlib · seaborn · scikit-learn

---

## Disclaimer

Independent portfolio project. Not peer-reviewed. Does not constitute clinical advice. All interpretations are my own and do not represent the views of the original authors.

---

*Mariana Nannetti · 
