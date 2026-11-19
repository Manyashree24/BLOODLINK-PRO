# 🩸 BloodLink Pro – Blood Bank & Donor Management System

> A complete blood-bank management web app. It covers donor eligibility screening, a donor registry, request-to-donor matching, inventory with expiry tracking, compatibility rules for every blood component, and a **machine-learning model that predicts which donors are likely to donate again**.

🔗 **Live demo:** https://&lt;your-username&gt;.github.io/bloodlink/
&nbsp;·&nbsp; Click **✨ Load demo data** to explore with 48 fictional donors, 46 blood bags and open requests.

![Dashboard](docs/screenshots/dashboard.png)

---

## 📌 Problem statement
Blood banks face three problems every day:
1. **Shortages** of rare groups (O−, A−, B−, AB−) during emergencies
2. **Wastage**, because components expire (platelets last only 5 days)
3. **Slow donor outreach**. Staff call donors one by one without knowing who is eligible or likely to respond.

**BloodLink Pro** puts stock, donors and requests on one dashboard. It matches a request with compatible units (oldest first) and ranks eligible donors by location and by an ML-predicted chance that they will return.

## ✨ Features
| Module | What it does |
|---|---|
| **Dashboard** | KPIs, red-cell stock per group with low-stock colour coding, units by group, component and donor-group charts, a 14-day expiry chart, and alerts for critical requests, low stock and expired bags |
| **Eligibility screening** | Age, weight, haemoglobin, BP, donation gap (90 days for men, 120 for women) and 9 deferral questions. Shows the earliest date you can donate again. You can register as a donor straight from the result |
| **Donor registry** | Search and filter by group, city and current eligibility, live eligibility status, **AI return-likelihood score**, and a "Donate" flow that records the donation and adds bags to inventory (whole blood, or split into PRBC + FFP + platelets). Low haemoglobin on the day defers the donor automatically |
| **Requests & matching** | Create a request (group, component, units, urgency, hospital, city). Matching lists compatible bags (same group first, then FIFO by expiry) and allocates them, ranks donors (nearby → exact group → AI score), and writes a ready-to-send donor appeal |
| **Inventory** | Bag IDs, expiry set automatically from shelf life (whole blood 35 days, PRBC 42, platelets 5, FFP 365), status filters, discard expired, CSV export |
| **Compatibility** | Separate rules for red cells, plasma, platelets and whole blood. Group picker, 8×8 matrix, an antigen/antibody explanation, and live counts of matching donors and units |
| **Donor AI** | RFMT logistic-regression model with metrics, confusion matrix, feature weights, dataset statistics and an interactive predictor. **You can upload the real UCI dataset** to retrain it |
| **Extras** | Dark mode, responsive layout, custom dialogs, data stays in the browser |

## 🤖 Machine-learning model – donor return prediction
Based on the **RFMT** framework used in blood-donation research (Yeh et al., 2009):

| Feature | Meaning |
|---|---|
| Recency | Months since last donation |
| Frequency | Total number of donations |
| Time | Months since first donation |
| Rate | Frequency ÷ Time (donations per month) |

- **Algorithm:** logistic regression written from scratch (`js/ml.js`) with standardisation, L2 regularisation, balanced class weights, a stratified 75/25 split, and accuracy / precision / recall / F1 / ROC-AUC metrics.
- **Data:** by default the app trains on a **synthetic dataset of 748 donors**. It follows the structure of the public **UCI "Blood Transfusion Service Center" dataset**. To train on real data, download `transfusion.data` from the UCI Machine Learning Repository and upload it on the *Donor AI* page. The parser skips the header row automatically.
- **Use in the app:** every registered donor is scored. Scores drive the donor ranking in request matching, so staff call the donors most likely to respond first.

![Donor AI](docs/screenshots/ai.png)

## 🧬 Compatibility rules implemented
- **Red cells:** the recipient must not have antibodies against donor antigens. Rh− recipients get Rh− cells. O− is the universal donor; AB+ is the universal recipient.
- **Plasma:** the reverse rule. AB is the universal plasma donor; O is the universal plasma recipient.
- **Platelets:** ABO plasma-compatible.
- **Whole blood:** ABO-identical, with the Rh rule.

## 🛠️ Tech stack
HTML5 · CSS3 · vanilla JavaScript · custom ML (`ml.js`) · custom SVG charts (`charts.js`) · localStorage · Node test runner · GitHub Pages. **No dependencies and no build step.**

## 📂 Project structure
```
bloodlink/
├── index.html            # App shell and all views
├── css/base.css          # Design system
├── js/
│   ├── ui.js             # Router, modals, toasts, storage, CSV, export
│   ├── ml.js             # Logistic regression + metrics (from scratch)
│   ├── charts.js         # SVG charts
│   ├── blood.js          # Domain rules: compatibility, shelf life, eligibility, RFMT, UCI parser
│   └── app.js            # App logic and views
├── tests/core.test.js    # Unit tests (compatibility, eligibility, ML)
├── docs/screenshots/
├── package.json
└── README.md
```

## 🚀 Run locally
```bash
git clone https://github.com/<your-username>/bloodlink.git
cd bloodlink
python -m http.server 8000     # visit http://localhost:8000  (or just open index.html)
```

## ✅ Run the tests
```bash
npm test          # or: node --test
```

## 🌐 Deploy to GitHub Pages
Go to **Settings → Pages → Deploy from a branch → `main` / root**. The app will be at `https://<your-username>.github.io/bloodlink/`.

## 📸 Screenshots
| Donor registry | Compatibility |
|---|---|
| ![Donors](docs/screenshots/donors.png) | ![Compatibility](docs/screenshots/compatibility.png) |

## 🔮 Future scope
- Multi-user backend (Node/Express or Firebase) shared by hospitals
- SMS / WhatsApp appeals to matched donors (Twilio)
- Map view of nearby donors and blood banks
- Demand forecasting per blood group (time-series model)
- Integration with the national e-RaktKosh portal

## ⚠️ Disclaimer
This is an educational project. The eligibility check is a pre-screen only. The blood bank's medical officer and laboratory cross-matching make the final decision. All demo donors are fictional.

## 👤 Author
**&lt;Manyashree;**, B.Tech CSE (AI & ML), Dayananda Sagar University, Bengaluru
GitHub: [@&lt;Manyashree24&gt;](https://github.com/Manyashree24)>)

## 📄 License
MIT. See [LICENSE](LICENSE).
