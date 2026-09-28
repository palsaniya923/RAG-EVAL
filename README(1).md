# RAG System — [Project Name]

A retrieval-augmented generation pipeline built with a simple-first approach:
core functionality first, additional layers added only when a real problem
justified them.

---

## Architecture (Current)

```
USER QUERY
    ↓
RETRIEVAL (Vector search, metadata filter)
    ↓
CONTEXT ASSEMBLY (top-k chunks, simple dedup)
    ↓
LLM (Reason + Generate with citations)
    ↓
FINAL RESPONSE
```

**Why this core, and nothing more (yet):** every additional layer (reranking,
query rewriting, full eval suites, observability stacks) adds latency, cost,
and a new failure point. Each one below was added — or deliberately *not*
added — for a specific, stated reason.

---

## Decision Log

Every layer in this system exists because of a measured problem, not because
it's a "best practice." This log is the most important part of this repo —
it's what separates informed engineering from tutorial-copying.

| Layer | Status | Reason |
|---|---|---|
| Vector retrieval | ✅ Added (core) | Baseline — needed from day one to answer any query |
| Query Rewrite/Expansion | ⏳ Not yet | Only needed if users send vague/short queries and retrieval starts missing |
| Hybrid BM25 + Vector | ⏳ Not yet | Only needed if keyword-heavy queries (names, IDs, codes) start missing |
| Reranker | ⏳ Not yet | Adds latency + cost; only justified if retrieval precision is measurably low |
| Grounding/Claim Verification | ⏳ Not yet | Doubles LLM calls; only justified if hallucination becomes an observed problem |
| Full Eval Suite (RAGAS/Judge/Gold) | ⏳ Not yet | Manual spot-checks are enough pre-launch; automate once regression tracking matters |
| Adversarial/Security Tests | ⏳ Not yet | Becomes mandatory once real users or sensitive data are involved |
| Full Observability Stack | ⏳ Not yet | Simple logging is enough at low scale; upgrade when multiple users/scale hits |

*(Update this table as the project evolves — mark ✅ with a date and the
metric that justified the addition, e.g. "Added reranker on 2025-01 — retrieval
precision was 60%, jumped to 85% after.")*

---

## Enterprise Scale-Up Plan

If this system had to run inside a larger company — more users, compliance
requirements, an existing production system already in place — here's what
would change and why. This section exists to show scale-awareness even
though the current build stays intentionally simple.

| Trigger | What Gets Added | Why |
|---|---|---|
| Company already has a production RAG with known issues | Jump straight to reranker + eval pipeline | No need to re-prove the basic version works — that's already been done |
| Healthcare / finance / regulated domain | Grounding/Verification + Security Tests become mandatory | Compliance requirements override the "add only when needed" default |
| Multiple concurrent users / scale | Full observability (traces, latency, cost dashboards) | Simple logging stops being enough to debug issues across many users |
| Existing team/architecture already in place | Adapt to their stack instead of redesigning | Architecture decisions inside a company are collaborative, not solo |

**Key principle carried into any company context:** read the company's actual
need first — don't build everything "because it's best practice." Understand
the existing system before proposing changes.

---

## How to Use This Repo

1. `[setup instructions — fill in]`
2. `[how to run — fill in]`
3. `[how to test — fill in]`

---

## Roadmap

- [ ] Add reranker once retrieval precision is measured and found lacking
- [ ] Add eval pipeline once regression tracking becomes necessary
- [ ] Document each future addition in the Decision Log above with before/after metrics
