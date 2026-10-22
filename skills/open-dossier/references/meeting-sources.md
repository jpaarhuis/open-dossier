# Ingesting meetings into a dossier

A meeting transcript is a first-class source type. The ingest pipeline in SKILL.md applies unchanged; this file covers what is specific to conversations: the transcript format, what a meeting analysis must hold, how actions and decisions point back to the moment they were said, and the contract for tools that record meetings and hand transcripts to a dossier. Read it when the user asks to ingest a transcript, meeting notes with speakers, or a recording's text, and when a backlog row says a transcript is waiting.

## Converting a transcript

One meeting becomes one source with the next S-ID. Transcripts arrive as a recorder's markdown, a Teams/Zoom export (.vtt, .docx), or pasted text. Convert any of them to this form in `sources/converted/YYYY-MM-DD-short-slug.md` (date = the meeting date):

```markdown
# Meeting: [title]

- Date: [YYYY-MM-DD], [HH:MM]–[HH:MM] ([n] min)
- Heard: [names, with organization where known]
- Invited, not heard: [names]   <!-- only when known -->
- Recorded by: [person or tool]; automatic transcription, names and terms may be misheard

---

[14:08:12] **Karel Korst:** De offerte stuur ik vrijdag.
[14:08:30] **Speaker 2:** Prima.
```

- **One line per utterance**, `[HH:MM:SS] **Speaker:** text`. Keep the words as transcribed; fix nothing silently. When a name or term is clearly misheard and you are sure of the right one (the attendee list, the dossier's actors), write the correction in the analysis, not in the transcript, and add the misheard form to the term's "Heard as" cell in `terms.md`.
- **Unnamed speakers** ("Speaker 2", "Spreker ?") stay unnamed. Never attribute what they said to a person by guessing; at most note "probably Karel ([INTERPRETATION])" in the analysis.
- **Original**: the file as delivered goes to `sources/originals/` unchanged. If it already has the form above, the converted file is a verbatim copy with a one-line provenance comment: copy the file (`cp`), then add the comment line. Do not write the transcript out again. If only the header differs, copy it and edit just the header.

## The meeting analysis

Use the analysis template from templates.md, with two sections first, before Key points:

```markdown
## Summary

[Two to four sentences: what the meeting was about and what came out of it. Concrete, with names.]

## State of play

- **[Subject]** — decided: [one sentence on where it stands] (@14:08, @14:12)
- **[Subject]** — agreed: [who does what] (@14:20)
- **[Subject]** — open: [what is explicitly not settled] (@14:31)
- **[Subject]** — discussed: [what was said, without an outcome] (@14:40)
```

- **Summary**: at most four sentences, one paragraph. A long meeting gets no longer summary; the detail goes into State of play.
- **State of play**: at most six subjects, in the order of the meeting; merge side tracks. Each subject gets exactly one status: `decided` (a choice was made), `agreed` (someone will do something), `open` (explicitly said not to be settled) or `discussed` (talked about, no outcome). Give the times of the one to three utterances that show it. Map the four words once in DOSSIER.md for a non-English dossier (Dutch: besloten, afgesproken, nog open, besproken).
- **Small talk** (the weekend, holidays, the weather, someone's family) is not dossier content: leave it out of the analysis and the wiki. Personal context about people belongs in whatever people notes the user keeps elsewhere, if anywhere.
- Then Key points, Decisions and hypotheses found, Risks and open questions and Follows from this source, as for any source.

## Labels and citations

A meeting is conversation, so label as for mail:

- What someone said is `[SOURCE]` for the fact *that they said it*, cited with its time: `(S014 @14:08)`. Whether the claim itself is true may still be a `[HYPOTHESIS]`.
- **Actions**: a commitment ("I'll send the quote on Friday") or something agreed together ("then we plan a follow-up next week") becomes an action with origin `S014 @14:08`, the owner who committed (a named speaker; an unnamed one leaves the owner open), and the due date resolved against the meeting date. A proposal, idea or wish ("we could also…", "we should look at that some time") is not an action.
- **Owners come from what was said.** An owner or co-owner is someone the transcript shows committing or being given the task. Never add someone because they were invited ("Invited, not heard"), because their role fits, or because they usually own such work. When it is unclear who, the dossier's owner alone or an open owner, and the question in the analysis.
- **Owners by full name.** Write every owner with first name and surname, as the transcript or the actors in `DOSSIER.md` spell them: "Mathijs van der Ven + Jeffrey", not "Mathijs + Jeffrey". A recorder or reader that links owners to people in a later meeting cannot tell two people with the same first name apart. Only the dossier's owner may be written by first name.
- **Decisions**: a choice made in the meeting is logged in `decisions.md` with the meeting date and `S014 @14:31` as origin. A suggestion that was not settled is a hypothesis or an open question, not a decision.
- **Contradictions** between what was said and what the dossier holds are handled as for any source: record both with their IDs (and times), flag in `backlog.md`.

## Tools that deliver transcripts

A recorder (an app that transcribes meetings) and the dossier meet in a fixed place, so any tool can feed any dossier and the dossier works the same without that tool. The contract:

1. **Deliver**: the tool writes the transcript into `sources/originals/` as `YYYY-MM-DD [title] (transcript).md` (add `HHMM` after the date when that name exists), in the converted form above, optionally with a first-line comment naming the tool and its own id for the meeting (`<!-- recorder: [tool], meeting: [id] -->`). It adds one row to "Sources to process" in `backlog.md` with status `to ingest`. It changes nothing else.
2. **Ingest**: a session (started by the user, or by the tool right after the meeting, e.g. `claude -p "/open-dossier ingest sources/originals/[file]"`) runs the normal ingest. In a git dossier, one ingest is one commit, so an unwanted ingest is one revert.
3. **Read back**: the tool finds the result through the source register in `index.md` (the row whose original is its file) and reads the analysis: Summary and State of play for a quick view, `actions.md` and `decisions.md` rows with that source ID for the rest. Tools only read these files.
4. **Retract**: before the ingest (the file is not in the register yet), the tool may remove its own original and backlog row again. After the ingest it may not: removing a processed source is the user's decision, through a session.

Before a meeting the tool may read `terms.md` and give its speech-to-text model the terms as a hint, those with a "Heard as" first: they were transcribed wrong before. It only reads the file; the ingest keeps it up to date.

## Recognizing which dossier a meeting belongs to

Tools that route meetings read the `## Recognition` section of `DOSSIER.md` (template in templates.md): the e-mail domains of the organizations in the dossier, the recurring meetings by their subject, and words that only this dossier uses. A person who works across many dossiers (a colleague) is never enough on their own. Fill the section at init when the user names the organizations; `lint` proposes additions from the sources (a domain seen in mail headers, a meeting series that keeps recurring).
