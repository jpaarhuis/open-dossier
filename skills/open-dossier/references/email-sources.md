# Ingesting email into a dossier

Email is a first-class source type. The ingest pipeline in SKILL.md applies unchanged; this file covers only what is specific to mail. Read it when the user asks to ingest mail, a mail thread, or attachments from mail.

## Acquiring the mail

Use whatever mail access the session has, in this order of preference:

1. **MCP tools** (e.g. an Outlook or Gmail MCP server): search for the message, fetch the full body, and save attachments. Check the available tools before telling the user mail access is missing.
2. **A local mail CLI** the user points you to.
3. **User-provided export** (.msg/.eml/.pdf dropped in an inbox folder): treat like any document.

If the mail tooling reads a locally cached mailbox (COM-based Outlook tools do), data can be stale after long idle periods. When the newest results look older than expected, trigger the tool's sync operation or tell the user, rather than concluding the mail does not exist.

## Converting mail to a source

One mail (or one coherent thread) becomes one source with the next S-ID.

- **Converted file**: `sources/converted/YYYY-MM-DD-short-slug.md` (date = the mail's sent date). Start with a header block, then the body verbatim:

```markdown
# Mail: [subject]

- From: [sender] · To: [recipients] · Cc: [cc]
- Sent: [YYYY-MM-DD HH:MM]
- Thread: [n messages, date range]   <!-- only for threads -->
- Attachments: [names, or none]

---

[body text]
```

- **Threads**: include every message in the thread once, oldest first, separated by `---` with a per-message From/Sent line. Strip repeated quoted history (the "On ... wrote:" pyramids): each message appears once, not re-quoted in every reply. Note the stripping in the provenance comment.
- **Original**: save the rawest form available (.msg/.eml if the tooling can produce it; otherwise the converted markdown is canonical, note that in a provenance comment).
- **Signatures, banners, disclaimers**: keep the first occurrence of a person's signature (contact data has value), drop repeated legal footers and "external sender" banners; note the removal.

## Attachments

Attachments are usually the real payload. Save them to `sources/originals/` alongside the mail. Then decide per attachment:

- **Substantive documents** (reports, decks, contracts, spreadsheets): register each as its **own source** with its own S-ID and full pipeline; the mail's analysis links to them ("carried A" / "carried by S0XX").
- **Trivial attachments** (inline images, logos, calendar .ics for a meeting already in the dossier): keep the file, mention it in the mail's converted header, no separate S-ID.

## Analysis specifics

Mail is conversation, so label with extra care:

- A statement in a mail is `[SOURCE]` for the fact *that the person wrote it*; whether the claim itself is true may still be a `[HYPOTHESIS]`.
- Commitments and agreements in mail ("I'll deliver X by Friday", "agreed, go ahead") are decisions or actions: log them with the mail's date and the S-ID as origin.
- Mark private or sensitive passages `(sensitive)` per the conventions in SKILL.md; mail contains them more often than formal documents.
