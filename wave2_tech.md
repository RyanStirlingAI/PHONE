# Wave 2: Technology Stack and Systems Specification

## How Many NZ -- Technical Blueprint for a Solo Operator

This document specifies every system, tool, and automation the campaign will build and use. Everything here passes three tests: it costs $0 or close to it, one person can build and maintain it, and Claude Code can do the heavy lifting on the build.

---

## 1. THE TECH STACK

### Data Collection

**Stats NZ Open Data API**
What it does: Pulls migration, population, housing, and demographic data directly from the government's statistical service. Stats NZ runs an open data portal at nzdotstat.stats.govt.nz with a SDMX-compatible REST API, plus newer endpoints at datadot.stats.govt.nz (based on .Stat Suite). Key datasets: International Travel and Migration (ITM), Estimated Resident Population, Building Consents, and Subnational Population Estimates.
Why chosen: Primary source for every major claim the campaign makes. API access means data can be pulled programmatically rather than manually downloading CSVs.
Cost: $0.
Setup time: 2-3 hours with Claude Code to write the Python wrapper and test key queries.

**MBIE Immigration Data**
What it does: Monthly and quarterly visa approval data by category, employer accreditation statistics, and sector breakdowns. Available through the MBIE migration data explorer (mbienz.shinyapps.io) and downloadable datasets at mbie.govt.nz. Some data available through their API; most as CSV/Excel downloads on a published schedule.
Why chosen: More granular than Stats NZ on visa categories and labour market dimensions.
Cost: $0.
Setup time: 1-2 hours to write scrapers and download scripts.

**Parliamentary Hansard**
What it does: Full text of everything said in Parliament. Available through parliament.nz with searchable records and RSS feeds for new content. The official Hansard XML feed provides structured data.
Why chosen: Feeds the accountability tracker. Every statement a politician makes about immigration gets logged.
Cost: $0.
Setup time: 2-3 hours to build the monitoring script and keyword filter.

**NZ Parliament Bills and Select Committee Tracker**
What it does: Monitors new bills, select committee calls for submissions, and committee reports. Parliament.nz provides RSS feeds and email alerts for these.
Why chosen: Ensures the campaign never misses a submission deadline or a relevant piece of legislation.
Cost: $0.
Setup time: 30 minutes to subscribe to the right feeds and configure alerts.

**Council Long-Term Plans and Infrastructure Strategies**
What it does: Population projections and infrastructure budgets from local councils. These are published as PDFs and occasionally structured data on individual council websites.
Why chosen: Provides the local dimension. The gap between projected population and funded infrastructure is content gold.
Cost: $0.
Setup time: 4-5 hours across the first month to download and index the key council documents (Auckland, Wellington, Christchurch, Tauranga, Hamilton, Queenstown). Not automated -- these change infrequently enough to update manually.

**Media Monitoring (RSS + keyword alerts)**
What it does: Monitors NZ Herald, Stuff, RNZ, Newsroom, Interest.co.nz, and The Kaka for immigration-related stories.
Why chosen: Enables reactive content within hours of a relevant story.
Cost: $0. Use a combination of RSS feeds (via a free reader like Feedly free tier or a self-hosted solution) and Google Alerts.
Setup time: 1 hour.

### Data Analysis

**Python (pandas, matplotlib, plotly)**
What it does: All data processing, analysis, and chart generation. Claude Code writes and maintains the scripts.
Why chosen: Free, powerful, and Claude Code is excellent at writing Python data analysis code. Pandas handles the Stats NZ and MBIE data natively.
Cost: $0.
Setup time: Already available in the Claude Code environment.

**SQLite**
What it does: Local database for the OIA tracker, accountability tracker, and data cache. Single-file database, no server required, trivial to backup.
Why chosen: Zero infrastructure. No database server to maintain. Claude Code can build and query it directly.
Cost: $0.
Setup time: 30 minutes to create the schema.

**Claude API (via Claude Code)**
What it does: Analyses OIA response documents, extracts key findings, drafts content, compares politician statements against historical records.
Why chosen: This is the force multiplier that makes one person operate like a small team.
Cost: The Claude Code subscription or API credits. The API itself is usage-based; at the volumes this campaign generates, expect $20-50/month. This is the one real cost in the stack.
Setup time: Already available.

### Content Production

