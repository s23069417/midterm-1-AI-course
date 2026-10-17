# Midterm 1: answers

GitHub username: s23069417
Session code: MAPLE-66
Fingerprint: 1cdb3e29

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used Claude (Anthropic) for the following purposes: 
To quickly understand the task and theory of the course (fragments, repetition, Wilson intervals, temperature, YIN and personal data). Check the Python code that calculates the recall, MRR, Wilson intervals, annual costs, and figures from Part 3.

I did the calculations myself, using my own data in Colab, and checked if the results matched the formulas in the README. Then I made a choice, which option is better and which option to choose, I also wrote the solution in my own way.  I used Yandex Translate to translate from one language to another.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.483 | 0.583 | 0.667 | 0.542 |
| B | 0.450 | 0.567 | 0.817 | 0.556 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.583 | 0.457 | 0.699 |
| A, k=5 | 0.667 | 0.541 | 0.773 |
| B, k=3 | 0.567 | 0.441 | 0.684 |
| B, k=5 | 0.817 | 0.701 | 0.894 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 10876 |
| A, k=5 | 14308 |
| B, k=3 | 7065 |
| B, k=5 | 7956 |

### 1d. Decision (at most 150 words)
Chosen candidate: B, k=5
(A,5) costs $14,308, above the $12,600 budget, so it is out. Among the affordable options, (B,5) has the highest recall, 0.817 (49/60, 95% CI 0.701-0.894), and costs only $7,956 (63% of the budget). Its interval does not overlap those of (A,3) (0.457-0.699) and (B,3) (0.441-0.684), so it is shown to be better than both, although the gap to (A,3) is very small. It is also cheaper than (A,3) ($10,876). Recall@5 by language: EN 0.80 (16/20), RU 0.90 (18/20), KK 0.75 (15/20). Kazakh users are served worst, but with 20 questions per language the intervals overlap (KK 0.53-0.89), so that gap is not proven.

### 1e. Next measurement (at most 60 words)
Collect at least 100 test questions per language and re-measure recall@5 of (B,5) separately for EN, RU and KK. If the KK interval stays clearly below RU and EN, the decision changes: I would test a larger k (for example k=7) for Kazakh requests. If the intervals overlap, B, k=5 stays.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
|---|---|---|---|---|---|---|
| H | 0.917 | KK, degraded | 0.867 | 0.703 | 0.947 | 26077 |
| L | 0.817 | KK, clean  | 0.733 | 0.556 | 0.858 | 10800 |

Note: for L the weakest cell is a tie: KK clean and KK degraded both 22/30 (0.733).
Personal-data pages per year sent abroad under H: 260975

### 2b. Memo (at most 300 words)
Recommendation: pipeline L, with safeguards.

Who is served worst. Under H the weakest cell is Kazakh degraded scans: 0.867 (26/30, 95% CI 0.703-0.947). Under L it is Kazakh scans in both conditions: 0.733 (22/30, CI 0.556-0.858). Overall figures hide this: L's overall 0.817 looks acceptable, but under L Kazakh pages are far below English clean (0.93). With only 30 invoices per cell the intervals of H and L overlap, so the gap between them in the worst cell is not proven. H is better overall (0.917 vs 0.817).

Personal data. Some scans carry a name and ИИН (Law No. 94-V). Under H about 260975 such pages a year would leave the country. Where does the scan go? Abroad (known). Who stores it? Unknown. For how long? Unknown. I would have to find out retention, use for training, sub-processors and the legal basis for transfer; a vendor's statement is not a compliance review. Under L the scan goes to our server in Kazakhstan, we store it and we set retention (known).

Safeguards someone can verify: a written retention limit with deletion logged and sampled monthly; an access list for the server; network logs showing no outbound traffic carrying scans; human review of all Kazakh and low-confidence pages; a monthly audit of 100 random pages.

Choice. L costs $10,800 a year against $26,077 for H and keeps ИИН data in Kazakhstan, but is about ten points less accurate. Residual risk: roughly one in four Kazakh invoices is misread, so errors reach payment unless reviewers catch them. Reviewer workload is the cost I accept.


## Part 3: Explain your own numbers

### Q1

Number: 1.33
Why (at most 40 words): Part of the Kazakh premium is encoding: bytes grow 2.15x because Cyrillic takes two bytes. Tokens grow 2.86x, so the remaining 1.33x comes from the tokenizer splitting Kazakh into more pieces.

### Q2

Number: 2.7 %
Why (at most 40 words): Only English is cut. 512 cl100k tokens become 526 e5 tokens, so the last 14 are over the 512 limit. Russian (256) and Kazakh (152) fit. The cut part is dropped, never embedded, so it cannot be found.

### Q3

Number: 44.4 %
Why (at most 40 words): T=2 makes the probabilities flatter but the ranking stays the same: Monday is still first. All five tokens still have nonzero probability, so the set that can be sampled is unchanged, just more random.


### Q4

Number: 13
Why (at most 40 words): The public leaderboard prefers X (66 vs 62 correct). With a penalty for wrong answers, Y wins 45 to 32. A leaderboard that gives no credit for "I don't know" rewards guessing, so models learn to bluff.

### Q5

Number: 19190
Why (at most 40 words): By turn 19 the first message has been paid for 19 times, so total cost grows with the square of the turns. Prompt caching makes the repeated history cheaper, but it is still sent every turn.
