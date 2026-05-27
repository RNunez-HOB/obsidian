  

🚢

# Human vs. Claude  
A Titanic ML Showdown

Using AI to compete in a Kaggle machine learning competition

Kaggle Machine Learning Claude AI Feature Engineering Random Forest

Data Science & AI Traineeship · April 2026

## What is Kaggle?

Kaggle is the world's largest data science community — a platform where companies and researchers post real datasets and challenges, and the community competes to build the best model.

- Hosted by Google since 2017
- 17 million+ registered users
- $30M+ in total prize money awarded
- Free compute (GPUs/TPUs) in notebooks
- Public notebooks to learn from others

17M+

Users worldwide

$30M+

Prizes awarded

100K+

Public datasets

Free

GPU compute

💡 Think of Kaggle as a **gym for data scientists** — real problems, real data, instant feedback on your solution.

## How a Kaggle Competition Works

Step 1

📥 Download  
the Data

→

Step 2

🔍 Explore &  
Analyse

→

Step 3

⚙️ Engineer  
Features

→

Step 4

🤖 Train a  
Model

→

Step 5

📤 Submit  
Predictions

→

Step 6

📊 See Your  
Score

### Training data

You receive labelled examples to learn from. The model finds patterns between passenger attributes and survival.

### Test data (hidden labels)

You predict on unseen passengers. Kaggle compares your predictions to the true answers and gives you a score.

Score = fraction of test passengers your model correctly classifies. The leaderboard updates in real time.

## The Titanic Competition

On 15 April 1912, the RMS Titanic sank after striking an iceberg. Of 2,224 passengers and crew, roughly 1,500 died — making it one of the deadliest peacetime maritime disasters in history.

Kaggle turns this tragedy into a **binary classification problem**: given passenger attributes, predict who survived (1) and who did not (0).

891

Training rows

418

Rows to predict

11

Input features

|Feature|Description|
|---|---|
|`Pclass`|Ticket class (1 = 1st, 2 = 2nd, 3 = 3rd)|
|`Sex`|male / female|
|`Age`|Age in years (20 % missing)|
|`SibSp`|Siblings / spouses aboard|
|`Parch`|Parents / children aboard|
|`Fare`|Passenger fare paid|
|`Embarked`|Port of embarkation (C/Q/S)|
|`Cabin`|Cabin number (77 % missing)|

## Key Patterns in the Data

"Women and children first" — a real signal hiding in the data

### By Sex

Female

74%

Male

19%

Women were 4× more likely to survive

### By Class

1st

63%

2nd

47%

3rd

24%

Class correlates strongly with survival

### By Age Group

Children

58%

Adults

38%

Seniors

25%

Children prioritised in lifeboat boarding

These patterns are the signal your model must learn. Feature engineering is about making this signal **easier for the algorithm to detect**.

## Our Experiment: 4 Competitors, Same Task

Can an AI model handle the whole ML pipeline — or does human insight still win?

⚡

### Haiku

Fastest & cheapest Claude model. Simple prompt, writes feature code, trains RandomForest.

🧠

### Sonnet

Mid-tier model with adaptive thinking enabled. Adds reasoning before writing code.

👑

### Opus

Most capable Claude model. Richer prompt, more context, Grandmaster persona.

👤

### Human

Manual EDA, Polars dataframes, iterative feature engineering over 3 versions.

All competitors used a **RandomForestClassifier** as the ML algorithm and submitted predictions to Kaggle's leaderboard. The difference was entirely in **how features were prepared**.

## ⚡ Claude Haiku — Fast & Affordable

- Smallest, fastest, cheapest Claude model
- Prompt: "You are an expert Kaggle data scientist"
- Given: column names, 5 sample rows, missing value counts
- Asked to: fill nulls, encode categoricals, create basic features
- No extended thinking — single-pass response

Features generated: Age (median fill), Fare, Sex encoded, Embarked encoded, Title from Name, FamilySize, IsAlone

Score: 0.72727