**Canva (Free Tier)**
What it does: Produces the branded graphics -- stat cards, scorecard visuals, comparison charts, social media templates.
Why chosen: Free tier is sufficient for everything the campaign needs. Templates ensure brand consistency without design skills. Supports the How Many NZ colour palette (navy #1B2A4A, off-white #F5F5F0, teal #2A7F6F, amber #D4860B).
Cost: $0.
Setup time: 2-3 hours to build the initial template set (stat card, scorecard, comparison, weekly wrap header).

**Claude Code (Content Drafting)**
What it does: Drafts social media posts, threads, fact-checks, weekly wraps, and OIA analysis write-ups following the voice guide and pillar templates from the content strategy.
Why chosen: Cuts content production time from hours to minutes. The human edits for tone and accuracy rather than drafting from scratch.
Cost: Covered under Claude API costs above.
Setup time: 1-2 hours to build the prompt templates for each content pillar.

**Phone Camera + CapCut (Free)**
What it does: Records and edits short-form video content for TikTok and Reels. CapCut handles text overlays, captions, and basic editing.
Why chosen: Free, mobile-first, designed for the exact format these platforms reward.
Cost: $0.
Setup time: 30 minutes.

### Content Distribution

**X/Twitter**
What it does: Primary platform for reaching journalists and the political class. This is where the agenda-setting happens.
Why chosen: NZ's political journalists live on X. Bernard Hickey, Thomas Coughlan, the entire Press Gallery -- they're all there. A post that gets traction on X reaches the people who matter.
Cost: $0.
Setup time: 30 minutes.

**Facebook**
What it does: Reaches the older, higher-turnout voter demographic. Local Impact Stories perform best here because of community group sharing.
Why chosen: Facebook still dominates NZ social media by user numbers, especially in the 35+ demographic that votes most reliably.
Cost: $0.
Setup time: 30 minutes.

**TikTok**
What it does: Reaches the under-35 demographic with short-form video. Algorithm is more favourable to new accounts than X or Facebook.
Why chosen: Undercrowded for NZ political content. A well-made 30-second data explainer can reach audiences that never see political X.
Cost: $0.
Setup time: 30 minutes.

**Instagram (Reels + Stories)**
What it does: Cross-posts video content from TikTok and static graphics from the X feed.
Why chosen: Incremental reach for near-zero additional effort.
Cost: $0.
Setup time: 15 minutes.

**GitHub Pages (howmany.nz dashboard hosting)**
What it does: Hosts the public data dashboard as a static site.
Why chosen: Free hosting, supports custom domains, handles enough traffic for a NZ political campaign. No server to maintain.
Cost: $0 for hosting. ~$25/year for the howmany.nz domain.
Setup time: 1 hour for initial deployment.

### Monitoring and Alerts

**Python scripts on a cron job (local machine or free-tier cloud)**
What it does: Runs the monitoring scripts on schedule -- checking for new Stats NZ releases, new Hansard entries, new media articles matching keywords.
Why chosen: Simple, reliable, no dependencies.
Cost: $0 if run locally. $0 on Oracle Cloud free tier or similar if you want it running 24/7 without your laptop open.
Setup time: 1-2 hours.

**Ntfy.sh or email alerts**
What it does: Sends push notifications to your phone when a monitoring script flags something.
Why chosen: Ntfy.sh is free, open source, and requires no account for basic use. Alternatively, a simple Python script can send emails via Gmail SMTP.
Cost: $0.
Setup time: 30 minutes.

### Tracking and Measurement

**Plausible Analytics (self-hosted) or Umami**
What it does: Privacy-respecting analytics for the dashboard website. Tracks page views, referrers, and geographic distribution.
Why chosen: Free if self-hosted. Lightweight. No cookie consent banner needed (privacy-respecting by design). Gives you data on which dashboard pages journalists are viewing.
Cost: $0 self-hosted on Oracle Cloud free tier.
Setup time: 2 hours.

**Manual tracking spreadsheet (Google Sheets)**
What it does: Tracks follower counts, engagement rates, media mentions, and journalist interactions across platforms.
Why chosen: Free. Simple. A weekly 10-minute update is all it needs.
Cost: $0.
Setup time: 15 minutes.

---

## 2. THE OIA MACHINE

### Purpose

An OIA request management system that lets one person run 50-100 simultaneous OIA requests across multiple agencies, track deadlines, automate follow-ups, process responses into content, and maintain a complete audit trail.

### Database Schema (SQLite)

```sql
-- Core OIA request tracking
CREATE TABLE agencies (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    short_name TEXT NOT NULL,
    oia_email TEXT NOT NULL,
    notes TEXT
);

CREATE TABLE oia_requests (
    id INTEGER PRIMARY KEY,
    agency_id INTEGER NOT NULL REFERENCES agencies(id),
    reference_number TEXT,           -- Agency's reference once assigned
    subject TEXT NOT NULL,            -- Brief description
    request_text TEXT NOT NULL,       -- Full text of the request
    date_sent DATE NOT NULL,
    date_acknowledged DATE,
    date_due DATE NOT NULL,          -- 20 working days from date_sent
    date_extended DATE,              -- New deadline if extension granted
    extension_reason TEXT,
    date_response_received DATE,
    status TEXT NOT NULL DEFAULT 'sent',
        -- sent, acknowledged, extended, responded, refused,
        -- partial, ombudsman_complaint, closed
    response_type TEXT,
        -- full_release, partial_release, refused, transferred
    refusal_grounds TEXT,            -- e.g., s9(2)(f)(iv)
    content_potential TEXT,          -- high, medium, low, none
    content_published INTEGER DEFAULT 0,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE oia_documents (
    id INTEGER PRIMARY KEY,
    request_id INTEGER NOT NULL REFERENCES oia_requests(id),
    filename TEXT NOT NULL,
    file_path TEXT NOT NULL,
    document_type TEXT,              -- response_letter, attachment, briefing, etc.
    page_count INTEGER,
    summary TEXT,                    -- Claude-generated summary
    key_findings TEXT,               -- Claude-extracted findings as JSON
    date_added TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE oia_followups (
    id INTEGER PRIMARY KEY,
    request_id INTEGER NOT NULL REFERENCES oia_requests(id),
    followup_type TEXT NOT NULL,
        -- deadline_reminder, extension_query, ombudsman_complaint,
        -- media_pitch, content_draft
    date_sent DATE,
    content TEXT,
    status TEXT DEFAULT 'pending'
);

-- Content produced from OIA responses
CREATE TABLE oia_content (
    id INTEGER PRIMARY KEY,
    request_id INTEGER NOT NULL REFERENCES oia_requests(id),
    content_type TEXT NOT NULL,      -- stat_drop, oia_reveal, thread, media_pitch
    platform TEXT,                   -- x, facebook, tiktok, dashboard
    draft_text TEXT NOT NULL,
    final_text TEXT,
    date_published DATE,
    engagement_notes TEXT
);
```

### Workflow

**Step 1: Drafting requests**

Claude Code drafts OIA requests from a brief. The operator provides a one-line description of what they want to know, and Claude Code produces a properly formatted request using the template below.

Claude Code prompt for drafting:

```
I need an OIA request to [agency]. I want to know [brief description].

Draft a formal OIA request that:
- Cites the Official Information Act 1982
- Is specific enough to avoid refusal as "too broad"
- Requests specific document types where appropriate (BIMs, Cabinet papers, aide-memoires, internal analysis)
- Specifies a reasonable date range
- Asks for the information in the most useful format (electronic copies preferred)
- Includes a request for any related documents that provide context
- Is polite and professional

Use this template structure:
[Date]
[Agency name]
By email: [agency OIA email]

Dear Sir/Madam,

Under the Official Information Act 1982, I request...

[Specific request]

I would appreciate receiving the information in electronic form. If you intend to charge for this request, please let me know in advance.

If this request is refused in whole or in part, please cite the specific grounds under the Act.

Yours faithfully,
[Name]
```

**Step 2: Sending and logging**

The operator sends the request via email and logs it in the database. Claude Code calculates the 20-working-day deadline (accounting for NZ public holidays) and creates the database entry.

**Step 3: Automated deadline tracking**

A daily cron job checks the database for:
- Requests approaching their deadline (5 days out): generates an alert
- Requests past their deadline with no response: drafts a follow-up email
- Requests with extensions approaching: generates an alert
- Requests past extended deadline: drafts an Ombudsman complaint

**Step 4: Processing responses**

When a response arrives, the operator saves the documents to a local folder. Claude Code then:
1. Reads the response letter and any attachments
2. Extracts key findings and summarises each document
3. Compares findings against the existing data in the accountability tracker
4. Rates the content potential (high/medium/low)
5. If high potential: drafts an OIA Reveal post for X, a longer analysis for the dashboard, and a media pitch email for the most relevant journalist
6. Updates the database

Claude Code prompt for processing:

```
I've received an OIA response. Here are the documents:

[Paste or attach documents]

Please:
1. Summarise each document in 2-3 sentences
2. Extract the key findings -- what does this tell us that wasn't publicly known?
3. Identify any contradictions with public statements by politicians (check against the accountability tracker data I'll provide)
4. Rate the content potential: HIGH (genuinely newsworthy, exclusive finding), MEDIUM (useful context, supports existing arguments), LOW (routine, confirms what's already known)
5. If HIGH or MEDIUM, draft:
   a. A 280-character X post with the single most striking finding
   b. A full OIA Reveal post (200-400 words) following the How Many NZ voice guide
   c. A 2-paragraph media pitch email suitable for sending to a political journalist
6. If the response includes refusals or redactions, note the grounds cited and draft a response (either an Ombudsman complaint or a social media post about the refusal, depending on the grounds)
```

**Step 5: Ombudsman complaints**

For refusals, Claude Code drafts a formal complaint to the Office of the Ombudsman, citing the specific section of the OIA used for refusal and arguing for release. The complaint itself becomes content: "We've complained to the Ombudsman because [agency] won't release [description]."

### Batch Strategy

Week 1: File 10 foundational requests across 5 agencies:
- MBIE: Net migration data by visa category for last 3 years; internal advice to ministers on migration levels
- Stats NZ: Regional population projections provided to government agencies
- Treasury: Fiscal impact modelling of net migration
- Ministry of Housing: Housing demand projections linked to population growth
- Ministry of Education: School capacity planning relative to migration-driven population growth

Week 3: File 10 more targeted at the "nobody is in charge" angle:
- DPMC: Cross-government strategy documents on population planning
- MBIE: Whether any agency is responsible for matching infrastructure to population growth
- Ministry of Health: Health workforce planning relative to population growth
- Ministry of Transport: Transport infrastructure planning linked to population projections
- Auckland Council, Wellington City Council: Infrastructure investment mapped to population milestones

Month 2: File 15-20 based on what the first responses reveal. Every interesting finding generates 2-3 follow-up requests.

Month 3+: Maintain 30-50 active requests at any time. Claude Code manages the pipeline; the operator spends 30 minutes per day on OIA administration.

---

## 3. THE DATA DASHBOARD

### Purpose

A public-facing website at howmany.nz that presents immigration and infrastructure data in clear, updated visualisations. It serves three audiences: journalists who need quick data for stories, politically engaged citizens who want to understand the numbers, and the campaign itself as a credibility anchor.

### Architecture

Static site built with HTML, CSS, and JavaScript. Data processed by Python scripts that run on a schedule and output JSON files. Charts rendered client-side using Chart.js (lightweight, free, no dependencies). Hosted on GitHub Pages with a custom domain.

No backend server. No database on the web side. All data is pre-processed into static JSON files by Python scripts that run locally or on a free-tier cloud instance.

```
howmany-nz/
├── index.html              # Landing page with headline stats
├── css/
│   └── style.css           # How Many NZ brand colours and typography
├── js/
│   ├── charts.js           # Chart rendering with Chart.js
│   └── data-loader.js      # Fetches JSON data files
├── data/
│   ├── migration.json      # Net migration time series
│   ├── housing.json        # Housing consents vs population growth
│   ├── health.json         # Health capacity vs population
│   ├── education.json      # School rolls vs capacity
│   ├── regional.json       # Regional breakdowns
│   ├── parties.json        # Party scorecard data
│   └── last_updated.json   # Timestamp of last data refresh
├── pages/
│   ├── migration.html      # Detailed migration data page
│   ├── housing.html        # Housing gap analysis
│   ├── health.html         # Health infrastructure gap
│   ├── scorecard.html      # Party scorecard
│   ├── oia.html            # OIA findings archive
│   └── about.html          # About the campaign, methodology, sources
└── scripts/
    ├── fetch_stats_nz.py   # Pulls data from Stats NZ API
    ├── fetch_mbie.py       # Pulls MBIE immigration data
    ├── process_data.py     # Transforms raw data into dashboard JSON
    └── deploy.sh           # Pushes updated data to GitHub Pages
```

### Data Sources and Endpoints

**Stats NZ -- NZ.Stat API**
Base URL: `https://nzdotstat.stats.govt.nz/WBOS/index.aspx` (SDMX REST queries)
Key datasets:
- International Travel and Migration (ITM): quarterly net migration, arrivals, departures by visa type. Dataset code: `ITM`. Updated quarterly.
- Estimated Resident Population: `ERP`. Updated quarterly.
- Building Consents Issued: `BLD`. Updated monthly.
- Subnational Population Estimates: `SUB`. Updated annually.

Example API call for net migration data:
```
https://nzdotstat.stats.govt.nz/wbos/index.aspx/DOTSTAT/query?datasetcode=ITM&format=csv
```

The newer Stats NZ data portal (datadot.stats.govt.nz) uses the OECD's .Stat Suite API. Claude Code will write the specific query parameters once the exact table IDs are confirmed during build.

**MBIE Migration Data**
URL: `https://www.mbie.govt.nz/immigration-and-tourism/immigration/immigration-research-and-evaluation/` (downloadable datasets in CSV/Excel)
Key datasets:
- Migration Trends report (annual)
- Monthly visa approvals by category
- Employer Accredited Employer Work Visa data

These are mostly CSV downloads on a published schedule rather than a live API. The Python script downloads fresh files when they appear.

**Reserve Bank of NZ (RBNZ)**
URL: `https://www.rbnz.govt.nz/statistics`
Key datasets: Population-related economic indicators, housing market data. Provides some data via API.

**Ministry of Housing and Urban Development**
URL: `https://www.hud.govt.nz/stats-and-insights/`
Key datasets: Housing dashboard data, public housing waitlist.

### Visualisations

The dashboard presents six core views:

1. **The Headline** (landing page): A single large number showing the latest annual net migration figure, with a comparison line ("That's a city the size of [X]"). Below it, three smaller cards: housing gap, health workforce gap, and education capacity gap. Updated quarterly when Stats NZ releases new data.

2. **Net Migration Over Time**: Line chart showing annual net migration from 2000 to present. Overlay lines for housing consents, hospital beds added, and school places added. The visual gap between the migration line and the infrastructure lines IS the argument. Navy line for migration, teal for infrastructure, amber for the gap.

3. **The Housing Gap**: Bar chart comparing annual net migration-driven housing demand (calculated from migration figures and Stats NZ household size data) against actual building consents. Two bars per year, side by side. The shortfall is visually obvious.

4. **Regional Breakdown**: Interactive map or table showing population growth vs infrastructure investment by region. Allows people to find their own area and see their local gap. This is what drives Facebook sharing.

5. **International Comparison**: Side-by-side table comparing NZ's population planning framework (or lack thereof) with Australia, Canada, UK, and Singapore. Simple checkmarks and crosses: Does the country have a migration target? An independent advisory body? A published infrastructure link? NZ gets crosses on every row.

6. **Party Scorecard**: Visual scorecard showing each party's position on key immigration planning criteria. Green/amber/red ratings. Updated as parties respond (or don't). This page gets its own URL for easy sharing: howmany.nz/scorecard.

