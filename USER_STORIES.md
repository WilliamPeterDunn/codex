# User stories

These stories turn the v1 product brief and architecture into reviewable, user-focused slices. They describe the planned self-hosted prototype; they do not imply that the features are implemented. Acceptance criteria are intended to guide design and later tests.

## 1. Find stories by title, creator, language, or format

**As a** reader, **I want** to browse and filter published stories by title, creator, language, or format, **so that** I can find a story I’m interested in and can access.

**Value:** Readers can discover relevant stories without an account, including stories in a language or medium that suits them.

**Acceptance criteria**

- A reader can browse published stories without signing in.
- A search term matches story title, creator display name, or description.
- Language and format filters can be applied individually or together.
- Search and filter selections are represented in the URL, so the reader can refresh or share the filtered view.
- Results show the title, creator, language, and format.
- Only published versions appear in public results. Drafts and unpublished versions do not appear in results or counts.
- A clear empty state appears when there are no matches, and the reader can clear filters to return to the library.

**Scope and edge cases**

- V1 supports `q`, `language`, and `format` filters. Topic filtering is not included in the planned routes or search fields.
- Search uses SQLite over title, creator, description, language, and format. Unsupported filter values are safely ignored or reset.
- Reads are dynamic so a publish or unpublish change is reflected on the next request. Private usernames and email addresses never appear in results.

## 2. Read or play a story and switch between its versions

**As a** reader, **I want** to read, view, or play a story and switch between its available language versions, **so that** I can engage with the work in a language I understand while seeing its connection to the source.

**Value:** Readers can access stories across media and languages while retaining clear attribution and translation context.

**Acceptance criteria**

- A public story page shows its title, creator’s public name, version language and format, attribution, and reuse terms.
- The selected version is presented in its supported medium: text, image, audio, video, or PDF.
- Available related versions are listed by language. Selecting one opens that version while keeping its source relationship visible.
- A translated version identifies its translator and review status, such as community or machine-assisted, pending review, or reviewed.
- Readers can tell which version they are viewing and distinguish it from the original.
- Media has an appropriate descriptive label; transcript, caption, or alt text is shown where available.
- Public pages and media requests do not expose drafts or unpublished versions. Following unpublishing, the next request no longer serves the version or its media.
- PDFs are offered as downloads; audio and video support valid byte-range playback requests.

**Scope and edge cases**

- V1 allows one version per work and language. Each version has one primary format; text is stored in the database and other formats reference one uploaded file.
- Terms are selected per version and are not silently inherited. Translation is owner-managed; automated translation and revision history are deferred.
- If the requested language is unavailable, show the available versions without implying that a translation exists. A story with no translations does not show a misleading switch control.
- Missing optional transcript, caption, or alt text is not implied to exist. Invalid media byte ranges return an error rather than incorrect partial content.

## 3. Publish a story with clear creator and reuse terms

**As a** storyteller, **I want** to publish a story with my chosen public name, description, language, format, attribution, and reuse terms, **so that** readers can understand who created it and how it may be reused.

**Value:** Readers can find and engage with stories while seeing the creator’s attribution and reuse terms.

**Acceptance criteria**

- A signed-in creator can create a draft with a title, public creator name or pseudonym, description, language, and one primary format.
- The creator can add story text or an uploaded media file and provide applicable image descriptions, audio/video transcripts or captions, and document details.
- The creator selects attribution and reuse terms for each version, or provides a custom permission statement. Terms are not silently inherited.
- Before publishing, the creator can preview the public page, including its media, attribution, and terms.
- Publishing makes the version visible to anonymous readers. Drafts remain unavailable to public pages, search results, and reader media requests.
- Public pages and results show public identity and omit private sign-in email and other private account fields.
- The interface explains that creator-selected terms do not by themselves establish consent or ownership.

**Scope and edge cases**

- Requires a signed-in creator account and the story/media creation flow. V1 supports in-place edits, not revision history.
- Reuse terms use available choices or a custom statement. The app does not make legal determinations.
- Missing required metadata or explicit attribution and reuse terms prevents publication. Invalid or over-limit media leaves the draft unpublished and produces a clear error.

## 4. Add a credited translation linked to its source

**As a** storyteller, **I want** to add a translation linked to my original story, with its own language, translator credit, review status, and reuse terms, **so that** readers can use it without losing its connection to the source.