# Prompt sent to Haiku (simplified) prompt = """ You are an expert Kaggle data scientist working on Titanic survival prediction. Train columns: [PassengerId, Survived, Pclass, Name, Sex, Age, ...] Missing values: Age: 177, Cabin: 687 Requirements: 1. Fill all missing values 2. Encode all categorical variables 3. Create useful new features (title from Name, family size, etc.) 4. Assign X_train, y_train, X_test Return ONLY Python code. """ # No thinking, direct code generation response = client.messages.create( model="claude-haiku-4-5", max_tokens=4096, messages=[...] )

## 🧠 Claude Sonnet — Thinking Before Acting

- Mid-tier model — smarter, somewhat slower
- Prompt adds a hint: "is_alone" feature suggestion
- Key addition: `thinking: {"type": "adaptive"}`
- Model reasons through the problem before outputting code
- Same Random Forest classifier, higher-quality features

Adaptive thinking lets Sonnet _consider_ which features matter before writing code — similar to how a human would sketch on paper first.

Score: 0.76315+3.6 pts vs Haiku

# Sonnet: same prompt structure, # plus adaptive thinking response = client.messages.create( model="claude-sonnet-4-6", max_tokens=4096, thinking={"type": "adaptive"}, messages=[...] ) # Thinking blocks come first; # skip them to get the code text = next( b.text for b in response.content if b.type == "text" )

## 👑 Claude Opus — Maximum Capability

- Persona: _"You are an elite Kaggle Grandmaster"_
- Prompt includes survival rates by sex & class
- Explicit feature ideas listed in the prompt
- Higher token budget (8,096 vs 4,096)
- Adaptive thinking enabled

Extra features attempted: Fare per person, Cabin deck letter, Age × Pclass interaction, per-group median imputation for Age.

Score: 0.77033+4.3 pts vs Haiku

# Opus: richer context in prompt data_info = f""" Survival rate by Sex: {train_df.groupby('Sex')['Survived'] .mean().to_dict()} Survival rate by Pclass: {train_df.groupby('Pclass')['Survived'] .mean().to_dict()} """ prompt = """ You are an elite Kaggle Grandmaster. Create rich features: - Title (Mr/Mrs/Miss/Master/Rare) - FamilySize = SibSp + Parch + 1 - IsAlone flag - Fare per person - Age * Pclass interaction - Cabin deck letter """

## Prompt Engineering Matters

The same model family, but progressively richer prompts → progressively better scores

⚡ Haiku — Basic prompt

"You are an expert Kaggle data scientist. Fill missing values, encode categoricals, create useful features (e.g. title from Name, family size). Return ONLY code."

0.72727

🧠 Sonnet — Guided prompt

"You are an expert Kaggle data scientist. Fill missing values, encode categoricals, create useful features (e.g. title, family size, **is_alone**). Return ONLY code."  
  
+ adaptive thinking enabled

0.76315

👑 Opus — Expert persona

"You are an **elite Kaggle Grandmaster**. Here are survival rates by sex and class. Create rich features: FamilySize, IsAlone, Fare per person, Cabin deck, Age×Pclass."  
  
+ adaptive thinking + 8K tokens

0.77033

🔑 **Key insight:** Giving the model _domain context_ (survival rates, explicit feature ideas, expert persona) dramatically improves output quality — without changing the underlying algorithm.

## Adaptive Thinking — Reasoning Before Coding

### What is extended thinking?

- The model works through the problem in a hidden "scratchpad" before generating its final answer
- **Adaptive** = only activates when the question actually requires it
- Analogous to writing rough notes before a clean solution
- Enables multi-step reasoning: "Which imputation strategy? What interaction terms?"

Feature engineering is _exactly_ the kind of task that benefits from thinking — it requires domain reasoning, not just pattern matching.

### What the model "thinks" (simplified)

"The dataset has 177 missing Age values. A global median would work, but per-title median (Mr → 30, Miss → 22, Master → 5) is more informative. Fare is right-skewed so a log transform or per-person normalisation would help. Cabin is 77% missing — I can still extract the deck letter (C, B, A…) for the 23% that have it and use 0 otherwise..."