### Update Process

Claude Code writes a Python script that runs weekly (or on-demand when new data drops):
1. Fetches latest data from Stats NZ API and MBIE downloads
2. Processes into the dashboard JSON format
3. Generates updated chart data
4. Commits to the GitHub repository
5. GitHub Pages automatically deploys the update

The operator triggers an update when major new data arrives (quarterly Stats NZ releases, monthly MBIE data). Between releases, the dashboard is static and requires no maintenance.

### Build Plan

Claude Code builds the entire dashboard. The prompt sequence:

```
Prompt 1: "Build a static website for howmany.nz using HTML, CSS, and Chart.js.
Use these brand colours: navy #1B2A4A, off-white #F5F5F0, teal #2A7F6F,
amber #D4860B. Font: Inter. The landing page should show one large number
(net migration), a subtitle comparison, and three stat cards below.
Clean, minimal, mobile-responsive. The aesthetic is a government report
designed by someone who cares about readability. Numbers are always the
largest element."

Prompt 2: "Write a Python script that fetches net migration data from the
Stats NZ API and outputs it as a JSON file suitable for Chart.js.
Include quarterly data from 2015 to present."

Prompt 3: "Add a housing gap page that shows a bar chart:
migration-driven housing demand vs building consents per year.
Pull housing consent data from the Stats NZ BLD dataset."

[Continue for each page]
```

