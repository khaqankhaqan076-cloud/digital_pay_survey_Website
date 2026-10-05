# digital_pay_survey_Website
its a survey for data collection process to get data for the data analysis and analyze real world data to complete the  analysys
# DigitalPay Research: Digital Payment Habits & Financial Behavior

An academic data analysis project that studies how students use digital payment methods and how digital payments affect their spending and financial behavior. Primary data is collected through a custom-built survey website that writes every response to a Google Sheet.

---

## 1. Project Overview

| Item | Details |
|---|---|
| **Title** | Digital Payment Habits & Financial Behavior |
| **Type** | Primary data collection + data analysis |
| **Population** | Students (school, college and university, including Bachelor's, Master's and PhD) |
| **Region** | Pakistan (amounts recorded in PKR) |
| **Collection method** | Online self-administered questionnaire |
| **Estimated completion time** | 4–6 minutes |
| **Storage** | Google Sheets via Google Apps Script |

### Research Questions
1. Which digital payment methods do students use most, and how often?
2. Does digital payment convenience lead to unplanned or higher spending?
3. How do budgeting, saving and expense-tracking habits relate to digital payment use?
4. How do trust, security concerns and scam experience affect usage?
5. How satisfied are students overall, and do they intend to keep using digital payments?
6. Do the answers differ by education level, age group, gender or source of money?

---

## 2. Data Collection

### 2.1 Survey Website
A single-file website (`index.html`) built with HTML, CSS and vanilla JavaScript. It has no framework or backend server.

- 7 survey sections shown one at a time with a progress bar
- Required-answer validation with inline messages
- Conditional questions based on education level
- A custom black and cyan interface that works on mobile and desktop

### 2.2 How Responses Reach the Dataset

```
Respondent → index.html → fetch() POST (JSON) → Google Apps Script → Google Sheet → CSV export → Analysis
```

1. The respondent completes the survey on the website.
2. When they click **Submit Survey**, the site collects all answers into one JSON object and adds a `submittedAt` timestamp (ISO 8601).
3. The data is sent with a POST request to a Google Apps Script web app (`no-cors` mode).
4. The script appends one row per response to the Google Sheet.
5. The sheet is exported as CSV for cleaning and analysis.

A copy of the last submission is also stored in the browser's `localStorage` as a backup on the respondent's device.

### 2.3 Sampling
- **Method:** convenience / voluntary response sampling, shared through student groups and social media
- **Target sample size:** *[add your target, e.g. 150–300 responses]*
- **Collection period:** *[start date] to [end date]*

---

## 3. Survey Structure

| # | Section | Topics |
|---|---|---|
| 1 | Student Profile | Age, gender, education level, class / degree / program / semester, monthly spending, main source of money |
| 2 | Digital Payment Usage | Own account or wallet, methods used, frequency, device, QR payments, duration, main purpose |
| 3 | Spending Behavior | Weekly transactions, monthly digital spend, spending category, impulse buying, spending more than with cash |
| 4 | Financial Management | Budgeting, expense tracking, saving, financial knowledge, credit / BNPL use, checking transaction history |
| 5 | Security & Trust | Fraud concern, scam or phishing victim, privacy trust, overall trust |
| 6 | Problems & Satisfaction | Failed transactions, most common problem, overall satisfaction |
| 7 | Overall Impact | Impact on money management, future use, recommendation to others |

### Conditional Questions
| If `edu` is | These fields are asked |
|---|---|
| School | `schoolclass` |
| College | `collegeclass` |
| University | `degree`, `program`, `semester` |

Fields that do not apply to a respondent are left empty in the sheet.

---

## 4. Data Dictionary

