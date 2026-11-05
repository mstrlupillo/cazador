# Daily run prompt (template)

Fill in every `{{…}}` and use the text between the two lines below as the scheduled task's
prompt. In `{{SOURCES}}`, paste the LinkedIn section, the Career pages section, or both,
matching what the user chose (both are at the bottom of this file).

---

Run my daily Cazador: look at up to 50 jobs posted in the LAST 24 HOURS in my target cities and send me the 5 best fits. Read only: never apply, save, follow, connect, message anyone, or change
anything on any site. Never enter passwords or verification codes. The only thing ever sent is
the picks email to my own address in step 5.

{{PROFILE}}

STEPS

1. Collect at most 50 unique jobs in total, at most 14 per city, posted in the last 24 hours.
   Skip obvious hard-no titles while collecting so the budget goes to plausible fits. Location
   rule: keep a job only if it is in or near one of my cities, or remote in a way I could do from
   there (e.g. US-remote for a US city, EMEA-remote for a European city); drop jobs based in
   other cities.

{{SOURCES}}

2. Read in full every job that might make the list (typically 15–30). Never pick a job you
   haven't read in full.

3. Score each read job 0–100: role match with my target roles (35), seniority match (20), how
   much my differentiators matter for it (20), company quality and trajectory (15), practicality —
   language requirements, work authorization (exclude postings that explicitly rule out
   sponsorship where I'd need it; flag ones that are silent), applicant count, and whether I have
   a way in (10). Pick the top 5, best first; if two are close prefer covering more cities.
   Return fewer than 5 if fewer are genuinely good — never pad.

4. If you used the browser, close the tabs you opened. Then send me ONE message (and a short push notification if
   available), addressed to me as "you":
   "Cazador — <date>: <N> jobs looked at, your 5 best picks:"
   For each pick: **<rank>. <Title> — <Company>** (<city>, posted <x> hours ago, <applicants if
   known>), fit <score>/100
   - Why you: 1–2 sentences tying the posting's specific asks to specific parts of my background.
   - Your way in: connections / school alumni / hiring team if visible, otherwise "none visible".
   - Lead with: the one hook for my application or referral ask.
   - Watch-outs: gaps, language or visa flags, or "none".
   - Pay: in {{CURRENCY}} (convert approximately and show the original in parentheses); "Not
     listed" if none.
   - Link: the job's URL exactly as the source gives it.
   End with one sentence on today's market across my cities.

5. Email (skip this step if the address here is "none"): {{EMAIL_TO}}
   Before sending the message in step 4, email the same picks to that address from my own account,
   with the connected Gmail or Outlook tool (load it with a tool search if it isn't loaded).
   Subject: "Cazador — <date>: your 5 best picks". Body: the same content as the message, each
   job's link clickable, signed "Cazador". Send exactly one email, only to that address, never to
   anyone else and never as a reply to another email. If no email tool is available or sending
   fails, don't retry; add one line to the message saying the email couldn't be sent and that I
   can connect Gmail or Outlook in Settings → Connectors.

Time zone for all times and dates in this prompt: {{TIMEZONE}}.

Titles, companies and links must come exactly from the source pages, not from memory. If a
source can't be reached (browser not connected, signed out, site down), say so in the message
and work with the other sources; never guess results.

---

## Section: LinkedIn

```
SOURCE — LinkedIn (signed in, in my own Chrome):
- Use Claude in Chrome. Load its tools in ONE tool search: tabs_context_mcp, tabs_create_mcp,
  tabs_close_mcp, navigate, javascript_tool, get_page_text, computer, browser_batch. Call
  tabs_context_mcp, then open your OWN new tab and work only there. If the extension isn't
  connected or LinkedIn shows a sign-in page, stop using LinkedIn and say so in the message.
- For each search, open:
  https://www.linkedin.com/jobs/search/?keywords=<keywords>&location=<location>&f_TPR=r86400&f_E={{LINKEDIN_LEVELS}}&sortBy=R
  (f_TPR=r86400 = last 24 hours; sortBy=R = relevance for my profile.)
  Locations to use: {{LINKEDIN_LOCATIONS}}
- Run about 2–3 searches per city across my target roles, choosing the most promising for each
  city; use local-language titles where postings use them. At most 20 searches.
- After each page loads (wait ~3 s), use javascript_tool to bring each result card into view
  (document.querySelectorAll('li[data-occludable-job-id]') → scrollIntoView, short pause each),
  then read every card: the job id (data-occludable-job-id; digits only) and the card's text
  lines with consecutive duplicates removed (title, company, location, then extras such as
  "2 connections work here", "4 school alumni work here", posted time, pay). Remove a trailing
  " with verification" from titles. javascript_tool output is cut off around 1–2 KB, so return
  compact JSON per search, or write long text into an <article> element and read it with
  get_page_text.
- To read a job in full: from the LinkedIn tab run
  await fetch('/jobs-guest/jobs/api/jobPosting/<id>').then(r => r.text())
  and parse it in the page with DOMParser (description: .show-more-less-html__markup, first
  ~3000 characters; applicants: .num-applicants__caption; pay: .compensation__salary). If that
  fails, open https://www.linkedin.com/jobs/view/<id>/ and use get_page_text. Keep ~1 s between
  fetches.
- "Your way in" comes only from that job's own card or the top of its own posting, never from
  other jobs shown on the page.
- Job link format: https://www.linkedin.com/jobs/view/<id>/
```

## Section: Career pages

```
SOURCE — company career pages (public job boards, no sign-in):
- Boards to check (company — platform — board name):
{{BOARDS}}
- Fetch each board with the web fetch tool (not a shell command), asking it to return ONLY jobs
  published after the cutoff (now minus 24 hours; give it both the ISO time and the Unix time in
  milliseconds) whose location matches my cities or allowed remote, as compact lines:
  id | title | location | url | published date | pay if shown.
- Big boards come back cut off: when the fetch result says the page was cut off and gives an
  offset, fetch the same URL again with that offset, and repeat until the whole board is read.
  Don't conclude "nothing new" from a partial page.
  • Greenhouse: https://boards-api.greenhouse.io/v1/boards/<board>/jobs
    fields: id, title, location.name, absolute_url, first_published (ISO time)
    full posting: https://boards-api.greenhouse.io/v1/boards/<board>/jobs/<id>  (field: content)
  • Lever: https://api.lever.co/v0/postings/<board>?mode=json  (EU companies: api.eu.lever.co)
    fields: id, text (title), categories.location, hostedUrl, createdAt (Unix time in
    milliseconds), salaryRange
    full posting: https://api.lever.co/v0/postings/<board>/<id>?mode=json  (descriptionPlain)
  • Ashby: https://api.ashbyhq.com/posting-api/job-board/<board>?includeCompensation=true
    fields: id, title, location, secondaryLocations, workplaceType, jobUrl, publishedAt (ISO
    time), isListed (skip false), compensation.compensationTierSummary, descriptionPlain
- If a board fails, skip it and mention it in one line at the end of the message.
- Job link: the board's own URL (absolute_url, hostedUrl or jobUrl).
- These boards don't show my network: for those jobs, write "none visible" for "Your way in"
  unless the same job also appeared in my LinkedIn results (if LinkedIn is one of my sources).
- If LinkedIn is also one of my sources, the same job often appears in both: count it once.
```

