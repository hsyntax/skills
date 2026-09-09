---
name: rename-pass
disable-model-invocation: true
description: Rename-pass over existing code — align identifiers to the vocabulary the artifact already commits to. Use when the user asks to rename or shorten identifiers, calls names noisy, or when identifiers contradict their own docs or schema.
---

# Rename Pass

A rename pass is **mechanical, not inventive**: every new name is drawn from vocabulary the artifact already commits to. If no established vocabulary exists to align with, stop and report — choosing new vocabulary is domain modeling, a decision for the user.

## 1. Establish the vocabulary

Read the artifact's prose before its code: header comments, the schema or migration it persists to, domain docs, and how sibling modules name equivalent things. The highest-value renames are vocabulary mismatches — code saying one word where the artifact's own docs and schema say another — and names restating what their scope already says (redundant module prefixes).

**Done when:** every candidate rename cites a vocabulary source. A name with no source is invention and stays out of the pass.

## 2. Map the blast radius

For every candidate, find its consumers: grep repo-wide (skip build output), and check what namespace each name lands in — a name scoped to its own module can stay short; a name exported into a flat shared namespace (e.g. a barrel export) needs a disambiguating prefix.

Classify every name **rename** or **keep**. Contract names are keeps: anything mirroring an external contract — DB columns, wire and serialized fields, string-literal tags, API routes, public method names consumers already call. Each keep carries its reason.

**Done when:** every name in the module is classified, each keep has a stated reason, and every consumer file of a renamed export is on the edit list.

## 3. Present the table, then apply

Present old → new as one table, then apply in the same turn — a coherent scheme surfaces what one-at-a-time renames hide: two old names mapping to one new name, or one vocabulary word used two ways. Apply mechanically:

- Exact whole-word patterns (`\b…\b`), longest identifier first, so substrings and plurals survive the pass (`NormalizedFooKey` before `FooKey`; `fooIds` before `fooId`).
- Unique identifiers rename across all files in one sweep. Locals that merely share a spelling rename one file at a time: the same spelling in another file is usually a different variable.
- Prose renames by hand. Comments use vocabulary words as English, so identifier patterns leave them stale — bring doc comments onto the new vocabulary alongside the code.

**Done when:** grep for each old identifier returns only contract keeps and prose that legitimately uses the word as English.

## 4. Verify and report

Run the static checks and tests of every touched module.

**Done when:** both pass, and the summary delivers the rename table, the keep list with reasons, and any judgment call the user may want reversed (e.g. a field kept to mirror a column name).
