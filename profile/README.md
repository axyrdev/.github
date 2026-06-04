<div align="center">

<a href="https://axyr.dev">
  <img src="https://axyr.dev/brand/axyr-github-banner.png" alt="Axyr — Ship AI code with proof." width="100%" />
</a>

<br/>
<br/>

**The AI Change Safety Layer** — your AI writes the code, Axyr proves whether it's safe to ship.

<br/>

[![Join the beta](https://img.shields.io/badge/Join_the_beta-22D07A?style=for-the-badge&labelColor=0B0D10)](https://axyr.dev)
&nbsp;
[![axyr.dev](https://img.shields.io/badge/axyr.dev-15181D?style=for-the-badge&logo=googlechrome&logoColor=22D07A&labelColor=0B0D10)](https://axyr.dev)
&nbsp;
[![@axyrdev](https://img.shields.io/badge/@axyrdev-15181D?style=for-the-badge&logo=x&logoColor=F2F4F7&labelColor=0B0D10)](https://x.com/axyrdev)

<sub>Built in Rust &nbsp;·&nbsp; Deterministic &nbsp;·&nbsp; Replayable to the byte &nbsp;·&nbsp; <strong>Proof, not vibes.</strong></sub>

</div>

<br/>

---

## The problem

AI now writes most of your code. **You don't read all of it.** The bug isn't in the line you reviewed — it's in the four hundred you accepted. A check that runs one line too late protects nothing, and a pattern matcher will still call it `SAFE`.

Axyr is the layer that sits on the change and gives a verdict you can **trust and replay** — built for the people shipping fast with AI.

<br/>

## What it does

Four engines, one frozen output contract. Every engine reasons about **data flow**, not blind pattern-matching.

| | Engine | Catches |
|---|---|---|
| 🛡️ | **Code Guard** | SQL injection · broken access control via *strict dominance* · IDOR — tracking taint through real execution paths. |
| 🗄️ | **Data Guard** | Destructive migrations — `DROP`, `TRUNCATE`, `DELETE` without `WHERE`, missing RLS. *Never let AI destroy your database.* |
| 🔑 | **Secret Vault** | Hardcoded provider keys (Stripe, AWS, GitHub, Google…), flow-sensitive — keys leaked into logs and responses too. |
| 📦 | **Deps Guard** | Malicious install scripts and insecure `http://` sources across npm, pip, and cargo. |

<br/>

## The verdict system

Every change gets one verdict — in monospace, replayable byte-for-byte.

| Verdict | Meaning |
|:--|:--|
| 🔴 `CRITICAL` | A security property is **provably broken**. The only state that blocks a merge. |
| 🟠 `WARNING` | Doubt with a real reason. Surfaced, never blocked — your call. |
| 🟢 `SAFE` | **Proven** to hold under the property. Not "looks fine" — proven. |
| ⚪ `UNKNOWN` | We can't prove it yet. Declared out loud — **never disguised as safe.** |

> We only block on **proof**. Doubt warns. Ignorance is declared — never hidden as `SAFE`.

<br/>

## The signature: strict dominance

Same code. Two lines swapped. One destroys you. A pattern matcher sees an `auth()` call in **both** files and calls them safe — because order is invisible to it.

**🟢 `SAFE` — guard *before* the action**

```ts
export async function DELETE(req, { params }) {
  const session = await auth();
  if (!session) return unauthorized();

  await prisma.invoice.delete({ where: { id: params.id } });
  return ok();
}
// delete is dominated by the auth check → proven safe
```

**🔴 `CRITICAL` — guard *after* the action**

```ts
export async function DELETE(req, { params }) {
  await prisma.invoice.delete({ where: { id: params.id } });

  const session = await auth(); // too late
  return ok({ user: session?.user });
}
// delete runs BEFORE any check — anyone deletes anything
```

A check that runs too late protects nothing. Pattern matchers say *"safe."* **Axyr doesn't.**

<br/>

## Why it's different

- **Change-oriented** — we judge the `diff`, not a frozen snapshot. The question is what *this commit* breaks.
- **Property-oriented** — we track the security property a commit violates: taint reaching a sink, a guard that no longer dominates.
- **Deterministic** — same facts in, same verdict out, replayable to the byte. No model lottery, no flaky runs.
- **Honest by design** — what we can't prove stays `UNKNOWN`. We declare our blind spots instead of hiding them as safe.

<br/>

## Status — building in public

A deterministic security certifier, written in **Rust**, in the open. No magic, no model that *"feels"* safe — just execution paths, dominance, and proofs you can replay.

```text
measured, not claimed
──────────────────────────────────────────────
golden cases passing       ·  115 / 115
engine test suite          ·  279 tests
mutation tests (property)  ·  22 / 22
silent false negatives     ·  0 on critical & high
blocking false positives   ·  0%   (gate: < 5%)
replay                     ·  2× byte-identical
```

> 🚧 **Private beta soon.** Join the waitlist → **[axyr.dev](https://axyr.dev)**

<br/>

<div align="center">
<sub>

**Axyr** &nbsp;·&nbsp; The AI Change Safety Layer &nbsp;·&nbsp; [axyr.dev](https://axyr.dev) &nbsp;·&nbsp; [@axyrdev](https://x.com/axyrdev)

</sub>
</div>
