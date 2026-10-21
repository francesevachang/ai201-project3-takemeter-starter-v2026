# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.

**Overall accuracy is at least 0.70 on the held-out set, in each of the three seeds.**

**Why this target:** Chance with three labels is about 0.33, and always guessing my biggest label scores that label's share of the data (I'll fill in the number once I've counted), so 0.70 should be a clear step up from both. I don't ask for more because `actionable_advice` vs `motivational_fluff` is fuzzy and 200 posts is little to learn it from.

---

## 2.

**Every label has an F1 of at least 0.60 on the held-out set, in all three seeds, except `help_request`, which must reach 0.75.**

**Why this target:** `help_request` should be the easiest label (questions about the author's own struggle), so it gets a higher bar. 0.60 on the blurry advice/fluff pair is a real target. My smallest label might have few test posts, so its F1 will swing between seeds and I didn't go tighter.

---

## 3.

**No label is under 20% of my 200 posts (at least 40 examples), and the largest label is at most 2.5 times the size of the smallest.**

**Why this target:** Natural sampling of r/getdisciplined will likely leave `help_request` short. Under 40 examples its test F1 would be too noisy to judge criterion 2. I allow 2.5:1 because forcing perfect balance would misrepresent the community.

---

## 4.

**A second person who labels my held-out test posts using only my written definitions agrees with my labels on at least 80% of them.**

**Why this target:** The test labels are what the model gets graded against, so if someone reading only my definitions can't match me at 80%, my advice/fluff line is too fuzzy and the model's score is partly label noise. Not 90%, because the test split is small (about 30 to 40 posts) and borderline posts will split.

---

## 5.

**Predictions with a top-class probability of at least 0.80 are correct at least 85% of the time, and there are at least 10 such predictions in the test set.**

**Why this target:** A confident prediction should beat the 0.70 overall bar by a clear margin, so high-confidence ones can be trusted and the rest reviewed. The minimum of 10 (my test split is only about 30 to 40 posts) stops a rarely-confident model passing on a few lucky cases.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
