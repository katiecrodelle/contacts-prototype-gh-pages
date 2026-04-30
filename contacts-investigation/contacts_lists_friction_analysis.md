# Contacts/Lists Workflow Friction Analysis
**Date**: April 14, 2026
**Analysis**: Qualitative (Reforge) + Quantitative (Snowflake) validation
**Goal**: Identify high-impact problems where data sources align

---

## Executive Summary

**Critical Finding**: Multiple friction points validated by BOTH qualitative complaints AND quantitative behavioral data, with highest impact on active email senders (27% import error rate).

**Top Problems by Data Convergence**:
1. **Import/Upload Failures** - Highest volume + clear behavioral signal
2. **List/Segment Confusion** - Heavy navigation bouncing + qualitative complaints
3. **Finding Contacts Within Lists** - 33K users clicking "view contact lists" + top qualitative theme
4. **List Creation Discoverability** - 38K users opening dialog but clear confusion in feedback

---

## 1. IMPORT/UPLOAD FAILURES
### Qualitative Signal (Reforge)
- **Volume**: 1,773 feedback snippets (11.8% of 15,000 contacts-related feedback)
- **Secondary Theme**: 1,009 snippets with "can't/unable to/not working" language
- **Pain Point Intensity**: Users report multiple retry attempts, giving up, requesting manual help

**Sample Feedback**:
- "I can get it to import one list but I can't get it to import future other lists"
- "I've followed those steps and have re-uploaded my list several times. The issue still persists"
- "The file upload...please try again [error keeps appearing]"
- "I need help...I've got all my profile details but can't upload the contacts"

### Quantitative Signal (Snowflake - Last 30 Days)
- **Total Import Attempts**: 59,120 unique users clicked final "Import" button
- **Import Errors**: 15,270 unique users (25.8%) hit import-button-error
- **Field Mapping Errors**: 10,579 additional users (17.9%) failed at field mapping stage
- **Upload Stage Errors**: 2,627 users (4.4%) failed at initial upload
- **User Cancellations**: 8,968 users abandoned the flow

**Combined Error Rate**: ~38% of users attempting import hit friction (errors or gave up)

#### Impact on Growing Users
- **Active Email Senders**: 42,384 attempted imports
  - **27.14% error rate** (11,504 users hit errors)
- **Non-Email Senders**: 16,736 attempted imports
  - 21.93% error rate (3,671 users hit errors)

**Business Impact**: We're actively blocking 11.5K engaged, email-sending users per month from growing their contact base.

---

## 2. LIST/SEGMENT CONFUSION & NAVIGATION FRICTION
### Qualitative Signal (Reforge)
- **List Management**: 648 snippets about creating/finding lists
- **Segments**: 504 snippets about segments
- **Search/Find**: 313 snippets about finding contacts/lists

**Sample Feedback**:
- "I don't really understand the difference between segments and tags"
- "Where is the create a new list button"
- "Can I have one master list and then divide the group into two lists?"
- "All I see is a list of all contacts. I would like to see each list separately"
- "Because candidly, you can't create a list very easily as a user"

### Quantitative Signal (Snowflake - Last 30 Days)
**Navigation Bouncing Pattern** (users searching for features):
- **From /contacts page**:
  - 42,375 users → Lists and Segments sidebar
  - 44,076 users → Audience sidebar
  - 42,464 users → back to Contacts sidebar
- **From /contacts/lists page**:
  - 24,569 users → back to Contacts sidebar
  - 6,787 users → Lists and Segments (already there!)
  - 5,803 users → Audience sidebar

**Total Navigation Churn**: 166K cross-navigation events from users bouncing between sections

**List Creation Activity**:
- 37,936 users opened "Create list dialog"
- 30,518 users selected "static list"
- 5,867 users selected "segment"
- **Gap**: ~7K users (19%) opened dialog but didn't select type → confusion/abandonment?

---