# Only Sonnet + Opus use this thinking={"type": "adaptive"} # Thinking blocks are hidden; # only the final text is used text = next(b.text for b in response.content if b.type == "text")

## Cost · Speed · Quality — The Eternal Tradeoff

Choosing the right model is itself an engineering decision

Fastest / CheapestSlowest / Most expensive

Haiku Sonnet Opus

### ⚡ Haiku

- ~10× cheaper than Opus
- Responds in seconds
- Great for prototyping & high-volume tasks
- Score: **0.72727**

Use when speed matters more than precision

### 🧠 Sonnet

- Balanced price/performance
- Adaptive thinking adds depth
- Best everyday workhorse
- Score: **0.76315**

The default choice for most DS tasks

### 👑 Opus

- Most capable reasoning
- Highest cost per token
- Best for complex, high-stakes tasks
- Score: **0.77033**

Reserve for your hardest problems

## 👤 The Human Approach — EDA-First, Iterate

### Three versions, each improving

Version 1 — Baseline 0.75358

RandomForest, Polars dataframes, log(Fare), one-hot encode Sex & Embarked, fill Age with forward-fill. Submit, observe score.

Version 2 — Feature tune 0.78229

Added mean-imputed Age, revisited feature selection based on EDA insights. Feature importance inspected.

Version 3 — Hyperparameter search 0.78947

GridSearchCV over n_estimators, max_depth, min_samples_leaf, max_features with 5-fold CV. Best params applied to final submission.

### EDA drove every decision

- Visualised missing value patterns before imputing
- Plotted survival rate by every categorical feature
- Noticed Fare is right-skewed → applied log transform
- Checked Pclass × Sex interaction heatmap
- Used train/validation split to test changes locally

📊 Used **Polars** (not Pandas) — a modern dataframe library that is 5-100× faster on large datasets.

## What Humans Still Bring to the Table

The human won — but why, exactly?

🔍

### Visual EDA

Plotting distributions and correlations reveals patterns (e.g. Fare skew, Cabin sparsity) that a prompt-only approach misses.

🔄

### Iterative refinement

V1 → V2 → V3: each submission gave feedback that shaped the next version. Claude models submitted once and stopped.

💡

### Domain curiosity

Questions like "does the cabin letter encode the deck level?" or "is a log-transform of Fare justified?" come from human curiosity.

🤖 **AI advantage:** Wrote correct, runnable feature engineering code in seconds. No syntax errors, no library confusion.

👤 **Human advantage:** Knew _which_ features to engineer, _when_ to iterate, and _how_ to read the leaderboard signal.

## The Results — Leaderboard Scores

Kaggle accuracy (fraction of 418 test passengers correctly classified)

Haiku

0.72727

Sonnet

0.76315

Opus v1

0.77033

Opus v2 ✦

0.77511

Human V1

0.75358

Human V2

0.78229

Human V3 🏆

0.78947 ★

Human V3 still leads — but Opus v2 closed the gap from **1.9 to 1.4 percentage points** by fixing overfitting, with **zero manual analysis**.

## Opus v2 — What Changed & Why It Worked

### vs Opus v1 — fixing the overfit

||Opus v1|Opus v2|
|---|---|---|
|Features|24|**12** (halved)|
|RF max_depth|none|**6**|
|min_samples_leaf|none|**5**|
|Train accuracy|99.8%|86.1%|
|5-fold CV|—|**82.3%**|
|Kaggle score|0.77033|0.77511|

**Key prompt change:** Titles (Mr, Miss, Master…) explicitly framed as proxies for _both_ age and fare/class — not just for age imputation. Interaction terms, ticket features, and name length were explicitly banned.

### vs Human V3 — same destination, different path

