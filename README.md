# The Unofficial Guide
Md Rashedul Islam — Corpus: `campus_life

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

     The Unofficial Guide searches the campus_life collection of 88 documents to answer questions about campus rules and student experiences. It keeps each document as one chunk, creates embeddings locally, and retrieves relevant documents with Chroma. Gemini generates answers from the retrieved text and names the source files. A relevance gate refuses questions when the closest document’s distance exceeds 0.6.

## Chunking Strategy

**Chunk size:** One complete document per chunk; currently 178–549 characters.
**Overlap:** 0 characters.

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

     I use one complete document per chunk for the campus_life collection. The starter run produced 88 chunks from 88 documents, ranging from 178 to 549 characters. The documents I inspected describe short campus rules, and splitting them could separate important contrasts, such as work-study versus other campus jobs.
     My chunk size is the length of each document rather than a fixed character limit. I use zero overlap because each document stays intact. This strategy is designed for this short-document collection; I would reconsider it for longer documents containing multiple topics.

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `` — produced by: **Chunk 1** ``

```
======================================================================
Chunk 1  |  source: admin_add_drop_deadline.txt#0  |  produced by: chunker.py::split_documents
======================================================================
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.

```

**Chunk 2** — source: `` — produced by: **Chunk 2** ``

```
======================================================================
Chunk 2  |  source: course_biol_160.txt#0  |  produced by: chunker.py::split_documents
======================================================================
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.

```

**Chunk 3** — source: `` — produced by: **Chunk31**``

```
======================================================================
Chunk 3  |  source: course_hist_118_workload.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.

```

**Chunk 4** — source: `` — produced by: **Chunk 4** ``

```
======================================================================
Chunk 4  |  source: dining_pellew_dining_hall_followup.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.

```

**Chunk 5** — source: `` — produced by:  **Chunk 5** ``

```
======================================================================
Chunk 5  |  source: housing_innisfree_hall.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.

```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** What determines housing lottery priority for juniors and seniors before random tie-breaking?

**Answer:**

```
For juniors and seniors, housing lottery priority is ordered by accumulated credit hours before any random tie-breaking.
Source: admin_housing_lottery.txt
```

**My relevance cutoff:** 0.6

My five in-corpus questions had best distances from 0.2000 to 0.4938. The five out-of-scope questions ranged from 0.8246 to 0.9340. I kept the starting cutoff of 0.6 because it lies inside the observed gap: it allows all five campus questions through and rejects all five unrelated questions.

A cutoff below 0.4938 could reject my answerable adviser question. A cutoff above 0.8246 could allow the unrelated Mongolia question through. These ten examples support my choice, but do not guarantee correct decisions for every future question.

| Question | In corpus? | Best distance |
|---|---|---|
| What appears on my transcript if I drop a course after week two but before the end of week six? | Yes | 0.2477 |
| Which type of campus-job earnings does not count against financial aid the way ordinary income does? | Yes | 0.2000 |
| What kind of adviser does declaring a major assign me? | Yes | 0.4938 |
| In which month do unused dining dollars disappear? | Yes | 0.2675 |
| What determines housing lottery priority for juniors and seniors before random tie-breaking? | Yes | 0.2087 |
| What is the capital of Mongolia? | No | 0.8246 |
| How do I change the oil in a diesel engine? | No | 0.9340 |
| Who won the 1994 World Cup? | No | 0.8859 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.8442 |
| How do I write a for loop in Rust? | No | 0.8960 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.** I shared five campus documents with Codex and asked for help creating test questions and acceptance criteria. It suggested questions with expected answer phrases and drafted criteria with explanations. Through follow-up requests, I refined the chunk-quality criterion to specify which five chunks I would inspect and added a financial-aid example. I used AI-generated wording in both questions.py and criteria.md


**2.** I shared the starter chunker and asked how to adapt it. Codex suggested keeping each short campus document intact and supplied a replacement split_documents function. I replaced the fallback call with that code, saved it, and rebuilt the index. The result remained 88 chunks, so I documented this as a deliberate whole-document strategy rather than claiming that it improved retrieval. I also used Codex to help interpret my measured distances and retained the 0.6 cutoff because it separated my ten example questions.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

Baseline evidence: [Full baseline run](results/run_2026-09-27_1956_before.md), produced by `run_eval.py::main`, using `store.py::search` and chunks from `chunker.py::split_documents`.

Settings: `campus_life`, `TOP_K = 5`, `THRESHOLD = 0.6`. Each of the five in-corpus questions was answered three times with caching off. The judgments below are manual; the blank scorer columns in the saved log are not failures.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunks contain the answer | At least 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 in-corpus answers | 5 of 5 | 5 of 5 | 5 of 5 | MET for in-corpus answers |
| 3. Gate stops out-of-corpus questions | At least 4 of 5 | 5 of 5* | 5 of 5* | 5 of 5* | MET |
| 4. Chunks preserve complete explanations | At least 4 of 5 sampled chunks | 5 of 5** | Same sample** | Same sample** | MET under the boundary-preservation interpretation below |
| 5. Cited sources support the answers | At least 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |

*Criterion 3 was measured in one deterministic pass by `run_eval.py::check_out_of_scope`; the same result is repeated as directed by the template. These are not three separate gate tests. The earlier `app.py ask` check for the Mongolia question also returned the required refusal text.

**Criterion 4 was inspected once using the five unchanged README samples, also saved in [chunks_before.txt](results/chunks_before.txt). I checked for sentences cut by chunk boundaries and for retained conditions or contrasts. Original headings and sentence fragments remain in the source documents. This is a source-preservation judgment, not a claim that every source sentence is grammatically complete, and it is not three independent inspections.

Criterion 2 is scored here on the five in-corpus answers in each run. The out-of-corpus refusal has no citation. The original phrase “every answer” is ambiguous about refusals, so this scope must remain explicit rather than claiming that every system response includes a source.

### Actual evidence

**Criterion 1 — retrieval and chunk contents.** The baseline log records this retrieval for the transcript question in run 1:

```text
- Best distance: 0.2477 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_grade_appeals.txt, admin_pass_fail_option.txt, admin_transcript_requests.txt, admin_withdrawal_deadline.txt
```

The matching chunk, produced by `chunker.py::split_documents` and displayed by `app.py chunks -n 5`, contains:

```text
You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

