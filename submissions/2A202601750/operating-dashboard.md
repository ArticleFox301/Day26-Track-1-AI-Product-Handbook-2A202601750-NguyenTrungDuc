# Operating Dashboard — AI Invoice Reconciliation Agent

- Học viên: Nguyễn Trung Đức
- Mã học viên: 2A202601750
- Mô hình: B2B
- Cập nhật: 2026-08-28
- North Star: Median time-to-first-value ≤ 14 days

## Chẩn đoán mô hình

This is B2B because the CFO or chief accountant pays from the accounting-operations budget, AP accountants in that business use the agent, and the product has no independent relationship with suppliers or other end users; MISA is only the distribution partner.

| Dữ liệu đầu vào | Trạng thái | Nằm ở đâu hoặc cần gì để đo | Ngày có số |
|---|---|---|---|
| Unit economics Day 24 | Modelled, not market-validated | Base ARPU 1,490,000 VND/month, CAC 3,000,000 VND, GM 87.25%, payback 2.31 months | 2026-08-28 |
| Value Metric and Cost/Job Day 25 | Modelled; pilot pending | Outcome price $1.39/job, cost $0.407/job, GM 70.7%, assumed containment 82% | 2026-08-28 |

## Kiểm kê đèn ứng viên

| Đèn ứng viên từ handbook | Tầng | Trạng thái | Bằng chứng hiện có hoặc kế hoạch đo |
|---|---|---|---|
| Time-to-first-value (TTFV) | L | 🔧 | Kickoff and first 100 completed-job events by 2026-09-30 |
| Pipeline coverage | L | 🔧 | MISA referral CRM stages by 2026-09-30 |
| Percent deals blocked at security/procurement | L | 🔧 | Mandatory CRM reason by 2026-09-30 |
| POC to paid | O | 🔧 | Cohort sheet for design partners by 2026-10-31 |
| Sales cycle days | O | 🔧 | Referral and signature timestamps by 2026-10-31 |
| Usage depth per account | O | 🔧 | Completed jobs and AP users by 2026-09-30 |
| Implementation cost divided by ACV | O | 🔧 | Timesheet linked to contract ID by 2026-10-31 |
| Revenue concentration | O | 🔧 | Billing export after 10 paid customers |
| NRR | G | ❌ | Needs two quarterly cohorts; baseline 2027-03-31 |
| Gross Margin | G | 🔧 | Revenue joined to all variable costs by 2026-09-30 |
| CAC payback | G | 🔧 | Partner fees and labour by 2026-11-30 |

## Đèn báo sớm

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| L-01 | Time-to-first-value | Median days from kickoff to first 100 reconciled invoice sets with audit trail | Weekly · Product Ops | Unmeasured | ≤14 days | 15–21 days | >21 days | [TB] Baseline after 3 pilots; boundary protects the 30-day learning cycle | 2026-08-28 | Pilot activation in about 2 weeks | R-01 |
| L-02 | Reconciliation precision | Correct outcomes divided by audited completed jobs | Weekly · ML Lead | Assumed 98% | ≥98% | 95–97.9% | <95% | [TB] Audit 200 jobs/week and lock baseline 2026-09-30; current value is an assumption | 2026-08-28 | Conversion and rework in 2–4 weeks | R-02 |

## Đèn vận hành

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| O-01 | Straight-through containment | Jobs completed without human escalation divided by attempted jobs | Weekly · Product Ops | Assumed 82% | ≥82% | 75.0–81.9% | <75.0% | [MH] MH-02; below 75.0% the target 60% GM is infeasible | 2026-08-28 | Cost/job and GM in same week | R-03 |
| O-02 | AI Cost/Job | Token, inference, infra, retry and HITL cost divided by completed jobs | Weekly · FinOps | $0.407 | ≤$0.417 | $0.418–$0.556 | >$0.556 | [MH] MH-01 derives green from 70% GM and red from the 60% GM floor | 2026-08-28 | Gross margin in same month | R-04 |
| O-03 | POC-to-paid conversion | Paid contracts divided by pilots ending in month | Monthly · Partnership Lead | Unmeasured | ≥60% | 40–59% | <40% | [TB] Baseline on 5 pilots by 2026-11-30; 60% turns 5 pilots into 3 paid accounts | 2026-08-28 | CAC payback in 1–3 months | R-05 |

