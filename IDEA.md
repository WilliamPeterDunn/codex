# Storytelling and translation platform

> **Status:** Working product brief for a rapid prototype. Product choices below are hypotheses to validate, not settled policy.

## Purpose

Help storytellers share stories in the media they choose, set clear attribution and reuse terms, and connect translations back to the original work. Readers can discover a story and experience its original or a translated version without losing that connection.

The project has two equally important outcomes:

1. Demonstrate rapid prototyping using LLM-assisted development.
2. Make the use of Codex tools clear and reviewable as part of the deliverable.

## Who it serves

- **Storytellers** who want to publish a story, retain attribution, and state how others may reuse it.
- **Readers** who want to find stories and read, listen to, or view them in a language they understand.
- **Translators** who need to contribute a version that remains linked to its source and credits.

## Core experience

### Storyteller workflow

1. Start a new story and add a title, creator name or pseudonym, description, language, and story format.
2. Add or link the story media (text, image, audio, or video) and provide descriptive text or a transcript where available.
3. Choose attribution and reuse terms, with plain-language explanations and a link to the full license or permission statement.
4. Preview the story page, then publish it.
5. Add a translation as a separate version linked to the source. Credit the translator and identify the translation's language and status (for example, community or machine-assisted, pending review, or reviewed).

### Reader workflow

1. Browse or search a library by title, creator, language, format, or topic.
2. Open a story page and see the creator, original language, format, attribution, and reuse terms before engaging with it.
3. Read or play the original, or switch to an available translation while keeping the relationship to the original visible.
4. Follow the source and translation credits and understand which version they are viewing.

## Prototype scope

Build a responsive web app that demonstrates the complete reader path and a lightweight publishing path with realistic sample content.

### Include

- A browsable story library with search and simple filters for language and format.
- A story detail page with a text, image, audio, or video presentation, creator attribution, license information, and related versions.
- A linked translation view with language, translator credit, and review status.
- A publish form covering story metadata, format, attribution, and reuse terms; publishing can use local/demo data until persistence is needed.
- A clear empty state and a small amount of sample content that demonstrates more than one medium and language.
- Responsive layouts, keyboard-accessible controls, and clear labels for media and language.

### Defer

- Accounts, uploads to durable cloud storage, moderation queues, payments, analytics, and production search infrastructure.
- Automated translation. It can be explored later, but must not silently replace a source or imply that generated translations have been reviewed.
- Complex regional policy or legal determinations. The prototype should present creator-selected terms plainly and avoid claiming that a checkbox resolves consent or ownership questions.

## Platform recommendation

Start as a **responsive web app**. Stories are shareable by link, and a browser experience works across phones and computers without app installation. Keep the first implementation small and easy to iterate; choose a TypeScript UI stack suited to the existing repository once the first screen and data model are ready. Use local/demo data initially so the core experience can be prototyped before committing to accounts, hosting, or a storage provider.

The first demo should use English UI and sample stories in at least two languages. Which region, language pair, and rights wording to prioritize should be validated with intended storytellers before treating them as product policy.

## Codex use as part of the deliverable

The implementation should make the LLM-assisted development process inspectable, not merely mention that Codex was used. Keep a concise `CODEX_LOG.md` with meaningful tasks, the Codex capabilities/tools used (for example, repository inspection, edits, command execution, and verification), key human decisions, and links to the resulting commits or files. Include useful evidence such as reviewed diffs and command output; do not record secrets or private user data. The log should help someone understand how the prototype was built and where human review shaped it.

## Open questions for validation

- Which first community, region, and language pair should the sample experience represent?
- Who may publish, and what consent or safeguarding steps are appropriate for stories involving other people or communities?
- Which reuse options are understandable and appropriate for the intended storytellers?
- Should translations be added by the original creator, a translator, or either with a review/approval step?
- What media types and file-size limits matter most in places with limited connectivity?
- What should be persistently stored in the first deployable version?