Estimated build time: 8-12 hours of focused work with Claude Code across the first two weeks.

---

## 4. THE CONTENT ENGINE

### How Claude Code Assists With Content Production

Claude Code does not replace the operator's judgement. It handles the mechanical work: pulling data, drafting text, formatting for platforms, and maintaining consistency with the voice guide. The operator provides editorial direction, checks accuracy, and adds the human texture that makes the voice believable.

### Workflow 1: Stats NZ Data Release to Social Media Post

**Trigger:** Stats NZ publishes new quarterly migration data.

**Step 1 -- Data pull (Claude Code, 5 minutes):**
```
Stats NZ just released the International Travel and Migration data for
[quarter]. Pull the new data using our fetch script. Compare to the
previous quarter and the same quarter last year. Calculate:
- Annual net migration (rolling 12 months)
- Change from previous quarter
- Change from same quarter last year
- Comparison to OECD average growth rate
- Housing demand implied (using 2.7 persons per household)
- How this compares to building consents for the same period
```

**Step 2 -- Draft post (Claude Code, 5 minutes):**
```
Using the How Many NZ voice (data-first, question-led, frustrated but
not angry, NZ English), draft a Stat Drop post for X/Twitter.

The data: [paste analysis output]

Requirements:
- Lead with the single most striking number
- Include one vivid comparison (city size, stadium capacity, etc.)
- End with a question directed at the government or a specific party
- Cite Stats NZ as the source
- Keep under 280 characters for the main post, with a follow-up
  tweet containing the detail
- Do NOT use the word "immigration" in the first line. Lead with
  the lived impact.
```

