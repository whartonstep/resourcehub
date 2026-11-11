# STEP Internal Resource Hub

A one-page site for STEP E/CDs, the board, and committees, with a few sections for everyone. It shows the next townhall, what's due soon, the STEP calendar, upcoming programming, how to run and promote an event, Pod Hangouts, and Cohort Cup standings. It also has an "Ask STEP" chat bot that answers questions about what's on the page.

The site is plain HTML hosted on GitHub Pages. There's no build step: edit, commit, and push to `main`, and the live site updates in a minute or two.

The events also power a public events page at [whartonstep.github.io](https://whartonstep.github.io) (repo: `whartonstep/whartonstep.github.io`). That page loads `events.js` from this site, so editing an event here updates both. You never need to touch the public repo to change an event.

## What's in the repo

| Path | What it is |
| --- | --- |
| `index.html` | The hub: layout, styles, content, and scripts |
| `events.js` | The event list, shared with the public events page |
| `images/` | Logo, favicon, touch icon, and event flyers |
| `po-instructions-for-vendors.pdf` | PO instructions linked from Run an Event |
| `chatbot/` | The "Ask STEP" bot, a Cloudflare Worker. See [chatbot/README.md](chatbot/README.md) |

## Updating content

Events are in `events.js`. Almost everything else you'll change is in the `<script>` block near the bottom of `index.html`.

### Settings (`CONFIG`)

- `gcalEmbedUrl`: the Google Calendar embed (STEP 26-27 plus the E/CD-only Mentorship calendar)
- `cohortCsvUrl`: the published CSV of the Cohort Cup tracker. The page finds the cohort names and reads the column headed "Total." If the sheet can't load, the page falls back to `FALLBACK_SCORES`.
- `chatbotUrl`: the bot's Worker address. Leave it empty to hide the chat button.

### Townhalls (`TOWNHALLS`)

A list of dates (`'YYYY-MM-DD'`). The page shows the next one and switches to the following date at 7 pm on the day of each townhall.

### Due Soon (`DUE`)

One entry per to-do:

```js
{ title: 'RSVP for the Hoco Alumni Brunch', due: '2026-11-07', tag: 'ecd', links: [
  ['RSVP form', 'https://forms.gle/...']
]},
```

- `tag`: `ecd` (E/CDs), `board` (board and committees), `all` (everyone), or `optional` (non-STEP opportunities, always listed last)
- `due`: the item hides after this date. With no `due`, set `label` (default "Open now") and optionally `hideAfter`.
- `links`: `[label, url]` pairs. If there's no link yet, leave `links: []` and set `pending` (for example `'Text Joel'`).
- Only add items that E/CDs, the board, or upperclassmen have to act on themselves. Registrations for first-year events go on the event, not here.

### Future Programming (`PROGRAMS` in `events.js`)

One entry per event. Past events hide on their own after their `date`. Clicking an event opens a pop-up with its details.

```js
{ title: 'Professional Dining 101', committee: 'ap', stage: 5,
  when: 'Mon, Nov 16, 6–8 pm', date: '2026-11-16',
  note: 'Short line shown in the list. Can include <a> links.',
  who: '...', what: '...', where: '...', why: '...',
  blurb: 'Full description. Separate paragraphs with \n\n.',
  links: [['Register', 'https://...']],
  image: ['images/flyer.webp', 'Alt text describing the flyer'] },
```

- `committee`: `ap`, `social`, `ce`, `mentorship`, `marketing`, `internal`, or `directors`
- `stage` (1–5): Idea, Planning, Date set, Location set, Finalized
- `date` is used for sorting and hiding. Leave it out for events with no date yet; they sort to the bottom.
- Everything after `when` is optional. Any of the five Ws you leave out shows "Not listed yet" in the pop-up.
- For a flyer, put the image in `images/` and add `image`.

**What the public page shows:** events at stage 3 (Date set) or later that have a `date`. It shows the title, `when`, the five Ws, `blurb`, flyer, and links. It never shows `note` or the stage bar.

- To keep an event off the public page, add `public: false`.
- To keep a link on the hub only (for example an E/CD form), add `'internal'` as a third item: `['Leader form', 'https://...', 'internal']`.
- `note` can mention internal details, since it only appears on the hub. Write the five Ws and `blurb` for a public audience.

### Other sections

Run an Event, Pod Hangouts, Files and Feedback, and Promote It are plain HTML higher up in `index.html`. Edit the links there directly. The "Suggested Events to Take Your Pod To" list is an embedded Google Sheet, so update the sheet, not the code.

## Previewing changes

Open `index.html` in a browser, or for images and the cohort CSV to load like they do on the live site, run this from the repo folder:

```bash
python3 -m http.server 8000
```

Then go to http://localhost:8000.

## Writing style

- Write "E/CD," never "E/CD and CD."
- Most of the hub's readers are first-years and E/CDs, so describe events for them.
- Everything here is public. Don't put private student information on the hub; the chat bot reads the page too.
