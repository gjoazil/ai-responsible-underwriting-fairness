Responsible AI for Predictive Underwriting

Building guardrails for algorithmic risk pricing: accuracy, explainability, and fairness as named pipeline stages, not afterthoughts.

Show Image Show Image Show Image

TL;DR
Best model: XGBoost, with about 95% accuracy and F1 on the test set.
Standard fairness check passed: the model met single-attribute gender fairness criteria under standard thresholds.
Intersectional audit failed: smoker status acted as a total veto, completely excluding that sub-segment.
Recommendation: add explainability (SHAP) and continuous governance guardrails before any real-world deployment.

The main lesson: a model can look fair one attribute at a time and still be seriously unfair in combination.

Product brief
Stakeholders: underwriting leaders, actuarial and risk teams, compliance, and model risk management.
Problem: manual underwriting is slow and inconsistent. Machine learning can speed it up, but it can also make unfair or opaque decisions at scale.
Success metric: accurate decisions that are explainable and that do not silently exclude any segment.
Guardrail metrics: fairness gaps (single-attribute and intersectional), explainability coverage of top decision drivers, and any segment with zero approvals.
Key findings
Model performance. I compared multiple algorithms on a public medical cost dataset. XGBoost performed best, at about 95% accuracy and F1.
Single-attribute fairness looked fine. Gender metrics satisfied standard thresholds.
The intersectional audit exposed the real risk. Smoker status behaved like an absolute veto: smokers were fully excluded, even where other attributes were favorable.
Why it matters. A single-attribute audit would have given false comfort. Fairness checks need to cover combinations of attributes, not just one at a time.
Data

No real policyholder, claims, or personal data is used.

Source: public medical cost dataset. TODO: add dataset name, link, license, and whether it is synthetic or a public anonymized dataset.
Target: TODO: define the prediction target (e.g., high-cost vs. low-cost, approve vs. decline).
Split and seed: TODO: add train/test split and random seed.

Because the data is not from a real insurer, the results show how the pipeline behaves. They do not describe any real insurer's population.

Method: pipeline stages
Pass
Fail
Public medical cost dataset
Preprocessing
Train and compare models
Accuracy and F1
Fairness audit: singleattribute
Fairness audit: intersectional
Explainability: SHAP
Governance gate
Deployment candidate
Mitigate and re-audit

Each stage is named and reviewable, so none of them can be skipped.

Key decisions and trade-offs
Decision	Options considered	Choice	Why
What to optimize	Accuracy only vs. accuracy plus fairness	Report both	Accuracy alone hides who gets excluded
Audit depth	Single attribute vs. intersectional	Both	The single-attribute audit passed. Only the intersectional one found the veto.
Explainability	None vs. SHAP	SHAP	Underwriters and compliance need to see what drives each decision
Fairness threshold	TODO: name the threshold used		
Results
Model	Accuracy	F1
XGBoost	~95%	~95%
TODO: other models tested		
Fairness audit
Audit	Outcome
Gender (single attribute)	Met standard thresholds
Smoker status (intersectional)	Complete exclusion of the smoker sub-segment

TODO: add the numbers from the notebook (approval rates per group, gap sizes).

What this means for the product

A hard approve/decline rule that keys on one attribute is a product decision, not just a modeling outcome. Options to evaluate with underwriting, legal, and compliance:

Move from binary exclusion to risk-based pricing for the affected segment.
Route edge segments to a human underwriter instead of auto-declining.
Define what is permitted by regulation in each jurisdiction before deploying.

This is a design discussion, not legal advice.

Governance guardrails (before any real deployment)
Mandatory intersectional fairness audit as a release gate.
SHAP review of top decision drivers, documented in a model card.
Human review path for segments with extreme approval or decline rates.
Ongoing monitoring of drift and fairness metrics after launch.
Clear escalation thresholds and an owner for each guardrail.
Limitations and responsible use
Results are from a public dataset, not a real insurer's population. They do not generalize.
Intersectional groups can be small, which makes estimates noisy.
Protected attributes may be encoded indirectly through other variables.
This is a research prototype. It must not be used to make real underwriting decisions.
Repository structure
ai-responsible-underwriting-fairness/
├── README.md
├── notebooks/01_responsible_ai_underwriting.ipynb
├── src/responsible_ai.py
├── docs/Responsible_AI_Predictive_Underwriting.pptx
├── requirements.txt
├── LICENSE
└── .gitignore
How to run
bash
git clone https://github.com/gjoazil/ai-responsible-underwriting-fairness.git
cd ai-responsible-underwriting-fairness
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt   # pandas, scikit-learn, xgboost, shap (+ any fairness library you used)
jupyter notebook notebooks/01_responsible_ai_underwriting.ipynb
Roadmap
Now: fill in the numbers above and publish with the cleaned repo structure.
Next: add a model card and a one-page governance checklist.
Later: test mitigation methods (reweighting, threshold tuning) and re-run the intersectional audit.
Contact