**Step 3 -- Edit and publish (human, 15 minutes):**
Operator reviews for accuracy, adjusts tone, adds any personal observation, and posts.

**Total time: 25 minutes from data release to published post.**

### Workflow 2: OIA Response to Thread

**Trigger:** OIA response arrives by email.

**Step 1 -- Process documents (Claude Code, 10 minutes):**
```
I've received this OIA response from [agency]. Here are the documents.

[Attach/paste documents]

Extract:
1. The single most newsworthy finding
2. Any numbers that contradict public statements by ministers
3. Any refusals or redactions and the grounds cited
4. The "money quote" -- the single sentence from the documents
   that would make the best screenshot
```

**Step 2 -- Draft OIA Reveal (Claude Code, 10 minutes):**
```
Draft an OIA Reveal post for X/Twitter as a thread (3-5 tweets).

Thread structure:
- Tweet 1: "I OIA'd [agency] asking [what]. Here's what they said."
  [Screenshot prompt]
- Tweet 2: The key finding in plain language
- Tweet 3: Why this matters / what it means for [housing/health/etc.]
- Tweet 4: The accountability angle -- who should have known this,
  who is responsible
- Tweet 5: CTA -- "If you're a journalist and want the full documents,
  DM me."

Voice: How Many NZ. Frustrated but measured. Let the documents do the
heavy lifting. Don't editorialize beyond what the data supports.
```

**Step 3 -- Draft media pitch (Claude Code, 5 minutes):**
```
Draft a short email pitch for [journalist name] about this OIA finding.

2-3 paragraphs max. Lead with the finding, not with who you are.
Offer the full documents. Don't tell them how to cover it.
Keep it factual and let them see the story potential themselves.
```

**Step 4 -- Edit and execute (human, 30 minutes):**
Operator reviews everything, creates the document screenshot with key passages highlighted, posts the thread, and sends the media pitch.

**Total time: 55 minutes from receiving the response to published content and journalist pitch.**

### Workflow 3: Politician's Statement to Fact-Check

**Trigger:** A minister or party spokesperson makes a claim about immigration on the record.

**Step 1 -- Capture and verify (Claude Code, 5 minutes):**
```
[Politician] just said: "[exact quote]" in [context: Parliament/media/X].

Check this against:
1. Our data: What do the actual Stats NZ/MBIE numbers show?
2. Our accountability tracker: Has this politician or party said
   something different before? When? What exactly?
3. Our OIA findings: Do any of our OIA responses contradict this claim?

Give me: the claim, the data, any contradictions, and a
one-sentence verdict (accurate / misleading / false / unverifiable).
```

**Step 2 -- Draft response (Claude Code, 5 minutes):**
```
Draft a Reframe post for X/Twitter.

Structure:
- Quote the politician's exact words
- State what the data actually shows (with source)
- If there's a contradiction with a previous statement, surface it
- End with a question, not an accusation

Voice: How Many NZ. Not calling anyone a liar. Just showing the
data next to the claim and letting people draw their own conclusions.
```

**Step 3 -- Edit and publish (human, 15 minutes).**

**Total time: 25 minutes from statement to published fact-check.**

### Workflow 4: Weekly Wrap Production

**Trigger:** Every Friday.

**Step 1 -- Compile the week (Claude Code, 15 minutes):**
```
It's Friday. Compile the weekly wrap for How Many NZ.

Pull from:
1. This week's data releases (any new Stats NZ, MBIE, or RBNZ data)
2. This week's OIA responses received
3. This week's notable political statements about immigration
   (from Hansard monitoring and media monitoring)
4. This week's international comparison news (any migration policy
   changes in AU, CA, UK)
5. Any updates to our party scorecard

Format as a briefing: 3-4 main items, each with a headline and
2-3 sentences of context. Include one "under the radar" item that
didn't get media coverage.
```

**Step 2 -- Draft the wrap (Claude Code, 10 minutes):**
```
Using the compiled material, draft the Weekly Wrap post.

800-1200 words. Conversational but informed. The tone is "here's
what you need to know this week from someone who's been watching
the numbers." Open with the most significant item. Close with a
preview of what's coming next week (upcoming data releases,
OIA deadlines, select committee hearings).

Voice: How Many NZ at its most approachable. This is the
lowest-anger content in the mix.
```

**Step 3 -- Edit and publish (human, 30 minutes).**

**Total time: 55 minutes for a comprehensive weekly briefing.**

---

## 5. THE MONITORING SYSTEM

### What Gets Monitored

Five monitoring streams, each handled by a Python script running on a cron schedule.

**Stream 1: Parliamentary Hansard**
What: All references to immigration, migration, population growth, net migration, visa, and related terms in parliamentary debates and written questions.
How: Python script pulls new Hansard entries from parliament.nz (they publish within 24-48 hours of the sitting). Keyword search across the full text. New matches get logged to the accountability tracker database and trigger an alert.
Schedule: Daily at 8am during sitting weeks.
Alert: Push notification via ntfy.sh with the speaker, date, and a snippet of the relevant quote.