## 3. VIEWING CONTACTS WITHIN SPECIFIC LISTS
### Qualitative Signal (Reforge)
**Direct quotes validating this need**:
- "All I see is a list of all contacts. I would like to see each list separately. how do I do that"
- "Is there a way to filter contacts by location"
- "Can I search my contacts for duplicates?"

### Quantitative Signal (Snowflake - Last 30 Days)
- **33,183 unique users** clicked "View contact lists" button on /contacts page
- **103,797 total clicks** = 3.1 clicks per user average
- **94,221 unique users** used contact search (1.5M total searches)
- **12,438 users** used "Bulk add contact lists" function

**Interpretation**: High volume of users trying to:
1. See which lists a contact belongs to
2. Filter/view contacts by specific list membership
3. Manage list membership in bulk

---

## 4. LIST MANAGEMENT OPERATIONS
### Qualitative Signal (Reforge)
- **Export**: 340 snippets about exporting contacts
- **Delete/Remove**: 266 snippets about deleting contacts/lists
- **Duplicates**: 107 snippets about finding/managing duplicates

**Sample Feedback**:
- "I want to export all my contacts to excel"
- "I need to delete a list of contacts"
- "Delete bounced email addresses from list"
- "Can you search my contacts for duplicates?"

### Quantitative Signal (Snowflake - Last 30 Days)
**Export Activity**:
- 10,603 users viewed import/export activity
- 7,865 users clicked "Export action" (31.6K clicks)
- 7,316 users clicked "Export contacts" (29.3K clicks)
- Export volume indicates this is a regular workflow need

**Delete Activity**:
- 23,251 users used "Bulk delete contacts" (132K clicks)
- 9,994 users clicked "Delete list action"
- 9,401 users confirmed list deletion
- 4,409 users needed to "Restore deleted contact" (recovery feature usage suggests accidental deletions?)

**Duplicate/Merge Activity**:
- 2,969 users clicked "Merge action" (10.5K clicks)
- 1,280 users saved duplicated lists
- 1,272+ users performed bulk list merges

---

## 5. SEGMENT ADOPTION & UNDERSTANDING
### Qualitative Signal (Reforge)
- **504 snippets** mention segments
- Confusion about segment vs. list distinction
- Users asking how to create segments for specific criteria

**Sample Feedback**:
- "How can I create a new segmented list that is just my Texas customers and in the swanhaven existing segment?"
- "I don't really understand the difference between segments and tags"
- Users requesting location-based, engagement-based filtering

### Quantitative Signal (Snowflake - Last 30 Days)
**Segment Creation**:
- 5,867 users selected "Create list: segment" option
- **vs. 30,518 users selected static lists** (5.2x more)
- Segment adoption is only 16% of list creation activity

**Interpretation**: Low segment adoption could indicate:
1. Users don't understand what segments do
2. Segments don't meet user needs
3. Poor discoverability/education on segment benefits

---

## 6. MULTIPLE LISTS / EMAIL DELIVERY CONCERNS
### Qualitative Signal (Reforge)
- **53 snippets** about contacts in multiple lists
- Specific concern: "If a contact is in multiple lists will they get multiple emails?"

### Quantitative Signal (Snowflake - Last 30 Days)
**List Membership Actions**:
- 12,438 users used "Bulk add contact lists"
- 3,339 users used "Bulk remove contact lists"
- 33,183 users clicked "View contact lists" (checking which lists contacts belong to)

**Interpretation**: Users actively managing contacts across multiple lists, but qualitative data shows confusion about email delivery implications.

---

## DATA QUALITY NOTES

### Qualitative (Reforge)
- **Sample Size**: 15,000 contact/list-related feedback snippets
- **Date Range**: Not specified in query, appears historical
- **Source**: Mixed (AI assistant, support calls based on examples)
- **Limitation**: Represents users who encountered problems severe enough to seek help

### Quantitative (Snowflake)
- **Date Range**: Last 30 days (March 15 - April 14, 2026)
- **Sample Size**: 174K+ unique users on /contacts page
- **Metrics**: GA4 events (page_view, click, scroll, user_engagement)
- **Limitation**: Behavioral data doesn't capture *why* users took actions

