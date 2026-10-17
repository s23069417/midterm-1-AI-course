**Midterm 1: your data**

GitHub username: s23069417 · session code: maple-66 · generator v1

**Fingerprint: 1cdb3e29**: copy it into the first lines of ANSWERS.md.
All numbers below are synthetic: this is an exam, not a measurement.

**Part 1: inputs**

retrieval_ranks.csv: 60 test questions (20 EN, 20 RU, 20
KK). rank_A and rank_B give the position of the correct chunk in the top
10 retrieved under config A and config B; **11 means the correct chunk
was not in the top 10.**

Production traffic and Model M token counts (tokens per item, as counted
by Model M\'s tokenizer):

  -------------------------------------------------------------------------------
  **language**   **share of  **question    **answer     **chunk A    **chunk B
                 traffic**   tokens**      tokens**     tokens**     tokens**
  -------------- ----------- ------------- ------------ ------------ ------------
  EN             45 %        25            102          243          63

  RU             28 %        41            168          401          104

  KK             27 %        71            291          693          180
  -------------------------------------------------------------------------------

-   Requests per day: **11,500**, in the traffic shares above.

-   System prompt: **294** tokens in every request.

-   Price: $1**permillioninputtokens,**$**6** per million output tokens.

-   Annual budget for this service (model calls only): **\$12,600**.

**Part 2: inputs**

doc_results.csv: for each pipeline, language and scan
condition, n_docs test invoices and n_correct = invoices whose number,
date, seller ID and total were all extracted exactly (strict scorer, as
in Lab 04).

-   **Pipeline H**: hosted vision-language API in a data centre abroad,
    pay per token; each page is sent as an image.

-   **Pipeline L**: open vision-language model on your own server in
    Kazakhstan, fixed monthly cost.

-   Volume: **5,500** scanned pages per day. In **13 %** of them an
    individual\'s name and ИИН are printed (sole proprietors, receipts).

-   Page image: **1654 × 2339** pixels. Pipeline H counts an image as
    ⌈width ÷ 28⌉ × ⌈height ÷ 28⌉ input tokens and returns
    about **291** output tokens per page (the JSON).

-   Pipeline H price: $2**permillioninputtokens,**$**10** per million
    output tokens.

-   Pipeline L: one server costs **\$450** per month and handles up
    to **4,000** pages per day; add servers as needed.

**Part 3: your five questions**

Answer each in ANSWERS.md: one **Number** line and one **Why** line of
at most 40 words.

**Q1**

One complaint sentence, measured three ways (synthetic numbers):

  ------------------------------------------------------------------------
          **characters**    **UTF-8 bytes**     **tokens (Model M)**
  ------- ----------------- ------------------- --------------------------
  EN      292               292                 56

  RU      304               570                 92

  KK      333               629                 160
  ------------------------------------------------------------------------

Compute the KK÷EN ratio for characters, for bytes and for
tokens. **Number:** the token ratio divided by the byte ratio (2
decimals). **Why** (at most 40 words): what does this number say about
where the Kazakh premium comes from, the encoding or the tokenizer?

**Q2**

A teammate sets the chunk size to **512 tokens, counted with cl100k**.
The retrieval model (e5-small) reads at most 512 of *its own* tokens.
Assume e5 tokens grow in proportion to cl100k tokens at the rate
measured in Lecture 4.1 (a chunker told 256 cl100k tokens produced
chunks of 263 / 128 / 76 e5 tokens for EN / RU / KK), and that every
chunk is exactly the full size.

**Number:** the share of an English chunk, in percent (1 decimal), that
the embedding never sees. **Why** (at most 40 words): which of the three
languages are cut, and what happens to the cut part?

**Q3**

A language model gives these next-token scores for the prompt \"The
branch is closed on\":

  -----------------------------------------------------------------------
  **token**                               **score**
  --------------------------------------- -------------------------------
  Monday                                  4.9

  Friday                                  3.4

  Sunday                                  2.8

  holiday                                 2.4

  Easter                                  1.0
  -----------------------------------------------------------------------

Apply softmax at temperature T = 2. **Number:** the probability of the
top token, in percent (1 decimal). **Why** (at most 40 words): what did
changing T do to the ranking of the tokens, and to the set of tokens
that can be sampled?

**Q4**

Two models answer the same 100 factual questions (1 point per correct
answer on the public leaderboard):

  ------------------------------------------------------------------------
                  **correct**     **wrong**     **\"I don\'t know\"**
  --------------- --------------- ------------- --------------------------
  Model X         66              34            0

  Model Y         62              17            21
  ------------------------------------------------------------------------

Now score them again with a penalty: +1 per correct answer, −1 per wrong
answer, 0 for \"I don\'t know\". **Number:** Y\'s penalised score minus
X\'s (negative if X is ahead). **Why** (at most 40 words): which model
does the public leaderboard prefer, and what does that teach a model to
do?

**Q5**

A support chat adds **101 new tokens per turn** (customer message plus
assistant reply) and has **19 turns**. The model keeps no state, so the
full history is sent again on every turn (Lecture 1.2). As in the
Lecture 1.2 table (100, 200, 300, \... tokens billed on turns 1, 2, 3,
\...), turn *i* is billed for the tokens of all *i* turns so far,
including its own. **Number:** the total input tokens billed over the
whole conversation. **Why** (at most 40 words): how many times has the
first message been paid for by the last turn, and what does prompt
caching change?

  -------------------------------------------------------------------------
          **qid**     **lang**      **rank_A**          **rank_B**
  ------- ----------- ------------- ------------------- -------------------
  **0**   q01         EN            1                   3

  **1**   q02         KK            1                   1

  **2**   q03         RU            1                   1

  **3**   q04         KK            1                   3

  **4**   q05         KK            11                  7
  -------------------------------------------------------------------------

  -------------------------------------------------------------------------------
           **pipeline**   **lang**   **condition**   **n_docs**   **n_correct**
  -------- -------------- ---------- --------------- ------------ ---------------
  **0**    H              EN         clean           30           28

  **1**    H              EN         degraded        30           27

  **2**    H              RU         clean           30           29

  **3**    H              RU         degraded        30           28

  **4**    H              KK         clean           30           27

  **5**    H              KK         degraded        30           26

  **6**    L              EN         clean           30           28

  **7**    L              EN         degraded        30           25

  **8**    L              RU         clean           30           27

  **9**    L              RU         degraded        30           23

  **10**   L              KK         clean           30           22

  **11**   L              KK         degraded        30           22
  -------------------------------------------------------------------------------
