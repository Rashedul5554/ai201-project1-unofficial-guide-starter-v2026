# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
<!-- e.g. "One of my questions is about a topic only two documents mention, so
     I expect that one to be hard." -->

Each of my five questions has an answer in a specific, short campus document, so I expect retrieval to find the relevant information in most cases. Similar administrative topics may compete in search results. Requiring four of five allows one retrieval miss to investigate, while a lower target would accept too many missed answers that are present in the collection.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
<!-- Why all five and not four? What about your setup makes that achievable —
     or what would have to go wrong for it not to be? -->

Every document has a source filename that the system can use to identify where its answer came from. Students need to verify details about deadlines, financial aid, and housing, so every generated answer should name a source. Allowing even one answer without a source would leave that answer difficult to check.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**
<!-- What did your distances look like when you set the cutoff in Milestone 4?
     Was there a clean gap, or did the two groups overlap? -->

My collection focuses on campus life, while the five out-of-scope questions ask about unrelated subjects such as engine maintenance and programming. The gate should reject these instead of allowing unsupported answers. Requiring four of five refusals allows one possible retrieval mismatch to investigate, while a lower target would permit too many answers outside the collection. I have not measured the distance scores yet; I will examine them when choosing the cutoff in Milestone 4.

---

## 4. Chunks preserve complete explanations

<!-- YOU WRITE THIS ONE.

     How would you know if your chunks were the right size? Name something
     countable or observable.

     Examples of the right shape — don't copy these, they should come from
     what you actually saw in Milestone 3:
       - "At least 4 of 5 sampled chunks read as a complete thought, with no
          sentence cut in half at either end."
       - "No chunk is shorter than 200 characters, since anything below that
          in my corpus turned out to be a heading with no content under it." -->


I will inspect the five chunks included in my README’s Sample Chunks section. At least 4 of 5 must contain complete body-text sentences, with no sentence cut off at either boundary. Document titles and headings do not count as incomplete sentences.. If a chunk includes a rule that has an exception or contrast in the same source paragraph, it must also include that exception or contrast.

**Why this target:**

The campus documents I read are short, but separating related statements could change their meaning. For example, the financial-aid document explains both that work-study earnings do not count against aid the way ordinary income does and that non-work-study campus earnings do count. A chunk describing this comparison should preserve both statements.

I chose four out of five because these short documents should usually fit complete explanations into a chunk, while allowing one problematic split to investigate and improve.


---

## 5. Cited sources support the answers

<!-- YOU WRITE THIS ONE TOO.

     Pick something you actually care about getting right. It could be about
     speed, about refusals, about a particular kind of question your corpus
     handles badly, about source attribution being correct rather than merely
     present — anything, as long as it names a number or an observable
     outcome. -->

For at least 4 of my 5 test questions, the answer directly addresses the question, and every factual claim is supported by the document or documents it cites. I will check each answer against its cited sources. A refusal to answer one of these in-scope questions counts as a failure.


**Why this target:**

My questions concern specific campus rules where small distinctions matter. For example, an answer about housing priority must distinguish the random numbers given to rising sophomores from the credit-hour priority used for juniors and seniors. Merely naming the housing document does not prove the answer represents it correctly.

I chose four out of five because each question has a clear answer in the documents I read. This requires consistent, supported answers while allowing one failure to investigate and improve.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
