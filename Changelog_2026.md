# CHANGELOG 2026
# Purpose: Human-readable record of structural changes to the Japanese Language Assistant workspace.
# Usage: Not required reading every session. Read when diagnosing issues, before major restructuring,
#        or when the user asks what has changed. Write to whenever files are created, deleted,
#        renamed, restructured, or have their format meaningfully changed.
#        Do NOT log routine content updates (e.g. adding a kanji, appending a session entry).
# At the end of each year, start a new file: Changelog_YYYY.md
# Last updated: 2026-04-30

---

[2026-04-30] Initial workspace setup

CREATED: Known_Kanji.md
  - Populated from user-provided kanji list
  - Format: flat text, categorised sections, one entry per line
  - Corrections applied: 同曜日 -> 土曜日; ニ (katakana) -> 二 (kanji)

CREATED: Session_Log.md
  - Format: dated entries with activity, result, notes, next fields

CREATED: New_Kanji_Tracker.md
  - Format: kanji | reading | meaning | first_introduced | familiarity | notes

CREATED: Grammar_Notes.md
  - Format: dated entries per grammar point with familiarity level
  - Includes N5 progression checklist for reference

CREATED: Practice_Sentence_Bank.md
  - Format: sentence / furigana / translation / status blocks, grouped by difficulty

CREATED: User_Profile.md
  - Format: sectioned flat text covering general info, strengths, weaknesses, notes

CREATED: Kana_Assessment_Checklist.md
  - Format: full hiragana and katakana reference with romanisations, grouped by row

CREATED: BOOTSTRAP.md
  - Format: stepwise session instructions with file map and tone notes

---

[2026-04-30] Post-setup revisions (same session)

MODIFIED: Known_Kanji.md
  - Added 水 (confirmed known by user)
  - Added [KANA] section (placeholder, populated after assessment)
  - [KANA] section updated after Session 1 assessment: hiragana and katakana confirmed strong

MODIFIED: Practice_Sentence_Bank.md
  - 水 added to Known_Kanji.md resolves kanji gap in sentence: 先生は水を飲む

MODIFIED: User_Profile.md
  - script_exposure updated after kana assessment

MODIFIED: Session_Log.md
  - Setup entry amended: kana ability note updated to reflect Session 1 outcome

MODIFIED: BOOTSTRAP.md
  - Tone notes: added explicit instruction that Claude has full autonomy over file management

CREATED: Changelog_2026.md (this file)

---

[2026-04-30] Session 2 — structural notes and instruction revision

MODIFIED: BOOTSTRAP.md
  - Removed instruction to take user corrections at face value
  - Added explicit skepticism policy: verify environmental and linguistic claims against disk
    before updating files; do not treat self-corrections as ground truth
  - Added note that tutor leads session structure by user preference

MODIFIED: User_Profile.md
  - Updated pace_preference from unknown to moderate-to-fast
  - Added strengths and weaknesses observed in Session 2
  - Added note that tutor leads sessions
  - Added skepticism note re: user self-corrections

NOTE: A project-level Known_Kanji file (separate from Known_Kanji.md on disk) was removed by
  the user during this session. Known_Kanji.md on disk is unaffected and remains authoritative.
  An error was made mid-session treating this as deletion of Known_Kanji.md — corrected before
  end of session.

---

[2026-04-30] Homework system added

CREATED: homework\ (directory)
CREATED: homework\shodo.json
  - Format: JSON array of kanji entries nominated for 書道 practice
  - Fields: kanji, kunyomi, onyomi, meaning, notes, nominated, completed
  - completed flag set by user when sufficiently studied

CREATED: homework\writing.json
  - Format: JSON array of writing exercises
  - Fields: id, type, prompt, hints, grammar_focus, assigned, completed, response
  - User transcribes pen-and-paper answer into response field; Claude reviews next session

CREATED: archive\ (directory)
CREATED: archive\shodo_2026.json
  - Flat archive of completed 書道 entries for 2026; append only
CREATED: archive\writing_2026.json
  - Flat archive of completed writing exercises for 2026; append only
  - Includes user responses for record

MODIFIED: BOOTSTRAP.md
  - Added shodo.json and writing.json to Step 1 reading list
  - Added homework review step to Step 2 (orient) and Step 3 (session open)
  - Added HOMEWORK section to Step 4 with assignment and archiving rules
  - Added homework note to Step 5 (end of session)
  - Added homework files to FILE MAP
  - Added HOMEWORK FILE FORMATS section with example entry schemas

---

[2026-04-30] Homework schema update — completed_at and confidence added

MODIFIED: homework\shodo.json
  - Added completed_at field (ISO date string or null) — set by Electron app at time of completion
  - Added confidence field (integer 1–5 or null) — user self-rating set via Electron app
  - All existing entries updated to include both fields; incomplete entries have null values
  - One entry (九) has confidence: 3 with completed: false — partial confidence recorded before completion

MODIFIED: BOOTSTRAP.md
  - shodo.json entry format updated to include completed_at and confidence
  - Field notes added explaining type and source of each new field

NOTE: An Electron app exists at D:\Claude\Japanese Language Assistant\app\ which the user uses
  for offline 書道 practice. It reads/writes homework\shodo.json directly. Claude should not
  assume it controls all writes to that file.

  Valid shodo.json entry states (completed / completed_at / confidence):
    1. false / null / null       — not started
    2. false / null / [1-5]      — INVALID (app now prevents this)
    3. true  / [date] / null     — completed, no confidence rated
    4. true  / [date] / 1        — completed, lowest confidence
    5. true  / [date] / 2-4     — completed, mid confidence
    6. true  / [date] / 5       — completed, highest confidence
  The 九 entry (confidence: 3, completed: false) is a dev artifact; treat confidence as null.

---

[2026-05-02] Migration to GitHub

MIGRATED: All environment files moved from local filesystem to public GitHub repository.
  Repo: omenjohn/Claude-Japanese-Language-Assistant
  Visibility: public (required for Claude's web_fetch tool to read raw content without auth)

MODIFIED: BOOTSTRAP.md
  - Added ENVIRONMENT section with read/write instructions for GitHub API
  - Reads: raw.githubusercontent.com (no auth required)
  - Writes: GitHub Contents API via PAT in system prompt
  - Added note that writes must go through the GitHub File Writer artifact
  - Updated Step 5 to reflect that file updates are provided as content for user to push
  - Removed references to local filesystem paths

CREATED: github_writer.html (not in repo — lives locally or as a Claude artifact)
  - Browser-based tool for pushing file updates to GitHub
  - Accepts PAT, file path, and content; handles SHA fetch and PUT automatically
  - Used at end of each session to commit changes Claude cannot write directly

NOTE: Claude's web_fetch tool cannot attach Authorization headers, so direct API writes
  are not possible from Claude. The artifact workaround is the current solution.
  If Anthropic adds header support to web_fetch in future, direct writes become possible.

NOTE: The Electron 書道 app (D:\Claude\Japanese Language Assistant\app\) still reads/writes
  homework\shodo.json locally. That file is not yet synced to GitHub automatically.
  Manual sync required when shodo.json changes via the app.