## Đèn kết quả

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| G-01 | Gross margin after AI cost | Revenue minus API, infra, retry and HITL, divided by revenue | Monthly · Finance | Modelled 70.7% | ≥70% | 60–69.9% | <60% | [MH] MH-01: 70% preserves price logic; 60% is the operating floor | 2026-08-28 | Unit-economics viability | R-04 |
| G-02 | CAC payback | Fully loaded acquisition cost divided by monthly gross profit/account | Quarterly · Finance | Modelled 12 months | ≤12 months | 12.1–18 months | >18 months | [TB] Actual partner fees and labour; baseline after 10 paid accounts by 2026-11-30 | 2026-08-28 | Cash runway | R-05 |

## Luật quyết định

| ID | NẾU | TRONG | VÀ | THÌ | KHÔNG THÌ | Luật dừng? |
|---|---|---|---|---|---|---|
| R-01 | Median TTFV >21 days | 2 pilot cohorts | ≥3 accounts and 300 attempted jobs | Stop new pilots for 14 days; cut onboarding to one workflow | Do not discount to compensate for delay | CÓ |
| R-02 | Precision <95% | 2 weekly audits | ≥200 audited jobs/week | Stop automatic posting; route low-confidence cases to approval and fix top 3 error classes | Do not shrink the audit sample | CÓ |
| R-03 | Containment <75.0% | 2 weeks | ≥1,000 attempts across 3 accounts | Freeze rollout; simplify rules and limit supported document types | Do not count escalations as AI-completed | CÓ |
| R-04 | AI Cost/Job >$0.556 | 2 weeks | ≥1,000 jobs with ≥95% ledger coverage | Limit context, route simple cases to cheaper model, renegotiate quota | Do not remove QA or exclude HITL | KHÔNG |
| R-05 | Conversion <40% or payback >18 months | 2 monthly cohorts | ≥5 ended pilots and 3 paid accounts | End broad promotion; retain one niche and renegotiate referral economics | Do not add pilots to hide conversion | KHÔNG |

## Cổng gác 90 ngày

| Ngày | Metric gác cổng | Ngưỡng | Bằng chứng vật lý | Nếu đạt | Nếu trượt |
|---:|---|---|---|---|---|
| 30 | Validated pain and outcome definition | ≥8 of 10 AP leads confirm both | Redacted transcripts and synthesis | GO | FIX |
| 60 | Straight-through containment | ≥75.0% over ≥3,000 attempts and precision ≥95% | Event-log cohort export and signed QA report | GO | PIVOT |
| 90 | Gross margin after variable costs | ≥60% over ≥10,000 jobs from ≥3 paid accounts | Billing export joined to cost ledger | GO | KILL |

## Kill criteria

KILL this product direction on day 90 if gross margin remains below 60% after two routing iterations, or precision remains below 95% on at least 1,000 audited jobs.

## Chưa đo được

| Đèn hoặc giả định | Cần gì để đo | Ai chịu trách nhiệm | Ngày có số |
|---|---|---|---|
| Assumed 82% containment and 98% precision | Event logs and stratified audit for 3 pilots and ≥3,000 attempts | ML Lead | 2026-09-30 |
| MISA access and referral economics | Written eligibility, sandbox access, fee schedule and named contact | Partnership Lead | 2026-09-30 |
| Actual CAC payback | Partner invoice and time ledger linked to 10 paid contracts | Finance Lead | 2026-11-30 |

## Phụ lục ngưỡng suy từ mô hình

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | Maximum AI Cost/Job | Price $1.39/job; target GM 70% | $1.39 × (1 − 70%) = $0.417 | O-02 green ≤$0.417; 60% floor gives $1.39 × 40% = $0.556; G-01 red <60% |
| MH-02 | Minimum containment | v=$0.095212/attempt; q=$0.0225/attempt; e=$1.20/escalation; P=$1.39/job; GM=60% | (v+q+e) ÷ (P×(1−GM)+e) = 1.317712 ÷ 1.756 = 75.04% | O-01 red <75.0%; green ≥82% retains modelled GM buffer |

## Ghi nhận AI critique

| Phản biện | Chấp nhận hay bác bỏ | Thay đổi đã thực hiện | Lý do |
|---|---|---|---|
| 82% containment and 98% precision are assumptions | Accepted | Marked unmeasured and added dated audit | Avoids fabricated evidence |
| Partner distribution does not imply B2B2C | Accepted | Classified B2B; MISA is distribution only | AP staff of payer are users |
| 70% GM target needs a survival floor | Accepted | Added 60% floor and $0.556 cost cap | Enables action before collapse |