**Stream 2: Stats NZ and MBIE Data Releases**
What: New publications on the Stats NZ release calendar and MBIE data pages.
How: Python script checks the Stats NZ release calendar (published months in advance) and scrapes the MBIE data publications page for new entries.
Schedule: Daily at 7am.
Alert: Push notification on the day of a relevant release, plus a 3-day advance warning.

**Stream 3: Media Monitoring**
What: NZ Herald, Stuff, RNZ, Newsroom, Interest.co.nz articles matching immigration-related keywords. Also monitors for mentions of "How Many NZ" or @HowManyNZ.
How: RSS feed monitoring. NZ Herald, Stuff, and RNZ all provide RSS feeds. Python script checks feeds every 2 hours, filters by keywords, and flags new matches.
Schedule: Every 2 hours, 6am to 10pm.
Alert: Push notification for high-relevance matches (immigration in headline, or direct mention of the campaign). Daily digest email for lower-relevance matches.

**Stream 4: Politician Social Media (X/Twitter)**
What: Posts by key politicians and party accounts about immigration, population, housing, or infrastructure.
How: This is the trickiest to automate given X API costs. Practical approach: maintain a list of ~30 accounts (party leaders, immigration spokespeople, key backbenchers). Use a free or low-cost monitoring tool -- Nitter RSS feeds if available, or a simple script that checks public profiles periodically. Alternatively, use a free IFTTT or Zapier tier to monitor specific accounts for keyword matches.
Schedule: Every 4 hours.
Alert: Push notification for direct immigration statements by senior politicians.

**Stream 5: OIA Deadlines**
What: Upcoming OIA deadlines, overdue responses, and extension dates.
How: Queries the SQLite OIA database directly.
Schedule: Daily at 8am.
Alert: Notification for any request due in 5 days, any request overdue, and any extension expiring in 3 days.

### Alert Priority System

**Immediate alert (push notification):** Stats NZ quarterly migration data release. Major politician statement on immigration. Journalist mentions or tags the campaign. OIA response received.

**Same-day alert (morning digest):** New relevant media article. New Hansard entry with immigration keywords. OIA deadline approaching.

**Weekly summary (Friday morning):** All monitoring activity for the week. Compiled by Claude Code into a brief that feeds into the Weekly Wrap production.

### Build Approach

Claude Code writes all monitoring scripts in Python. Each script is standalone, does one thing, and outputs to a common alerts system. Total code is probably 500-800 lines across all five streams. Build time: 4-6 hours over the first two weeks.