**Value:** Readers can identify the source and understand who translated each version and whether it has been reviewed.

**Acceptance criteria**

- The story owner can add a translation as a separate version under the same work.
- The creator records its language, translator credit, and translation/review status, such as community or machine-assisted, pending review, or reviewed.
- The creator provides attribution and reuse terms for the translation; source terms are not silently copied.
- A published translation appears as an available version with its language, translator, status, and visible link to the source.
- Switching versions makes clear which language and version the reader is viewing and provides a way back to the source.
- A draft translation is not visible to anonymous readers; only published versions are available publicly.
- V1 does not generate translations automatically.

**Scope and edge cases**

- Requires an existing story and an authorized creator account. Translation uses the same supported formats and media limits as other versions.
- A work can have one version per language. A duplicate language or missing required language, translator credit, review status, or reuse terms is rejected with a clear message.
- If the source is not publicly available, a translation does not appear as an orphaned public result. Unpublishing removes a version from public navigation and blocks its media.

## 5. Upload and serve story media safely

**As a** storyteller, **I want** to attach supported media to a story version, **so that** readers can experience it in its intended format.

**Value:** Stories can be shared as text, images, audio, video, or PDF while uploads and published media remain protected.

**Acceptance criteria**

- The creator can upload JPEG, PNG, WebP, MP3, WAV, MP4, WebM, or PDF files up to 40 MiB each.
- The app checks actual streamed byte count and file type. It rejects unsupported types, files over the limit, and uploads exceeding the creator’s 20-file or 200 MiB account quota.
- Files are stored under generated IDs. A failed or interrupted upload is cleaned up and can be retried from the beginning.
- Readers can access media for published versions. Draft media is available only to its owner; media for unpublished versions is no longer accessible to readers.
- Audio and video support valid byte-range requests with `206` responses and reject invalid ranges with `416`. Responses use safe content headers and are not cached; PDFs download as attachments.
- The creator can provide applicable descriptive text, transcript, captions, or alt text.

**Scope and edge cases**

- Each version has one primary format and either text stored in SQLite or one uploaded media file. Resumable uploads and other document formats are deferred.
- Reject uploads that exceed temporary-space or host-disk reservations even if an individual file size is allowed. Clean stale temporary files and reservations after restart.
- Resolve media only by generated asset ID; never accept user paths or arbitrary filesystem access.

## 6. Manage my creator account and published stories

**As a** storyteller, **I want** to sign in and manage my own stories, **so that** I can publish and maintain my work under the identity I choose to show readers.

**Value:** Creators can publish under a display name or pseudonym while keeping sign-in details private.

**Acceptance criteria**

- A creator can sign up with a username, private email, display name, and password, then sign in using only username and password.
- A creator can access a dashboard and create or edit their own stories and versions. They cannot edit another creator’s work.
- A creator can change their password while signed in; other active sessions are revoked.
- A creator can see and correct their public display name. Private email and authentication fields are omitted from public pages, search results, public data, and logs.
- Sign-in and sign-up failures return generic messages that do not reveal whether a username exists. Username availability is not exposed.

**Scope and edge cases**

- Email is unverified and is not used for messaging or recovery in V1. Forgotten-password reset requires maintainer verification through an independently known out-of-band channel; if identity cannot be verified, the account cannot be recovered.
- Duplicate signup attempts return a generic response. Apply configured sign-in and sign-up rate limits.

## 7. Report a concern and let maintainers unpublish a version

**As a** signed-in reader, **I want** to report a story that concerns me, **so that** a maintainer can review it and remove it from public access if needed.

**Value:** Readers have a clear reporting path, and maintainers can respond without an in-app moderation system.

**Acceptance criteria**

- Only signed-in readers can submit a report for a story or version. The report is stored privately and is not shown on public pages.
- A reader receives confirmation without seeing other reports or maintainer notes.
- Reports are limited to five per account per hour.
- A maintainer can list reports and unpublish a version using host-only commands inside the app container; these actions are not available through web routes.
- Following unpublishing, the version is absent from public browsing and direct page access, and its media is inaccessible to readers on the next request.
- The creator’s draft or ownership data is retained when a version is unpublished.

**Scope and edge cases**

- An in-app moderation queue, report status workflow, and automatic moderation are deferred.
- Anonymous and rate-limited reports are rejected. Report listing and unpublishing commands do not expose passwords, session tokens, or private account email.