| Field | Description | Type |
|---|---|---|
| `submittedAt` | Submission time (ISO 8601, UTC) | Datetime |
| `age` | Age group | Ordinal |
| `gender` | Gender | Nominal |
| `edu` | Education level (School / College / University) | Nominal |
| `schoolclass` | Current school class | Text |
| `collegeclass` | Current college class / year | Text |
| `degree` | Bachelor's / Master's / PhD | Nominal |
| `program` | Degree program | Text |
| `semester` | Semester or year | Text |
| `spending` | Monthly personal spending band (PKR) | Ordinal |
| `source` | Main source of money | Nominal |
| `use` | Uses digital payments (Yes / No) | Binary |
| `account` | Own account / wallet status | Nominal |
| `methods` | Payment methods used (**multiple answers**) | Multi-select |
| `frequency` | How often payments are made | Ordinal |
| `device` | Device mostly used | Nominal |
| `qr` | QR payment use | Ordinal |
| `duration` | Time using digital payments | Ordinal |
| `purpose` | Main reason for using digital payments | Nominal |
| `transactions` | Transactions per week | Ordinal |
| `digitalspend` | Monthly digital spending band (PKR) | Ordinal |
| `category` | Most common spending category | Nominal |
| `impulse` | Unplanned purchases because of convenience | Likert (5) |
| `more` | Spends more than with cash | Likert (5) |
| `budget` | Sets a monthly budget | Ordinal |
| `track` | Tracks expenses | Ordinal |
| `save` | Saves regularly | Ordinal |
| `literacy` | Financial knowledge self-rating | Ordinal |
| `credit` | Uses BNPL / credit | Ordinal |
| `history` | Checks transaction history | Likert (5) |
| `security` | Concern about fraud | Ordinal |
| `scam` | Victim of a payment scam or phishing | Nominal |
| `privacy` | Trusts services to protect information | Likert (5) |
| `trust` | Overall trust in digital payments | Ordinal |
| `problem` | Failed / delayed transaction frequency | Ordinal |
| `issue` | Most common problem | Nominal |
| `satisfaction` | Overall satisfaction | Ordinal |
| `impact` | Improved money management | Likert (5) |
| `future` | Will keep using digital payments | Ordinal |
| `recommend` | Would recommend to other students | Ordinal |

> **Note:** `methods` can contain several values in one cell (for example `Easypaisa, JazzCash`). Split it into separate yes/no columns during cleaning.

---

## 5. Data Cleaning Plan

1. Export the Google Sheet as CSV and keep the **raw file unchanged**.
2. Remove test submissions and obvious duplicates (same `submittedAt` and identical answers).
3. Standardize text fields (`program`, `semester`, `schoolclass`, `collegeclass`): trim spaces, fix case and spelling.
4. Split `methods` into one binary column per payment method.
5. Convert Likert and ordinal answers to ordered categories or numeric scores (for example Strongly Disagree = 1 to Strongly Agree = 5).
6. Check for missing values, which should only appear in conditional fields.
7. Save the cleaned dataset as a new file.

---

## 6. Analysis Plan

- **Descriptive analysis:** frequencies and percentages for every question, plus summary tables by education level
- **Cross-tabulation:** payment frequency × education level, impulse buying × digital spend, scam victim × trust
- **Association tests:** chi-square tests for categorical variables, Spearman correlation for ordinal scales
- **Group comparison:** Mann-Whitney U or Kruskal-Wallis tests for Likert scores across groups
- **Visualization:** bar charts, stacked Likert charts, heatmaps and a summary dashboard
- **Suggested tools:** Excel, Python (pandas, matplotlib / seaborn, scipy), Power BI

---

## 7. Repository Structure

```
├── index.html          # Survey website (data collection)
├── README.md           # Project documentation
├── data/
│   ├── raw/            # Original CSV export from Google Sheets (do not edit)
│   └── cleaned/        # Cleaned dataset
├── notebooks/          # Analysis notebooks
├── reports/            # Final report and charts
└── dashboard/          # Power BI / Excel files
```

---

## 8. Running the Survey Locally or Online

- **Locally:** open `index.html` in any modern browser.
- **Online:** upload `index.html` to Netlify Drop or GitHub Pages and share the link.
- **Before sharing:** submit one test response and confirm a new row appears in the Google Sheet. Delete the test row afterward.

The Google Apps Script must be deployed as a web app with access set to **Anyone**, and the `SCRIPT_URL` inside `index.html` must match the deployed URL. If new questions are added, add matching columns in the sheet or update the script.

---

## 9. Ethics & Privacy

- Participation is voluntary and anonymous.
- No names or personally identifying details are requested.
- Students under 18 should have permission from a parent or guardian.
- Data is used only for academic research purposes.
- Restrict access to the Google Sheet to the research team.
- Report results in aggregate form only.

---

## 10. Limitations

- Convenience sampling means results may not represent all students.
- Answers are self-reported and may be affected by recall or social desirability bias.
- Online distribution may favor students who already use smartphones and social media.
- Because the survey uses `no-cors` submission, the website cannot confirm that the sheet received each response. Check the sheet regularly.

---

## 11. Author

**[Your Name]**
[Program / Institution]
[Email, optional]
[Supervisor, if any]

---

## 12. License

For academic and educational use. *[Add a license if you publish the repository, for example MIT.]*