The operator needs a machine that runs 24/7 (or close to it) for the cron jobs. Options: an old laptop, a Raspberry Pi, or the Oracle Cloud free tier (which provides a always-free ARM VM that's more than sufficient for these lightweight scripts).

---

## 6. THE ACCOUNTABILITY TRACKER

### Purpose

A system that logs every public statement every party and politician makes about immigration, population, and related infrastructure. Over time, it becomes a searchable record of who said what, when, and whether they followed through. It feeds the party scorecard and enables contradiction detection.

### Database Schema

```sql
CREATE TABLE politicians (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    party TEXT NOT NULL,
    role TEXT,                       -- PM, Minister of Immigration, spokesperson, etc.
    electorate TEXT,
    social_handles TEXT,             -- JSON: {"x": "@handle", "facebook": "url"}
    active INTEGER DEFAULT 1
);

CREATE TABLE statements (
    id INTEGER PRIMARY KEY,
    politician_id INTEGER NOT NULL REFERENCES politicians(id),
    date DATE NOT NULL,
    source_type TEXT NOT NULL,       -- hansard, media, social_media, press_release, event
    source_url TEXT,
    source_name TEXT,                -- e.g., "NZ Herald", "Parliament Hansard", "Q+A"
    quote TEXT NOT NULL,             -- Exact words
    context TEXT,                    -- What was the broader discussion
    topic_tags TEXT,                 -- JSON array: ["net_migration", "housing", "targets"]
    sentiment TEXT,                  -- pro_restriction, pro_status_quo, ambiguous, deflection
    contains_number INTEGER DEFAULT 0,  -- Did they state a specific target or figure?
    stated_number TEXT,              -- The actual number if they gave one
    contradicts_id INTEGER,          -- References another statement this contradicts
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE party_positions (
    id INTEGER PRIMARY KEY,
    party TEXT NOT NULL,
    topic TEXT NOT NULL,             -- net_migration_target, housing_link, infrastructure_plan, etc.
    position TEXT,                   -- Their stated position
    position_date DATE,
    evidence_statement_ids TEXT,     -- JSON array of statement IDs supporting this
    score TEXT,                      -- green, amber, red
    last_updated DATE
);

CREATE TABLE scorecard_history (
    id INTEGER PRIMARY KEY,
    date DATE NOT NULL,
    party TEXT NOT NULL,
    criteria TEXT NOT NULL,
    score TEXT NOT NULL,             -- green, amber, red
    justification TEXT NOT NULL,     -- One-sentence explanation
    evidence_url TEXT
);
```

### Scorecard Criteria

Six criteria, each scored green/amber/red:

1. **Net migration target**: Has the party stated a specific annual net migration number?
   - Green: Specific number published
   - Amber: Range or qualified position
   - Red: No stated target

2. **Housing-population link**: Does the party's housing policy account for migration-driven demand?
   - Green: Policy explicitly links housing targets to population projections
   - Amber: Acknowledges the connection but no specific planning
   - Red: Housing policy ignores migration entirely

3. **Infrastructure planning**: Does the party link infrastructure investment to population growth?
   - Green: Published plan connecting infrastructure spend to projected population
   - Amber: General statements about infrastructure without population linkage
   - Red: No connection made

4. **Transparency**: Has the party responded to How Many NZ's questions and/or published its own population analysis?
   - Green: Responded with specifics
   - Amber: Responded in general terms
   - Red: No response / refused to engage

5. **Independent review**: Does the party support an independent migration advisory body (similar to Australia's Centre for Population)?
   - Green: Explicit support
   - Amber: Open to the idea
   - Red: No position or opposed

6. **Migrant welfare**: Does the party address the impact of migration levels on migrants themselves (exploitation, housing conditions, access to services)?
   - Green: Specific policies addressing migrant welfare in context of volumes
   - Amber: General statements about migrant welfare
   - Red: No position

### Scorecard Visual Generation

Claude Code generates the scorecard as an HTML table on the dashboard and as a Canva-ready specification for the social media graphic. The social media version is a simple grid: parties across the top, criteria down the side, coloured circles (green/amber/red) in each cell.

The scorecard updates monthly or when a party makes a significant policy announcement. Each update is content: "Updated scorecard. [Party] moved from red to amber on [criterion] after [what they said/did]. [Other party] remains red across the board."

### Contradiction Detection

Claude Code's most valuable role in the accountability tracker is spotting contradictions. When a new statement is logged, Claude Code compares it against all previous statements by the same politician and party. If the new statement conflicts with a prior position, it flags it and drafts a contradiction post:

```
Prompt: "Here is a new statement by [politician] on [date]: '[quote]'.
Here are their previous statements on this topic: [list].
Does the new statement contradict any previous statements?
If yes, identify the specific contradiction and draft a short
X post showing both quotes side by side with dates."
```

---

## 7. BUILD PRIORITY AND TIMELINE

### The Honest Assessment

Before laying out the timeline, here's what I'd flag if I were reviewing this as a senior engineer.

The system described above is substantial. If you tried to build everything before doing anything else, you'd spend 3-4 weeks building and 0 weeks campaigning. That's backwards. The campaign needs to be live from day one, even if the tooling is rough. Build in parallel with operating, and let the tooling improve as you learn what actually matters.

Some of what's described above will turn out to be less useful than expected. The monitoring system sounds great but if you're checking X and the Herald manually every morning anyway (which you will be), the automated monitoring is a nice-to-have rather than a must-have in the early weeks. Build the minimum, start posting, and automate the pain points as they emerge.

### Week 1: Minimum Viable Tech Stack

**Day 1-2: Accounts and presence (3 hours)**
- Create @HowManyNZ accounts on X, TikTok, Facebook, Instagram
- Set up consistent profile images (text mark in navy), bios, and banner images
- Register howmany.nz domain (~$25)

**Day 2-3: OIA machine v1 (4 hours)**
- Claude Code creates the SQLite database with the schema above
- Claude Code writes the OIA request template and a simple Python script to calculate deadlines and track status
- Draft and send the first 10 OIA requests
- No fancy UI needed. A command-line Python script that adds requests and shows upcoming deadlines is sufficient.

**Day 3-4: Data foundation (4 hours)**
- Claude Code writes Python scripts to pull the key Stats NZ datasets (net migration, building consents, population estimates)
- Download the latest MBIE immigration data manually
- Build a local data folder with clean CSVs ready for analysis

**Day 4-5: First content (3 hours)**
- Claude Code helps draft the first 5 Stat Drop posts using the data foundation
- Create 2-3 basic data graphics in Canva using the brand template
- Post the first content

**Day 5-7: Content rhythm (ongoing)**
- Post daily. One Stat Drop per day for the first week.
- Engage with NZ political X -- reply to journalists' posts about housing and infrastructure with relevant data.

**Week 1 output:** Live social accounts posting daily. 10 OIA requests filed and tracked. Local data ready for analysis. Total build time: ~14 hours.

### Weeks 2-3: Dashboard and Monitoring

**Dashboard v1 (8 hours across week 2)**
- Claude Code builds the static site structure
- Landing page with headline net migration stat
- Housing gap page with the bar chart comparison
- Basic "About" page with methodology and sources
- Deploy to GitHub Pages at howmany.nz

**Monitoring v1 (3 hours across week 2)**
- Set up RSS feed monitoring for NZ Herald, Stuff, RNZ (Python + cron)
- Set up Stats NZ release calendar alerts
- Set up OIA deadline alerts from the database
- Push notifications via ntfy.sh

**Content templates (2 hours in week 3)**
- Claude Code builds the prompt templates for each content pillar
- Test the data-release-to-post workflow with historical data
- Build a small library of Claude Code prompts that can be reused

**Weeks 2-3 output:** Dashboard live with core visualisations. Automated monitoring for data releases and media. Content production workflow established. Total additional build time: ~13 hours.

### Month 1 (weeks 3-4): Accountability Infrastructure

**Accountability tracker v1 (4 hours)**
- Claude Code creates the database schema
- Backfill last 6 months of politician statements from Hansard (Claude Code does the extraction; human spot-checks)
- Build the contradiction detection prompt

**Party scorecard v1 (3 hours)**
- Research current party positions on the six criteria
- Populate the database
- Claude Code generates the first scorecard graphic
- Add the scorecard page to the dashboard

**OIA pipeline scaling (2 hours)**
- File second batch of 10 OIA requests based on gaps identified
- Refine the request template based on any acknowledgements or push-back from the first batch

**Month 1 output:** Full accountability tracker with historical data. Party scorecard live on the dashboard. 20 OIA requests active. Daily content rhythm established. Total additional build time: ~9 hours.

### Month 2: Automation and Refinement

**Dashboard v2 (4 hours)**
- Add regional breakdown page
- Add international comparison page
- Improve mobile responsiveness
- Add the OIA findings archive page

**Content engine refinement (3 hours)**
- Refine Claude Code prompts based on what's worked and what hasn't
- Build the weekly wrap production workflow
- Start video content production (TikTok/Reels)

**Monitoring improvements (2 hours)**
- Add Hansard monitoring if not already done
- Add politician social media monitoring (even if manual list-checking)
- Tune keyword filters to reduce noise

**OIA pipeline at scale (ongoing, 30 min/day)**
- Processing first responses as they arrive
- Filing follow-up requests
- Building the OIA-to-content pipeline

**Month 2 output:** Dashboard approaching complete. Content production partially automated. OIA responses generating exclusive content. Monitoring covering all five streams. Total additional build time: ~9 hours.

### Month 3: Optimisation and Scale

**Analytics (2 hours)**
- Deploy Umami or Plausible on the dashboard
- Start tracking which pages journalists visit
- Set up the Google Sheets tracking for social media metrics

**Scorecard automation (2 hours)**
- Automate the scorecard update process
- Build the visual generation pipeline so updates take 15 minutes instead of an hour

**OIA machine v2 (3 hours)**
- Add Ombudsman complaint templates
- Build the refusal tracking and escalation workflow
- Improve the response analysis prompts based on what's worked

**Content engine at full speed**
- All seven content pillars operational
- Reactive content workflow under 90 minutes from trigger to publication
- Weekly wrap production under 60 minutes

**Month 3 output:** All systems operational and refined. The operator spends most of their time on editorial decisions, journalist engagement, and public appearances rather than on building infrastructure. Total additional build time: ~7 hours.

### Total Build Time Summary

| Phase | Build Hours | Calendar Time |
|---|---|---|
| Week 1 (MVP) | 14 hours | 7 days |
| Weeks 2-3 (dashboard + monitoring) | 13 hours | 14 days |
| Month 1 remainder (accountability) | 9 hours | 7 days |
| Month 2 (refinement) | 9 hours | 30 days |
| Month 3 (optimisation) | 7 hours | 30 days |
| **Total** | **52 hours** | **3 months** |

That's roughly 4-5 hours per week of building, which leaves the majority of time for actually running the campaign: writing content, engaging on social media, processing OIA responses, attending events, and building journalist relationships.

### What to Cut If You're Falling Behind

If the build is taking longer than expected or the campaign demands are overwhelming, here's what to deprioritise in order of least impact:

1. **Analytics** (Plausible/Umami) -- nice to have, not essential. You'll know if the dashboard is working by whether journalists cite it.
2. **Politician social media monitoring** -- you're already on X looking at this manually. Automation saves 15 minutes a day at best.
3. **Regional breakdown page** on the dashboard -- useful but not critical in the first 3 months. National-level data makes the argument fine.
4. **International comparison page** on the dashboard -- the social media posts make this comparison already; a dedicated page is a luxury.
5. **Video content** -- if time is tight, text and graphics on X and Facebook are sufficient. Video is an amplifier, not a requirement.

What you do NOT cut: the OIA pipeline, the daily posting rhythm, and the party scorecard. These three things are the campaign's core engine. Everything else supports them.

---

## ENGINEERING REVIEW

I've gone through this document as a senior engineer asking one question: can one person actually build and maintain this with Claude Code?

**Verdict: Yes, with caveats.**

The caveats:

The Stats NZ API is real but occasionally poorly documented. Expect to spend an hour or two figuring out the exact query parameters for the datasets you need. Claude Code can help but may need to iterate based on actual API responses. Budget extra time in week 1 for this.

X/Twitter API access is the biggest technical risk. Free tier API access has been restricted repeatedly. The monitoring system's politician social media stream may need to rely on manual checking or workarounds (RSS via third-party services, periodic manual checks) rather than direct API access. This is annoying but not fatal. The campaign existed before automated monitoring existed.

The 52-hour total build estimate assumes Claude Code handles the coding efficiently, which it will for standard Python and HTML/JS work. If you hit an unusual API format or data quality issue, individual tasks could take 2-3x the estimate. Build in buffer.

SQLite is the right choice for this scale. Don't be tempted to use PostgreSQL, MongoDB, or anything that requires a running server. You are one person. Every dependency is a potential maintenance burden. SQLite is a single file you can back up by copying it.

GitHub Pages has soft limits on bandwidth (100GB/month) and repository size (1GB). A NZ political campaign dashboard will never come close to either limit. If the campaign somehow goes viral enough to exceed GitHub's limits, that's a good problem to have and you can migrate to Cloudflare Pages (also free) in an afternoon.

The content production time estimates assume familiarity with the workflow. The first time you process an OIA response through the full pipeline, it will take 2 hours, not 55 minutes. By the fifth time, it will take 40 minutes. The estimates reflect steady-state performance after the first 2-3 weeks.

One thing that's not in this document: backups. Copy your SQLite database to Google Drive or a USB stick once a week. Your OIA tracker and accountability database represent hundreds of hours of accumulated work. Losing them to a laptop failure would be devastating. A weekly manual backup takes 2 minutes and removes the risk entirely.

The system is designed to degrade gracefully. If the monitoring scripts break, you still have the OIA pipeline and the content engine. If the dashboard goes down, you still have social media. If Claude Code is unavailable for a day, you can post manually using the data and templates you've already built. Nothing is a single point of failure except the operator themselves, and that's an unavoidable constraint of a one-person campaign.

This stack is not elegant. It's a collection of scripts, a SQLite database, a static website, and a lot of Claude Code prompts held together by cron jobs and discipline. That's exactly what a zero-budget solo campaign should look like. Elegance is a luxury. Reliability and speed are necessities.

---

*Technical specification prepared March 2026. Designed for a solo operator with Claude Code, zero budget, and limited patience for systems that break.*
