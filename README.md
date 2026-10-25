# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

I collected over 200 public posts from r/getdisciplined on Reddit, where people ask for help with, and share advice about, building habits, beating procrastination, and staying motivated, etc. Reading through them, I noticed several things I could point at: some posts describe a specific method or routine (a schedule, an app, a rule like "no phone in the bedroom") and some don't; some ask for help while others give advice; and some draw on the author's own experience while others offer generic motivation. My classifier uses these distinctions to sort each post into `actionable_advice`, `motivational_fluff`, and `help_request`, so it can separate posts a reader could act on from posts that only sound inspiring.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `actionable_advice`

**Definition:** The post's main purpose is to recommend something to others, and it gives a concrete method or step a reader could try today (a step, routine, tool, rule, or timeline).

**Example 1:** [I stopped relying on motivation and made journaling a three question appointment](https://www.reddit.com/r/getdisciplined/comments/1xb70pm/i_stopped_relying_on_motivation_and_made/)
>

**Example 2:** [[METHOD] How I went from rock bottom to disciplined in 6 months.](https://www.reddit.com/r/getdisciplined/comments/9t0gqz/method_how_i_went_from_rock_bottom_to_disciplined/)
>

### `motivational_fluff`

**Definition:** The post's main purpose is to encourage or share a mindset ("just start," "discipline is freedom"), with no specific method a reader could act on.

**Example 1:** [[Advice] Don't prove them right or wrong!](https://www.reddit.com/r/getdisciplined/comments/n8fq60/advice_dont_prove_them_right_or_wrong/)
>

**Example 2:** [Just 1 habit to change your life](https://www.reddit.com/r/getdisciplined/comments/1isg6mj/just_1_habit_to_change_your_life/)
>

### `help_request`

**Definition:** The post's main purpose is to ask for advice or support with the author's own struggle. Any past attempts it describes are background, not recommendations.

**Example 1:** [Tips for getting out of bed in the cold mornings?](https://www.reddit.com/r/getdisciplined/comments/1x9t5z9/tips_for_getting_out_of_bed_in_the_cold_mornings/)
>

**Example 2:** [Whenever I get stressed, my entire routine falls apart. How do I break this cycle?](https://www.reddit.com/r/getdisciplined/comments/1x8zg9n/whenever_i_get_stressed_my_entire_routine_falls/)
>

### The hardest boundary

The post [Just 1 habit to change your life](https://www.reddit.com/r/getdisciplined/comments/1isg6mj/just_1_habit_to_change_your_life/) was hard to decide a label for. 

**Which two labels to decide between:** `actionable_advice` and `motivational_fluff`

**The decision rule I used every time:** If the post names a concrete method that a reader could start without deciding what it means (what to do, and ideally when or how), it's actionable_advice. A general principle or attitude ("follow through," "be consistent," "just start") is motivational_fluff, even when phrased as a command and even if the post ends with a vague pointer to resources. The test I used was whether the advice could become a calendar entry or a checklist item. "Put your phone in another room from 9pm" can, and "do what you say you're going to do" can't.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** I copied 284 public posts from r/getdisciplined. I saved Reddit's top posts of the year, newest posts, and most argued-about posts of the year. I kept only the original post (title plus text). I skipped comments, posts with only a title, removed posts, and repeats.

**How I labelled them:** I labelled 20 posts first with no help, using only my own definitions. Those rows say `cold` in the note column. An AI tool labelled the other posts using my definitions. Those rows say `pre-labelled`. Rows where the AI was unsure say `pre-labelled, unsure`. I then read the AI label and fixed the ones I disagreed with.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `actionable_advice` | 86 | 30% |
| `motivational_fluff` | 77 | 27% |
| `help_request` | 121 | 43% |
| **Total** | 284 | 100% |

**Three hard cases**

**1.**
> *The post:*
>
> Discipline isn’t suffering
>
> Society convinced you that discipline means struggle, it’s not, putting your shoes on going to the gym is not struggling, reading isn’t struggling, learning isn’t struggling, it’s the same thing as you can be cold and wet and miserable or you can just be cold and wet, Discipline is just a term nowadays associated with all these negative emotions of struggle and suffer and punishment, It’s not, It’s simply a bridge to your desires, and it’s easy, The actual tasks are simple and mundane, It’s the psychology war you have inside your mind before, that is what makes it a suffering and a struggle, so eliminate it, I eliminated mine and I’ve been disciplined, 
>
> \- how do you eliminate it? Its a simple concept, For most people discipline is - Decision -> Psychological war -> the actual task, You skip the psychological war part, so it’s just Decision -> The actual task, Dive first thing into the task, you will realize it ain’t hard and it ain’t suffering and it’s actually easy and interesting once you are invested. The more you think about doing something, the harder it becomes, the more you actually do the thing, the mind calibrates itself to the action. Just control your hands and legs, the start the task, don’t listen to your mind, don’t ask for permission, Pretend it doesn’t exist. Thats how you do it. 
>
> \- Now Discipline that feels like punishment, how do you eliminate that? Instead of being 100% disciplined, let’s be 50% disciplined, so throughout your day if you have 6 hours of free time, you can do whatever you want for 3 hours, play video games, eat, watch youtube, etc. and for 3 hours you be productive,   
> For example, you come home at work at let’s say 6 pm, you have till 12 pm, that’s 6 hours. Here’s how I do it -   
> 6 pm - 7:30 pm - Chill  
> 7:30 pm - 9 pm - Productive  
> 9 pm - 10:30 pm - Eat and chill  
> 10:30 pm - 12 pm - productive
>
> Boom, that’s 3 hours of productivity right there, and I do this everyday. On weekends you can stretch it to 6-7 hours of productivity, It’s an easy system without restrictions. 
>
> Goodluck!

> *Could have been:* `motivational_fluff` or `actionable_advice`
>
> *I chose `actionable_advice`, because:* the second half gives a schedule that could go straight into a calendar. My rule says a real method wins, even if the rest of the post is just mindset.

**2.**
> *The post:*
>
> 90 Days No Sugar: My skin and energy finally came back.
>
> Three months ago I finally stopped my cycle of eating junk and feeling like sh\*t. I cut out all added sugar and processed foods cold turkey. My main focus was getting rid of acne and low energy levels that I knew I shouldn’t have. Honestly, the first week was rough. I literally had cold sweats for like 3 days. Pretty sure that was the zero caffeine. But every morning I felt noticeably better. My energy doesn't crash in the afternoon anymore and my skin has cleared up completely.
>
> I was not too far from going on Accutane, but it turns out my diet was the problem. Taking care of my body from the inside out changed my entire perspective. Now, I actually look forward to my simple nightly skincare routine because I’m proud of the progress I see. Staying disciplined with food made the biggest impact on my skin and life. I finally feel like a functional human.

> *Could have been:* `motivational_fluff` or `actionable_advice`
>
> *I chose `motivational_fluff`, because:* the post tells a personal story. It never tells the reader to do something. "Cut sugar" is just what the author did.

**3.**
> *The post:*
>
> What shape do your impulse buys take?
>
> I noticed most of my impulse buys have the same shape. It's late, I'm tired,
>
> I've been scrolling, and the thing is already in the cart before I've really
>
> decided anything. 
>
>
>
> What finally helped wasn't a budget or deleting my saved card. It was putting
>
> something in the gap between "add to cart" and "pay". Nothing big, about 30
>
> seconds:
>
>
>
> Say it plainly in my head: this is an urge to buy, not a need then put the phone face down and breathe out once, slowly. Often after closing the tab, the cart stays saved, nothing is lost.
>
> If I still want it tomorrow, I buy it. "Not now, it's still there" works way better than "never".
>
>
>
> Most of the time I don't even open the cart again the next day. Sometimes I
>
> do and I buy it, and that's fine too, at least it was a decision and not a
>
> reflex.
>
>
>
> It's only half a fix though. It does nothing against the "sale ends in 2
>
> hours" stuff, those countdowns still get me. 
>
>
>
> So I'm curious: what's your version of the pause? And how do you handle the
>
> fake-urgency sales, where waiting 24h is exactly what they're built to stop?

> *Could have been:* `help_request` or `actionable_advice`
>
> *I chose `help_request`, because:* the title and the end are both questions, and the author says the fix is "only half" working. The trick is what they tried, so it counts as background.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`, fine-tuned for 3-way classification
(`actionable_advice`, `motivational_fluff`, `help_request`).

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128 tokens,
seed 42. Trained on Apple Silicon GPU (MPS), torch 2.14.1.

**Anything I changed from the defaults, and why:** Nothing. All hyperparameters
are the notebook defaults.

**Split sizes:** 284 labelled posts, stratified 70/15/15 (seed 42):

| Split | Total | actionable_advice | motivational_fluff | help_request |
|---|---|---|---|---|
| Train | 198 | 60 | 53 | 85 |
| Validation | 43 | 13 | 12 | 18 |
| Test | 43 | 13 | 12 | 18 |

No label has fewer than 8 posts in test, but 12–18 per label is still small:
one post moves a label's recall by 6–8 points, so expect noticeable variance
across seeds.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
