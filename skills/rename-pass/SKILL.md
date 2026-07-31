---
name: rename-pass
description: Rename-pass over existing code — align identifiers to the vocabulary the artifact already commits to and strip redundant module prefixes. Use when the user asks to rename or shorten types/identifiers, calls names noisy or hard to read, or when identifiers contradict the vocabulary of their own docs or schema.
---

# Rename Pass

A rename pass is **mechanical, not inventive**: every new name is drawn from vocabulary the artifact already commits to. If no established vocabulary exists to align with, this is domain modeling, not a rename pass — switch to the domain-modeling skill.

## 1. Establish the vocabulary

Read the artifact's prose before its code: file header comments, the schema or migration it persists to, domain docs, and how sibling modules name equivalent things (service suffixes, record types, prefix conventions). The highest-value renames are usually vocabulary mismatches — code saying one word where the artifact's own docs and schema say another.

**Done when:** every candidate rename cites a vocabulary source. A name with no source is invention and stays out of the pass.

## 2. Map the blast radius

For every rename candidate, find its consumers: grep imports repo-wide (skip build output), and check the export mechanism — per-module subpath exports scope names so short ones can't collide; a flat barrel export means short generic names need a disambiguating prefix.

Classify every name **rename** or **keep**. Contract names are keeps: anything mirroring an external contract — DB columns, serialized and wire fields, string-literal tags, API routes, public method names consumers already call. Each keep carries its reason.

**Done when:** every name in the module is classified, each keep has a stated reason, and every consumer file of a renamed export is on the edit list.

## 3. Present the table, then apply

Present old → new as one table before editing — a coherent scheme surfaces what one-at-a-time renames hide: two old names mapping to one new name, or one vocabulary word used two ways. Then apply mechanically:

- Exact whole-word patterns (`\b…\b`), longest identifier first, so substrings survive the pass (`NormalizedFooKey` before `FooKey`; `queueIds` must outlive a `queueId` rename).
- Unique identifiers rename across all files in one sweep. Generic patterns — bare local-variable spellings like `const queue =` — rename one file at a time: the same spelling in another file is usually a different variable.
- Prose renames by hand. Comments use vocabulary words as English, so identifier patterns leave them stale — bring doc comments onto the new vocabulary alongside the code.

**Done when:** grep for each old identifier returns only contract keeps and prose that legitimately uses the word as English.

## 4. Verify and report

Typecheck and run the tests of every touched package.

**Done when:** both pass, and the summary delivers the rename table, the keep list with reasons, and any judgment call the user may want reversed (e.g. a field kept to mirror a column name).
