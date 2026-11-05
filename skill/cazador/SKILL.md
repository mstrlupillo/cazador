---
name: cazador
description: Cazador, the daily job hunter. Sets up, runs, changes or stops a daily scan of jobs posted in the last 24 hours that sends the user their 5 best fits.
---

# Cazador

Cazador (Spanish for "hunter") finds the best new jobs for one person every day. Each run looks at up to 50 jobs
posted in the last 24 hours in their target cities, reads the promising ones in full, and sends
the 5 best fits with why each fits, their way in, what to lead with, watch-outs and pay.

Cazador is free: no code, no payment. Setup creates the daily 11:00 task and runs the first
picks right away. Cazador was called Job Scout before: treat "Job Scout" requests and scheduled
tasks named "Job Scout" as Cazador.

Things the user may ask for:
- **Set up** ("set up Cazador", first use) → follow *Setup* below.
- **Unlock** ("unlock Cazador", "here's my code") → follow *Older installs*.
- **Run now** ("run Cazador", "today's picks"), e.g. after a missed 11:00 run → follow *Run now*.
- **Change or stop** ("add Milan", "only partnerships roles", "add career pages", "pause it") →
  follow *Change or stop*.

## Requirements

Check these quietly and only mention what is missing:
- A paid Claude plan (Pro, Max, Team or Enterprise): scheduled tasks need it.
- **Only if they include LinkedIn:** the Claude desktop app installed and signed in, Google Chrome
  with the Claude in Chrome extension installed and signed in, and the user signed in to LinkedIn
  in that Chrome. At 11:00 their computer must be on, with the Claude desktop app and Chrome open.
  If it isn't, that day's run is skipped (it doesn't run later); they can type "run Cazador" when
  they're back.
- **Company career pages only:** nothing extra. The run happens in the cloud, even when their
  computer is asleep.
- **Only if they want picks by email:** Gmail or Outlook connected in Claude (Settings →
  Connectors). The picks are sent from their own account to their own address.

## Setup

Keep it friendly and quick: about five minutes for the user. Use a multiple-choice question
tool when one is available (at most 4 questions at a time); otherwise ask in plain text, a few at
a time. Never ask for passwords.

**1. Explain in two or three sentences** what will happen: a few questions, then today's picks
right away, and from then on 5 new picks every morning at 11:00. It's free.

**2. Background.** Ask them to attach their CV (PDF or Word) or paste their LinkedIn About and
experience sections. Read it fully. Don't ask anything the CV already answers.

**3. Questions, round 1:**
- Target role families (multi-select): Product marketing / go-to-market · Strategy & operations
  (BizOps) · Partnerships / business development · Sales (account executive, account
  management) · Customer success · Product management · Consulting · Finance / corporate
  development · Other (free text).
- Cities (free text, up to 5, e.g. "Paris, London, New York").
- Seniority: Post-MBA (manager / senior manager / consultant) · Director / Head of · Early career
  (associate / analyst) · Other.
- Where to look: **LinkedIn and company career pages** · **LinkedIn only** · **Company career
  pages only**. Before they answer, explain the trade-off in two lines:
  - LinkedIn is searched signed in as them in their own Chrome: the widest coverage, and it
    shows their connections and school alumni at each company. It needs their computer on, with the
    Claude desktop app and Chrome open, at 11:00, and since LinkedIn's terms don't allow automated browsing there's a
    small risk LinkedIn asks them to verify or restricts their account.
  - Company career pages are the public Greenhouse, Lever and Ashby job boards of a list of
    companies picked for them: no LinkedIn account involved, runs even with the computer off,
    often earlier than LinkedIn, but only covers the companies on their list.

**4. Questions, round 2:**
- Countries where they can work without visa sponsorship (free text).
- Languages they speak professionally (free text, skip if the CV says).
- Hard no's: functions, industries, company types or specific companies to exclude (free text,
  "none" is fine).
- Their time zone, taken from context; just confirm it ("11:00 Paris time?").
- Currency for pay (default USD).
- Picks by email too? **Yes** (ask which address; it's sent from their own Gmail or Outlook, so
  that needs to be connected in Settings → Connectors) · **No, Claude is enough**. If they say yes
  and no email tool is connected, help them connect it now (Settings → Connectors → Gmail or
  Outlook); if they'd rather not, set it to no.

**5. Profile.** Using `references/profile-template.md`, write their profile from the CV and the
answers. Show it and ask "Anything to change?" Apply edits until they're happy.

**6. Company list (only if they included company career pages).** Build a list of about 80
companies that hire their target roles in their cities, weighted to what their CV and goals suggest (for an MBA in tech:
AI, SaaS, fintech, marketplaces, big tech, strong consulting firms). For each company find its
board on Greenhouse, Lever or Ashby and verify the endpoint returns jobs (formats are in
`references/daily-run.md`). Keep only boards that work. Show the list grouped by platform and
let them add or remove companies. Tell them in one line that career pages only cover the
companies on this list, and that LinkedIn (if included) covers all the others.