The other four questions retrieved their corresponding financial-aid, declaring-a-major, dining-dollars, and housing-lottery documents in all three runs.

**Criteria 2 and 5 — source naming and supported answers.** These are actual run-1 answers recorded by `run_eval.py::main` in the baseline log:

```text
If you drop a course after week two, it shows as a "W" on your transcript (admin_add_drop_deadline.txt).
```

```text
Work-study earnings do not count against your financial aid the way ordinary income does (admin_campus_jobs_and_financial_aid.txt).
```

```text
Declaring a major assigns you a departmental adviser.

Source: admin_declaring_a_major.txt
```

```text
Unused dining dollars disappear in May.

Source: admin_dining_dollars.txt
```

```text
For juniors and seniors, housing lottery priority is determined by accumulated credit hours before any random tie-breaking.

Source: `admin_housing_lottery.txt`
```

The full log preserves runs 2 and 3 as well. All 15 answers address their questions, name a source, and match the campus facts being tested.

**Criterion 3 — gate.** The baseline log, produced by `run_eval.py::check_out_of_scope`, states:

```text
Produced by `run_eval.py::check_out_of_scope`, cutoff 0.6. Refused 5 of 5.
```

The five best distances were 0.825, 0.934, 0.886, 0.844, and 0.896, all above 0.6. The separate, previously observed `app.py ask` output for “What is the capital of Mongolia?” was:

```text
I don't have enough information about that.
```

**Criterion 4 — preserved contrast.** The transcript chunk above retains both the week-six drop window and the week-two boundary for a W. Another actual sample from `housing_innisfree_hall.txt#0`, produced by `chunker.py::split_documents`, preserves both statements:

```text
The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.
```

All five complete samples remain in the Unit 1 Sample Chunks section and in `results/chunks_before.txt`.

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | All five questions retrieved an answer-containing document in every run, exceeding the 4-of-5 target. |
| 2 | Every answer names a source | MET for in-corpus answers | All 15 in-corpus answers name a source. Refusals are excluded from this count; the original wording needs that scope clarified. |
| 3 | Gate stops out-of-corpus questions | MET | All five out-of-scope questions were refused in the deterministic gate check. The separate Mongolia check displayed the required refusal message. |
| 4 | Chunks preserve complete explanations | MET under stated interpretation | One inspection found all five samples retained their full source text and contrasts without cutting sentences. This does not assert grammatical completeness of original fragments. |
| 5 | Cited sources support the answers | MET | All five answers in each of the three runs address the question and state facts supported by their cited documents; none refuses an in-corpus question. |



## Diagnoses

Under the scoring interpretations documented above, no baseline criteria were missed. All five test questions retrieved an answer-containing source, and all three answer runs named sources and stayed supported by those sources. The relevance gate refused all five out-of-scope questions.

I inspected the five README sample chunks once. All five preserve complete source documents, including their conditions and contrasts, with no sentence cut off by the chunker. Some documents contain original sentence fragments, so this result measures preservation of source text rather than grammatical quality.

These results do not establish that the system works equally well on harder questions. My five questions each ask for a fact stated directly in one short document, and my out-of-scope questions are clearly unrelated to campus life. For a future evaluation, I would consider tightening the retrieval target from 4/5 to 5/5 and adding questions that require combining information across documents. I am keeping the original targets for this comparison.

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:** I reduced TOP_K in config.py from 5 to 3.

**Why I picked it:** All five baseline questions retrieved their answer-containing document first. Some additional retrieved documents were unrelated to the specific answer. I am testing whether retrieving three chunks preserves answer quality while reducing the context sent to the model. I kept the questions, chunking strategy, model, and relevance cutoff unchanged.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