- **How they found regularisation:** Human used GridSearchCV across depth/leaf params. Opus v2 had them set directly via prompt guidance — same answer, different route.
- **Feature selection:** Human ran EDA plots to see which variables signal. Opus v2 relied on domain instructions baked into the prompt.
- **Iteration:** Human submitted 3 times and adjusted. Opus v2 ran once — a single shot.
- **Remaining gap:** Human V3 still wins by 1.4 pts. EDA and leaderboard feedback gave the human signal that a one-shot prompt cannot replicate.

💡 **The overfitting fix was the same insight** — both human and AI converged on a regularised, lean RF. The difference is that the human _discovered_ it through experimentation; the AI needed it _stated_ in the prompt.

## The Accuracy Ceiling — Why 100% is a Lie

### Irreducible noise

Some outcomes on the Titanic were genuinely random — a wealthy woman who gave up her lifeboat seat, a steward who survived by luck. No model, however sophisticated, can predict these.

Researchers estimate the **theoretical accuracy ceiling** for the Titanic dataset at approximately **83–85%**.

Scores above 83% on this competition are typically achieved by **leaderboard hacking**: submitting 418 predictions, flipping one at a time to reverse-engineer the hidden labels.

### What this means for you

- The noise floor is a real mathematical concept — _Bayes error rate_
- Chasing tiny score improvements past ~80% often means overfitting to noise
- Our best score (0.78947) is close to the practical ceiling for honest models
- In real data science, "good enough and interpretable" beats "perfect and opaque"

💡 The skill isn't maximising accuracy — it's knowing _when to stop_ and _what the model actually learned_.

## AI in Data Science — Where Is This Heading?

### The shift already happening

- Claude Code now writes **~90% of its own codebase** — the loop has closed
- LLMs generate full EDA notebooks, feature engineering pipelines, and model-selection scripts in seconds
- Agentic workflows: AI writes code → runs it → reads the error → fixes it, without human intervention
- Kaggle already has LLM-generated notebooks ranking in the top 10% on many competitions

### What changes for data scientists

- **Problem definition** matters more — garbage in, garbage out applies to prompts too
- **Evaluation literacy** — knowing whether AI output is correct is a core skill
- **Domain knowledge** — what the AI can't Google is what you bring
- **Iteration speed** — human + AI teams move 10× faster than either alone

The data scientist of 2026 is not someone who _writes_ more code — it's someone who _directs_ better, _questions_ sharper, and _understands_ deeper. AI raises the floor; your expertise raises the ceiling.

## Key Takeaways

### Using Claude Code in ML projects

- **Prompt quality = code quality.** Give the model domain context, not just instructions. Survival rates, feature ideas, expert framing all helped.
- **Match model to task complexity.** Haiku for quick scripts, Opus for your hardest feature engineering challenges.
- **Adaptive thinking is free reasoning.** Turn it on for tasks that require multi-step decisions.
- **Let AI write the boilerplate.** Encoding, imputation, one-hot expansion — AI handles these instantly so you can focus on insight.

### On data science in general

- **EDA first, always.** Looking at your data before modelling is still irreplaceable — even in an AI-assisted workflow.
- **Iterate, don't optimise.** Three versions of a human solution beat a single shot from the best AI model.
- **Know your ceiling.** The Titanic dataset caps at ~83%; stop before you overfit to noise.
- **Human + AI > either alone.** The best teams will combine AI speed with human curiosity and domain knowledge.

🚀

## Your Turn

The Titanic competition is free, beginner-friendly, and still open

1️⃣

### Join Kaggle

Create a free account at **kaggle.com** and enrol in the Titanic competition

2️⃣

### Explore the Data

Run your own EDA. What patterns do _you_ find? Can you beat 0.78947?

3️⃣

### Use Claude Code

Write a prompt, let Claude generate your features, refine it. Compare prompt strategies.

In the traineeship you won't just solve known problems — you'll define new ones, choose the right tools, and direct AI to do the heavy lifting while you bring the thinking.  
**That's exactly what this experiment showed is possible.**

kaggle.com/competitions/titanic  ·  anthropic.com/claude-code

[[Project - Titanic Survival Classifier]]