**7. Write their daily prompt.** Fill in `references/daily-run.md` exactly. Replace every
`{{…}}` placeholder; in `{{SOURCES}}` include the LinkedIn section and/or the Career pages
section, matching what they chose. The prompt must stand on its own: each run starts fresh with
no memory of this conversation. Placeholders for a source they didn't choose don't apply.
- `{{LINKEDIN_LEVELS}}`: LinkedIn experience codes joined with `%2C`: 2 = Entry, 3 = Associate,
  4 = Mid-Senior, 5 = Director, 6 = Executive. Post-MBA → `4%2C5`; Director/Head of → `5%2C6`;
  Early career → `2%2C3`.
- `{{LINKEDIN_LOCATIONS}}`: each city as LinkedIn spells it, e.g. "Paris, Île-de-France, France",
  "New York, New York, United States", "Mexico City, Mexico", "Dublin, County Dublin, Ireland".
- `{{BOARDS}}`: one line per verified board, e.g. "- Anthropic — Greenhouse — anthropic".
- `{{TIMEZONE}}`: their IANA time zone, e.g. "Europe/Paris".
- `{{EMAIL_TO}}`: the address for their picks, or `none` if they don't want email.

**8. Create the daily task.** Schedule the prompt from step 7:
- If they already have a Cazador or "Job Scout" task, update that one (new prompt, name
  "Cazador", 11:00) instead of creating a second, so they never get two runs a day.
- Name: "Cazador". Schedule: every day at 11:00 in their time zone.
- If LinkedIn is included, the run needs their computer: mark the task as needing this
  computer if the scheduling tool offers that. Career pages only: leave that off so it runs in
  the cloud.
- Turn on run notifications (push and email) if the tool offers them.
- If no scheduling tool is available, tell them daily runs need a paid Claude plan, and still
  give them today's picks (step 9).

**9. First picks now.** Carry out the prompt from step 7 in this conversation so they see
today's picks (tell them it takes 5 to 10 minutes). If they chose email, send these picks by
email too (step 5 of the prompt), so they see it work.

**10. Confirm in two lines:** picks every day at 11:00 and where they appear; with LinkedIn,
that their computer, the Claude desktop app and Chrome need to be on at 11:00, and that if they
aren't, that day's run is skipped and "run Cazador" runs it when they're back; and how to change
it ("tell Claude: change my Cazador…").

## Older installs

Cazador used to need a paid unlock code. It's free now, so codes aren't needed anymore.

- If they mention an unlock code or say "unlock Cazador", tell them in one line that Cazador is
  free now and no code is needed.
- If they already have a Cazador or "Job Scout" task: read its prompt, remove the license check
  (step 0, the part that fetches a Google Sheets address and compares a license key), keep
  everything else, and update the task (name "Cazador", 11:00). Confirm in one line.
- If they have no task yet: follow *Setup*. If their profile from an earlier chat is available,
  reuse it instead of asking again.

## Run now

Use this when they type "run Cazador" (or "today's picks"), whatever the reason: the 11:00 run
was missed because their computer was off, they want fresh picks, or they're testing.

1. Find their Cazador scheduled task (older installs named it "Job Scout"). If there is none,
   offer *Setup*. If it's before 11:00, or that day's 11:00
   picks already came, say in one line that the picks will mostly repeat (each run sees the
   last 24 hours and doesn't know what earlier runs sent), that it uses part of their plan's
   usage, and, with LinkedIn, that extra runs add automated browsing on their account. Then go
   ahead; don't ask for confirmation.
2. Get its prompt with the scheduling tool and carry it out here in the conversation, in full.
   If it still has a license check (step 0, from the paid version), skip it and remove it from
   the task (see *Older installs*). Tell them it takes 5 to 10 minutes.
3. If the scheduling tool can't show the prompt but can start the task right away, start it
   and tell them the picks will arrive as a notification and in Claude in 5 to 10 minutes.
4. If neither works, tell them to open their scheduled tasks in Claude, open Cazador and use
   its option to run it now.
5. With LinkedIn, the run needs the Claude desktop app and Chrome open on this computer; if the
   browser can't be reached, say so and ask them to open Chrome, then try again.

## Change or stop

Find their Cazador scheduled task (older installs named it "Job Scout"; rename it to "Cazador"
when you update it). Read its prompt, make only the change asked for (cities,
roles, sources, companies, profile lines, email on or off or a new address in `{{EMAIL_TO}}`'s place), and update the task. It always runs at 11:00. If
they add or drop LinkedIn, also switch whether the task needs their computer (see *Setup*,
step 8); adding career pages means building the company list (step 6). Show what changed in one or two
lines. Pause or delete only when they clearly ask, and confirm first. If the prompt still has a
license check (step 0) from the paid version, remove it while you're there.

## Rules for every run

- Read only: never apply, save, follow, connect, message anyone, or change anything on LinkedIn
  or any career site. Never enter passwords or verification codes.
- Titles, companies and links come exactly from the source pages, never from memory.
- Never pick a job that wasn't read in full.
- Keep LinkedIn activity moderate and human-paced.