---

## HIGH-IMPACT PROBLEMS TO SOLVE (Ranked by Data Convergence)

### TIER 1: Clear Volume + Clear Friction
**1. Import Failures at Final Stage**
- **Qual**: 1,773 import complaints + 1,009 "can't/won't work" mentions
- **Quant**: 15,270 users (25.8%) hit final import errors
- **Impact**: Blocking 11.5K active email senders/month from growth
- **Business Goal**: Directly impacts adoption/retention goals

**2. Finding "Create List" Functionality**
- **Qual**: 648 snippets about list creation confusion, users can't find button
- **Quant**: 37,936 opened dialog but clear drop-off and navigation patterns suggest discovery issues
- **Impact**: Users with intent to organize contacts are getting blocked

**3. Viewing Contacts by List**
- **Qual**: Direct user requests "I want to see each list separately"
- **Quant**: 33,183 users clicking "View contact lists" (3.1 avg clicks per user)
- **Impact**: Core workflow need for users managing segmented audiences

### TIER 2: Moderate Volume + Clear Confusion
**4. List vs. Segment Understanding**
- **Qual**: 504 segment mentions, confusion about difference from lists
- **Quant**: Only 16% of list creation uses segments, heavy navigation bouncing (166K events)
- **Impact**: Potential adoption barrier for power feature

**5. Import Field Mapping Errors**
- **Qual**: CSV/spreadsheet upload complaints
- **Quant**: 10,579 users (17.9%) fail at field mapping stage
- **Impact**: Second-highest error point in import flow

**6. Multiple Lists Email Delivery Confusion**
- **Qual**: 53 snippets, specific concern about duplicate sends
- **Quant**: 12,438 users managing bulk list membership
- **Impact**: User anxiety may limit list usage

### TIER 3: Lower Volume but Clear User Need
**7. Duplicate Contact Management**
- **Qual**: 107 snippets requesting duplicate search/cleanup
- **Quant**: 2,969 users using merge actions (10.5K clicks)
- **Impact**: Data quality concern for engaged users

**8. Contact Export Workflow**
- **Qual**: 340 snippets requesting export functionality
- **Quant**: 7,865 users exporting (31.6K export actions)
- **Impact**: Regular workflow need, but usage suggests it's being found/used

---

## RECOMMENDED NEXT STEPS

### Immediate Analysis
1. **Import Error Root Cause**: Query error logs/reasons for 15,270 import failures
   - What specific errors are users hitting?
   - Are errors concentrated in specific file types, sizes, or data formats?

2. **Navigation Flow Analysis**: Map typical user journey patterns
   - What are users looking for when they bounce between Contacts/Lists/Audience?
   - Can we instrument more specific click tracking for "looking for X" behaviors?

3. **Field Mapping Failure Analysis**: Why do 10,579 users fail at field mapping?
   - Common field mapping mistakes?
   - Confusing UI elements?

### Research Opportunities
1. **User Testing**: Watch users attempt to:
   - Import a contact list from CSV
   - Create a new list and add contacts to it
   - Find contacts belonging to a specific list
   - Understand when to use segment vs. static list

2. **Survey Active Email Senders**: The 11.5K who hit import errors
   - What were they trying to import?
   - What happened when it failed?
   - Did they find a workaround or give up?

### Product Hypotheses to Test
1. **Import Success Rate**: Can we reduce errors from 25.8% → <10%?
   - Better file validation before upload
   - Clearer error messages with actionable fixes
   - Common format templates/examples

2. **List Creation Discoverability**: Can we reduce time-to-first-list?
   - More prominent "Create list" CTA on contacts page
   - Onboarding flow for new users

3. **Contacts-by-List View**: Do users want filtered contact view by list?
   - Prototype: Add "Filter by list" to /contacts page
   - Measure: Time to find specific contacts, user satisfaction

