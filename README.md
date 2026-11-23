# econ3916-lab17-classification
Lab 17 Submission

# Classification: When 90% Accurate Is Worthless

## Objective
I built a loan-default classifier and showed why accuracy fails under class imbalance, then chose a decision threshold based on the cost of each type of error instead of the default 0.5.

## Methodology
- Simulated 6,000 loan applications with a default rate of 9.6%, where a rule that approves everyone is 90.4% accurate.
- Split the data 70/30 into training and test sets, stratified on default so both sets keep the same default rate.
- Fitted a scaled logistic regression in scikit-learn and compared its accuracy and confusion matrix with the approve-everyone rule.
- Refitted the model in statsmodels to read each coefficient as a change in log-odds and as an odds ratio, then applied the odds ratio for one more prior default at starting probabilities of 2%, 10% and 40%.
- Traced precision, recall and accuracy across six thresholds, from 0.5 down to 0.04.
- Priced the two errors (a false negative loses $12,000, a false positive loses $1,600) and computed the closed-form threshold, FP cost / (FP cost + FN cost).
- Found the cost-minimising threshold by grid search on cross-validated training probabilities, then priced it once on the test set.
- Wrote `precision_recall_at()`, a function that returns a classifier's precision and recall for any model, rows and threshold.

## Key Findings
- At the 0.5 threshold, the model's accuracy was 90.4%, the same as approving everyone, with 0 predicted defaults out of 1,800 test applicants. Every defaulter was approved.
- The model still learned something: at lower thresholds its declines held far more defaulters than the 9.6% base rate, so the 0.5 cutoff was hiding it.
- The odds ratio for one more prior default was 1.63, which raised the probability of default from 2% to 3.2%, from 10% to 15.3%, and from 40% to 52.0%. The same odds ratio means different probability changes depending on the starting point.
- The cost-minimising threshold was 0.121, close to the closed-form 0.118. Its expected cost was $944 per applicant on the test set, against $1,147 at 0.5, so the default threshold costs 21% more.
- At the closed-form threshold, precision was [YOUR VALUE] and recall was [YOUR VALUE].
