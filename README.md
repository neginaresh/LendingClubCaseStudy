# Lending Club Case

## Table of Contents

1. [Problem Statement](#problem-statement)
2. [Objectives](#objectives)
3. [Data Understanding](#data-understanding)
   - [Data Understanding Domain](#data-understanding-domain)
   - [Decision Matrix](#decision-matrix)
   - [Important Columns](#important-columns)
   - [Ignored Columns](#ignored-columns)
4. [Data Understanding EDA](#data-understanding-eda)
   - [Rows Analysis](#rows-analysis)
   - [Columns Analysis](#columns-analysis)
     - [Drop Columns](#drop-columns)
     - [Convert Column Format](#convert-column-format)
     - [Standardize Values](#standardize-values)
     - [Convert Column Values](#convert-column-values)
     - [Added New Columns](#added-new-columns)
5. [Missing Data Rules](#missing-data-rules)
   - [Column Dropping Rules](#column-dropping-rules)
6. [Outlier Treatment Rules](#outlier-treatment-rules)

# Problem Statement
Lending Club is a consumer finance marketplace for personal loans that matches borrowers who are seeking a loan with investors looking to lend money and make a return. It specializes in lending various types of loans to urban customers. When the company receives a loan application, the company has to make a decision for loan approval based on the applicant's profile.

Like most other lending companies, lending loans to ‘risky’ applicants is the largest source of financial loss (called credit loss). The credit loss is the amount of money lost by the lender when the borrower refuses to pay or runs away with the money owed.

In other words, borrowers who default cause the largest amount of loss to the lenders. In this case, the customers labelled as 'charged-off' are the 'defaulters'.

The core objective of the exercise is to help the company minimize the credit loss. There are two potential sources of credit loss:

1. Applicant likely to repay the loan, such an applicant will bring in profit to the company with interest rates. Rejecting such applicants will result in loss of business.
2. Applicant not likely to repay the loan, i.e., will potentially default, then approving the loan may lead to a financial loss for the company.

# Objectives
The goal is to identify these risky loan applicants so that such loans can be reduced, thereby cutting down the amount of credit loss. Identification of such applicants using EDA using the given dataset is the aim of this case study.

In other words, the company wants to understand the driving factors (or driver variables) behind loan default, i.e., the variables which are strong indicators of default. The company can utilize this knowledge for its portfolio and risk assessment.

# Data Understanding
The dataset contains information about past loan applicants and whether they ‘defaulted’ or not. The aim is to identify patterns that indicate if a person is likely to default, which may be used for taking actions such as denying the loan, reducing the amount of loan, lending (to risky applicants) at a higher interest rate, etc.

The dataset reflects loans post-approval, thus does not represent any information on the rejection criteria process. The overall objective will be to observe key leading indicators (driver variables) in the dataset, which contribute to defaulters. Use the analysis as a foundation for the hypothesis.

## Data Understanding Domain
### Leading Attribute
Loan Status - Key Leading Attribute (loan_status). The column has three distinct values:
- Fully-Paid: The customer has successfully paid the loan.
- Charged-Off: The customer is "Charged-Off" or has "Defaulted."
- Current: These customers, the loan is currently in progress and cannot contribute to conclusive evidence if the customer will default or pay in the future. For the given case study, "Current" status rows will be ignored.

### Decision Matrix
Loan Accepted - Three Scenarios:
- Fully Paid: Applicant has fully paid the loan (the principal and the interest rate).
- Current: Applicant is in the process of paying the installments, i.e., the tenure of the loan is not yet completed. These candidates are not labeled as 'defaulted.'
- Charged-off: Applicant has not paid the installments on time for a long period, i.e., he/she has defaulted on the loan.

Loan Rejected - The company had rejected the loan (because the candidate does not meet their requirements, etc.). Since the loan was rejected, there is no transactional history of those applicants with the company, and so this data is not available in this dataset.

### Important Columns
The given columns are leading attributes or predictors. These attributes are available at the time of the loan application and strongly help in predicting loan pass or rejection. Key attributes Some of these columns may get dropped due to empty data in the dataset:

#### Customer Demographics
- Annual Income (annual_inc): Annual income of the customer. Generally, the higher the income, the more chances of loan pass.
- Home Ownership (home_ownership): Whether the customer owns a home or stays rented. Owning a home adds collateral, which increases the chances of loan pass.
- Employment Length (emp_length): Employment tenure of a customer (this is overall tenure). The higher the tenure, the more financial stability, thus higher chances of loan pass.
- Debt to Income (dti): The percentage of the salary which goes towards paying the loan. Lower DTI, higher the chances of a loan pass.
- State (addr_state): Location of the customer. Can be used to create a generic demographic analysis. There could be higher delinquency or defaulters demographically.

#### Loan Attributes
- Loan Amount (loan_amt)
- Grade (grade)
- Term (term)
- Loan Date (issue_date)
- Purpose of Loan (purpose)
- Verification Status (verification_status)
- Interest Rate (int_rate)
- Installment (installment)
- Public Records (public_rec): Derogatory Public Records. The value adds to the risk of the loan. The higher the value, the lower the success rate.
- Public Records Bankruptcy (public_rec_bankruptcy): Number of bankruptcy records publicly available for the customer. The higher the value, the lower is the success rate.

### Ignored Columns
The following types of columns will be ignored in the analysis. This is a generic categorization of the columns, which will be ignored in our approach and not the full list:
- Customer Behavior Columns: Columns that describe customer behavior will not contribute to the analysis. The current analysis is at the time of the loan application, but the customer behavior variables generate post the approval of loan applications. Thus these attributes will not be considered towards the loan approval/rejection process.
- Granular Data: Columns that describe the next level of details which may not be required for the analysis. For example, grade may be relevant for creating business outcomes and visualizations, sub-grade is very granular and will not be used in the analysis.

## Data Understanding EDA
### Rows Analysis
- Summary Rows: No summary rows were there in the dataset.
- Header & Footer Rows: No header or footer rows in the dataset.
- Extra Rows: No column number, indicators, etc., found in the dataset.
- Rows where the loan_status = CURRENT will be dropped as CURRENT loans are in progress and will not contribute to the decision making of pass or fail of the loan. The rows are dropped before the column analysis as it also cleans up unnecessary columns related to CURRENT early and columns with NA values can be cleaned in one go.
- Find duplicate rows in the dataset and drop if there are.

### Columns Analysis
#### Drop Columns
- There are multiple columns with NA values only. The columns will be dropped.
- This is evaluated after dropping rows with loan_status = Current.
- (next_pymnt_d, mths_since_last_major_derog, annual_inc_joint, dti_joint, verification_status_joint, tot_coll_amt, tot_cur_bal, open_acc_6m, open_il_6m, open_il_12m, open_il_24m, mths_since_rcnt_il, total_bal_il, il_util, open_rv_12m, open_rv_24m, max_bal_bc, all_util, total_rev_hi_lim, inq_fi, total_cu_tl, inq_last_12m, acc_open_past_24mths, avg_cur_bal, bc_open_to_buy, bc_util, mo_sin_old_il_acct, mo_sin_old_rev_tl_op, mo_sin_rcnt_rev_tl_op, mo_sin_rcnt_tl, mort_acc, mths_since_recent_bc, mths_since_recent_bc_dlq, mths_since_recent_inq, mths_since_recent_revol_delinq, num_accts_ever_120_pd, num_actv_bc_tl, num_actv_rev_tl, num_bc_sats, num_bc_tl, num_il_tl, num_op_rev_tl, num_rev_accts, num_rev_tl_bal_gt_0, num_sats, num_tl_120dpd_2m, num_tl_30dpd, num_tl_90g_dpd_24m, num_tl_op_past_12m, pct_tl_nvr_dlq, percent_bc_gt_75, tot_hi_cred_lim, total_bal_ex_mort, total_bc_limit, total_il_high_credit_limit).
- There are multiple columns where the values are only zero. The columns will be dropped.
- There are columns where the values are constant. They don't contribute to the analysis; columns will be dropped.
- There are columns where the value is constant but the other values are NA. The column will be considered as constant; columns will be dropped.
- There are columns where more than 65% of data is empty (mths_since_last_delinq, mths_since_last_record) - columns will be dropped.
- Drop columns (id, member_id) as they are index variables and have unique values and don't contribute to the analysis.
- Drop columns (emp_title, desc, title) as they are descriptive and text (nouns) and don't contribute to analysis.
- Drop redundant columns (url). On closer analysis, the URL is a static path with the loan id appended as a query. It's a redundant column to (id) column.
- Drop customer behavior columns which represent data post the approval of loan.
- They contribute to the behavior of the customer. Behavior of the customer is recorded post-approval of loan and not available at the time of loan approval. Thus these variables will not be considered in the analysis and thus dropped.
- (delinq_2yrs, earliest_cr_line, inq_last_6mths, open_acc, pub_rec, revol_bal, revol_util, total_acc, out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee, last_pymnt_d, last_pymnt_amnt, last_credit_pull_d, application_type).

#### Convert Column Format
- (loan_amnt, funded_amnt, funded_amnt_inv) columns are Object and will be converted to float.
- (int_rate, installment, dti) columns are Object and will be converted to float.
- strip "month" text from term column and convert to an integer.
- Percentage columns (int_rate) are objects. Strip "%" characters and convert the column to float.
- issue_d column converted to datetime format.

#### Standardize Values
- All currency columns are rounded off to 2 decimal places as currency is limited to cents/paise, etc.

#### Convert Column Values
- loan_status column converted to boolean Charged Off = False and Fully Paid = True. This converts the column into ordinal values.
- emp_length converted to an integer with the following logic. Note < 1 year is converted to zero and 10+ converted to 10.
  < 1 year: 0,
  2 years: 2,
  3 years: 3,
  7 years: 7,
  4 years: 4,
  5 years: 5,
  1 year: 1,
  6 years: 6,
  8 years: 8,
  9 years: 9,
  10+ years: 10

#### Added New Columns
- verification_status_n added. Considering domain knowledge of lending = Verified > Source Verified > Not Verified. verification_status_n correspond to {Verified: 3, Source Verified: 2. Not Verified: 1} for better analysis.
- issue_y is the year extracted from issue_d.
- issue_m is the month extracted from issue_d.

## Missing Data Rules
Columns with a high percentage of missing values will be dropped (65% above for this case study).
Columns with a fewer percentage of missing values will be imputed.
Rows with a high percentage of missing values will be removed (65% above for this case study).
Column Dropping Rules
The approach taken here in this analysis, if the total number of rows (for all columns) which are blank is less than 5% of the dataset, we are dropping the rows. If the total rows are greater than 5%, we will impute.
If the dataset of blanks is considerably small, dropping the rows will possibly be a more accurate approach without impacting the dataset and the outcomes.
If the dataset of blanks is considerably large, dropping the rows will skew the analysis and impute approach will be taken.
In the current dataset, the combined row count of blanks for emp_length and pub_rec_bankruptcies is 1730, which is 4.48% of the total rows; thus, dropping the rows will be the more accurate approach.
If imputing, we will correlate emp_length with annual_inc, with the logic that the higher the length of employment, the higher the salary potentially. With this approach, the outliers can potentially introduce noise.

## Outlier Treatment Rules
The approach taken in this analysis is to drop all outlier rows.
The following columns were evaluated for outliers: loan_amnt, funded_amnt, funded_amnt_inv, int_rate, installment, annual_inc, dti.
Total rows dropped due to outlier treatment: 3791.
Percentage of rows dropped due to outlier treatment: 10.29%.


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->