4. **Segment Education**: Can we increase segment adoption from 16% → 30%?
   - In-app tooltips explaining segment benefits
   - Suggest segment creation for common use cases (e.g., "engaged contacts", "recent signups")

---

## MEASUREMENT PLAN

**Success Metrics**:
- **Import Success Rate**: 74.2% → 90%+ (reduce errors from 25.8% to <10%)
- **Active Email Sender Import Errors**: 27.14% → <15%
- **Time to First Successful Import**: Measure and reduce by 30%
- **List Creation Completion Rate**: Measure dialog open → list created conversion
- **Navigation Efficiency**: Reduce cross-section navigation events by 25%
- **Segment Adoption**: 16% → 25% of list creation uses segments

**Leading Indicators**:
- Reduced "import-button-error" events
- Reduced "cancelled-import" events
- Increased "View contact lists" success (fewer repeated clicks)
- Reduced navigation sidebar bouncing patterns
- Increased contact search usage with filters

---

## APPENDIX: FRICTION BY SUPPORT TOPIC (User-Provided)

From user's initial context:
```
Import/Upload Issues     9.5%
Add Contact              6.6%
Create New List          6.4%
Export Contacts          6.3%
Segments                 4.9%
Delete/Remove Contact    3.4%
Find/Search Contacts     3.2%
```

**Analysis Note**: Support volume percentages align with our findings. Import/Upload (9.5%) is #1, and our quantitative data shows 25.8% error rate, validating this as highest-impact problem.

1. What Makes a "Bad Header"?

  The system REQUIRES "Email" or "Email Address" as a column header (case-insensitive). Users fail when they:
  - Don't label the header at all
  - Use variations like "Email1", "E-mail", "Emails"
  - Use "Secondary Email" but not "Email"

  Other common header issues:
  - "Organization" doesn't map to "Company"
  - "Member Name" doesn't split into First/Last Name
  - Multiple email columns confuse the system

  2. Are There REQUIRED Columns?

  YES - Only ONE: Email Address

  Confirmed by user quotes:
  - "It says you must have an email address"
  - "Email address is the most required to import"
  - "I have SMS number... but it says you must have an email address"

  "Missing columns" actually means: The system can't FIND the email column because the header isn't labeled "Email"
   or "Email Address"

  3. Clear Percentages

  Total Users Attempting Upload: 4,431 unique users

  Error Users: 9,067 unique (NOT the same as 10,844)
  - These 9,067 are unique people
  - The 10,844 is event count (users trying multiple files)
  - ~1,394 users tried 2+ file types (XLSX then CSV)
  - Average: 1.2 attempts per error-user

  Breakdown by file type (users overlap):
  - XLSX only: 5,982 users (66% of error users)
  - CSV only: 3,968 users (44% of error users)
  - XLS only: 943 users (10% of error users)
  - Both XLSX & CSV: 1,394 users (trying multiple formats)

  Anchored to 4,431 upload attempts:
  - 48.1% hit ANY error (2,129 users)
  - 17.3% explicitly cancelled (765 users)
  - 7.1% silently abandoned (315 users)
  - Only 27.6% success rate!

  4. Abandonment

  Total Abandonment: 72.4% of users don't successfully complete

  ┌───────────────────────┬────────┬───────┐
  │         Type          │ Users  │   %   │
  ├───────────────────────┼────────┼───────┤
  │ Explicit cancellation │ 765    │ 17.3% │
  ├───────────────────────┼────────┼───────┤
  │ Silent abandonment    │ 315    │ 7.1%  │
  ├───────────────────────┼────────┼───────┤
  │ Error-induced         │ 2,129  │ 48.1% │
  ├───────────────────────┼────────┼───────┤
  │ Success               │ ~1,222 │ 27.6% │
  └───────────────────────┴────────┴───────┘

  The typical journey:
  - Try .xlsx → error
  - Retry with .csv → error
  - Give up or call support

  Critical finding: 93.8% see an error message at some point (4,154 of 4,431). Almost EVERYONE who tries file
  upload hits friction!
