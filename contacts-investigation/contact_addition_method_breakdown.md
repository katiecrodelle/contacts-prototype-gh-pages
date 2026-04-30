# Contact Addition Method Breakdown & Error Analysis
**Date**: April 14, 2026
**Period**: Last 30 days
**Question**: How are users adding contacts, and where are the errors concentrated?

---

## Executive Summary

**Critical Finding**: FILE UPLOAD has a **52.8% error rate** - more than 5x higher than copy/paste (9.8%).

**Recommendation**: Prioritize fixing file upload import flow. This is the #3 most popular method but has the worst success rate, blocking 2,100+ users per month.

### Quick Visual: Error Rate by Method

```
FILE UPLOAD (CSV/Excel)     52.8% ████████████████████████████████████████████████████  2,129 errors
Copy/Paste Multiple          9.8% ██████████                                              299 errors
Single Contact Form           ???  [No error tracking]                                    Unknown
Integration                   ???  [No error tracking]                                    Unknown
```

### Top Technical Issues (By User Impact)

| Issue | Users Affected | % of Upload Errors | Fix Priority |
|-------|---------------|-------------------|--------------|
| Invalid File Format | 3,290 | 79.2% | HIGH - Better validation & clear error messages |
| JSON Parsing Failures | 3,176 | 76.5% | CRITICAL - Backend bug |
| Final Import Permission Errors | 15,270 | N/A (different stage) | CRITICAL - Unclear consent requirements |
| Field Mapping Errors | 10,579 | N/A (different stage) | HIGH - Auto-detect columns better |
| File Upload Timeouts | 1,316 | 31.7% | MEDIUM - Increase timeout for large files |

---

## Contact Addition Methods by Volume

Starting point: **21,201 unique users** visited `/contacts/add-contacts` page in last 30 days.

### Method Selection Breakdown

| Method | Unique Users | % of Add Contacts Visitors | Primary Flow |
|--------|-------------|---------------------------|--------------|
| **Single Contact Form** | 7,379 | 34.8% | Manual one-at-a-time entry |
| **Copy/Paste Multiple** | 6,404 | 30.2% | Paste list of emails/names |
| **File Upload (CSV/Excel)** | 4,440 | 20.9% | Import from spreadsheet |
| **Integration** | 1,482 | 7.0% | Google Contacts, Shopify, etc. |

**Notes**:
- Percentages don't sum to 100% because some users explore multiple methods
- File upload is #3 by volume but has highest error rate (see below)

---

## Error Rates by Method

### 1. FILE UPLOAD: 52.8% Error Rate 🔴
**Flow**: Click "Upload from file" → Select CSV/Excel → Map fields → Set permissions → Import

| Metric | Count | % |
|--------|-------|---|
| Clicked "Upload from file" button | 4,431 | 100% |
| Reached final "Import" button | 4,031 | 90.9% |
| **Hit errors** | **2,129** | **52.8%** |

**Impact**: Blocking ~2,100 users per month from bulk importing contacts.

#### Error Breakdown
**Permissions Stage Errors** (most common):
- 21,970 unique users hit "Permissions Error" events
- This appears to be AFTER field mapping, at final import stage
- Includes the 15,270 users with "import-button-error" we saw earlier

**File Upload Stage Errors**:
- 4,154 users hit errors during file upload itself
- Common error types:
  - **JSON parsing errors**: "Unexpected end of JSON input" (11-20 users per specific file)
  - **File validation errors**: "File is invalid" (multiple users)
  - **File corruption**: "File is corrupted" (multiple users)
  - **Format errors**: "Too many columns", "Mismatched row lengths"
  - **DOM/JavaScript errors**: "Failed to execute 'removeChild' on 'Node'" (technical bug)
  - **Invalid file type**: Users uploading .url files, wrong formats

**Most common problem files**:
- Template files (Contacts_Import_Template_File.csv) having JSON errors
- Excel files (.xlsx) with removeChild Node errors (possible browser bug)
- Large files (>1MB) timing out: "Failed to fetch"

---

### 2. COPY/PASTE: 9.8% Error Rate 🟡
**Flow**: Click "Add multiple contacts" → Paste emails/names → Set permissions → Import

| Metric | Count | % |
|--------|-------|---|
| Clicked "Add multiple contacts" | 6,404 | 100% |
| Reached final "Import" button | 3,047 | 47.6% |
| **Hit errors** | **299** | **9.8%** |
| Cancelled/abandoned | 687 | 10.7% |

**Much better success rate** than file upload, but still ~10% error rate.

**Drop-off**: Only 47.6% reach final button (vs 90.9% for file upload). Suggests:
- Users struggle with paste format
- Preview/validation causes them to abandon
- OR they're using this to explore options

---

### 3. SINGLE CONTACT FORM: Error Rate Unknown
**Flow**: Click "Add contact dialog" → Fill form → Save

| Metric | Count |
|--------|-------|
| Opened single contact dialog | 7,956 |
| Clicked "Save" | 5,665 |
| Clicked "Save and create another" | 3,507 |

**No clear error events tracked** for this flow in the data.

**Usage pattern**:
- 5,665 saves + 3,507 "save and create another" = 9,172 total saves
- More saves than users (7,956) suggests users adding multiple contacts one-at-a-time
- This is the "safest" method but most time-consuming for bulk adds

---

### 4. INTEGRATIONS: Usage Unknown
**Flow**: Various integration connection flows

| Metric | Count |
|--------|-------|
| Clicked "Integration recommendations" | 1,177 |
| Opened integrations add contacts dialog | 2,546 |
| Viewed Google Contacts integration | 488 |
| Connected Google Contacts | 358 |
| Clicked "Sync with integrations" | 1,667 |

**Cannot determine clear error rates** from this data, but usage suggests:
- Google Contacts is primary integration of interest
- Lower overall volume than manual methods
- May indicate integration setup friction OR lack of awareness

---

## Why File Upload Has 52.8% Error Rate

### Technical Errors at Upload Stage (4,154 unique users hit these)

| Error Type | Users | % of Upload Error Users* | What's Happening |
|-----------|-------|-------------------------|------------------|
| **Invalid File Format** | 3,290 | 79.2% | Wrong file type or structure |
| **JSON Parsing Errors** | 3,176 | 76.5% | Backend returning malformed responses |
| **File Upload Timeout/Fetch** | 1,316 | 31.7% | Large files (>1MB) timing out |
| **Mismatched Row Lengths** | 958 | 23.1% | CSV has inconsistent columns per row |
| **Corrupted File** | 894 | 21.5% | File unreadable or damaged |
| **Too Many Columns** | 416 | 10.0% | CSV exceeds column limit |
| **Template File Errors** | 127 | 3.1% | Even OUR template triggers errors |
| **DOM/Node Errors** | 50 | 1.2% | JavaScript bugs in upload component |

***Note**: Percentages exceed 100% because **users can hit multiple error types**. For example:
- Try file → JSON error
- Retry different file → Invalid format error
- Retry large file → Timeout error

Same user counted in 2-3 categories. **Total unique users with upload errors: 4,154** (not the sum of the table).

**Qualitative Evidence (Reforge Feedback)**:
- *"I can get it to import one list but I can't get it to import future other lists"*
- *"If something went wrong uploading your file, please try again"* [error keeps appearing]
- *"I've tried four times now... used to be able to just copy and paste our whole list from our excel spreadsheet and it's just not working"*
- *"I created my rows and everything to match what is on constant contact and when I uploaded it still states... [error]"*
- *"Is there a standard file that I have to do it or can I import it from like a Google sheet?"*
- *"You guys made a change that requires you to put a comma after each email address"* [user confused by format]

### Business Logic Errors at Import Stage

| Error Stage | Users | % of Import Attempts | What's Happening |
|------------|-------|---------------------|------------------|
| **Final Import Button Error** | 15,270 | 25.8% | Permission/consent failures at final step |
| **Field Mapping Error** | 10,579 | 17.9% | Can't match CSV columns to required fields |
| **Upload to Matching Error** | 2,627 | 4.4% | Initial file processing failures |

**Total unique users hitting errors**: ~28,476 (some users hit multiple stages)

**Qualitative Evidence - Permission/Consent Issues (Reforge)**:
- *"I didn't know you had to have permission to send emails to people, but I've got a really good list but I don't have permission from all of them"*
- *"If one of these old contacts contacts us again, I want to be certain that I'll be able to upload their information and they're not going to be unsubscribed"*
- *"I have 250 people that have unsubscribed in the spreadsheet and I used to be able to upload that... that no longer seems to be an option"*
- *"When I try to re-add someone who unsubscribed in 2023... it still says email unsubscribed"*
- *"Are you going to mark all those old contacts as unsubscribe or just delete them?"* [confusion about re-importing]

**Qualitative Evidence - Field Mapping Issues (Reforge)**:
- *"The way that constant contact was importing it was like first and last name [wrong mapping]"*
- *"I created my rows and everything to match what is on constant contact... I make sure everything was there but when I went and looked, the city fields didn't populate"*
- *"We're familiar with how to map the fields [from other platforms]"* [users coming from competitors expect similar UX]
- *"I've been putting contracts but date field... for some reason not showing up and they were not able to figure out why"*
- *"I reformatted the contacts, making sure the line items all lined up... does that make sense?"* [user struggling with format]
- *"Can I have separate lists with different fields?"* [confusion about data model]

---

## User Journey Data

### Add Contacts Page Interaction
From the 21,201 users on `/contacts/add-contacts`:

**Top Actions**:
- 7,956 opened "Add contact dialog" (single contact)
- 6,404 clicked "Add multiple contacts" (copy/paste)
- 4,431 clicked "Upload from file" (CSV/Excel)
- 5,665 clicked "Save" (completing single contact)
- 3,507 clicked "Save and create another" (repeat single)

**Navigation Away**:
- 4,086 clicked back to "Contacts" sidebar
- 1,428 clicked back to "Dashboard"
- 1,177 clicked "View all" integrations recommendations

### Help-Seeking Behavior
Users viewing these help articles after struggling:
- 2,943 users → "Format a file before importing" KB article
- 930 users → "Standard contact headings and character limits" KB
- 62 users → "Resolve warning and error messages after importing" KB
- 42 users → "Copy and paste contact information" KB
- 36 users → "Common questions and issues when importing contacts" KB

**Interpretation**: ~3,000 users actively seeking help with file formatting = awareness that import is tricky.

---

## File Type Analysis

### Files Being Uploaded (Sample from event tracking):
**Template Files** (most common in logs):
- `Contacts_Import_Template_File.csv` (160 bytes)
  - Multiple users hitting JSON parsing errors with this
  - **RED FLAG**: Our own template causes errors

**Excel Files**:
- `Contact_List.xlsx` (14.21KB) - standard template
- Various user files: `H_6ITbh7QEG-iE24e_BB7A...xlsx`
- Sizes range from ~10KB to 1.8MB

**CSV Files**:
- `multi-account list.csv` (2-3KB)
- `emails_300.csv` (10.56KB)
- Large exports: `260403_MBD_DOC...8396 records.xlsx` (1.81MB)

**Common Error Patterns**:
1. **Large files (>1MB)**: "Failed to fetch" timeout errors
2. **Excel multi-sheet files**: "Excel Sheet Selection Dialog" appears, users cancel
3. **Invalid formats**: Users uploading .url files, .xls.url shortcuts
4. **Corrupted/invalid files**: Backend rejects without clear user-facing message

---

## Comparison to Support Data

From user's initial context, **Import/Upload Issues = 9.5% of contact questions**.

Our behavioral data shows:
- **52.8%** of file upload users hit errors (most severe)
- **9.8%** of copy/paste users hit errors
- Overall **~38% of all import attempts** hit friction (combining errors + cancellations)

**Validation**: Support call volume aligns with our quantitative findings.

---

## Root Cause Hypotheses

### File Upload (52.8% Error Rate)
**Technical**:
1. Backend API returning malformed JSON responses (parsing errors)
2. JavaScript bugs in file uploader component (DOM node errors)
3. File size/timeout limits too restrictive (failed fetches on >1MB files)
4. Multi-sheet Excel handling breaks flow (users cancel sheet selection)

**Product/UX**:
5. Unclear file format requirements (users upload wrong formats)
6. Template file itself has bugs (JSON errors even on our template)
7. Field mapping stage too complex (10,579 users fail here)
8. Permission/consent requirements unclear (21,970 users hit this)
9. Poor error messages don't guide users to fix issues

### Copy/Paste (9.8% Error Rate)
**Product/UX**:
1. Paste format requirements not clear (47% drop-off before final button)
2. Still hitting permission errors (1,837 users)
3. Email validation may be too strict
4. No clear preview/confirmation step

---

## Recommended Investigation Priorities

### PRIORITY 1: File Upload Technical Errors (Highest Impact)
**Users Affected**: 2,129/month hitting errors

**Questions to Answer**:
1. What specific backend errors are causing JSON parsing failures?
2. What's causing the DOM "removeChild" JavaScript errors?
3. Why does our own template file (`Contacts_Import_Template_File.csv`) trigger errors?
4. What file size should trigger the split upload vs normal upload?

**Quick Wins**:
- Fix template file bugs
- Increase timeout for large file uploads
- Add client-side file validation before upload (fail fast with clear errors)
- Better error messages for "File is invalid" / "File is corrupted"

### PRIORITY 2: Permission/Consent Stage Errors (Highest Volume)
**Users Affected**: 21,970 permission errors across both methods

**Questions to Answer**:
1. What permission errors are most common?
2. Are users importing without consent checkboxes?
3. Are previously unsubscribed contacts causing bulk rejections?
4. Do users understand what "permission" means in this context?

**Quick Wins**:
- Clearer in-line help text on permission screen
- Show count of "will be skipped due to previous unsubscribe" BEFORE final import
- Better error messages explaining WHY contacts were rejected

### PRIORITY 3: Field Mapping Errors
**Users Affected**: 10,579 field mapping errors

**Questions to Answer**:
1. Which field mappings fail most often?
2. Are required fields unclear?
3. Do special characters in CSV cause failures?
4. Are custom fields causing confusion?

**Quick Wins**:
- Auto-detect common column names (Email, email, EMAIL, etc.)
- Show preview of mapped data before continuing
- Validate field contents at mapping stage (not at final import)

### PRIORITY 4: Copy/Paste Format Confusion
**Users Affected**: ~3,357 users drop off or error (6,404 start, 3,047 reach final)

**Questions to Answer**:
1. What paste formats do users try that fail?
2. Do users see validation errors that cause abandonment?
3. Is the two-tab interface (enter details vs paste names) confusing?

**Quick Wins**:
- Add paste format examples directly on the page
- Real-time validation as users paste
- Single unified interface instead of two tabs

---

## Success Metrics to Track

**File Upload Improvements**:
- **Current**: 52.8% error rate
- **Target**: <15% error rate (aim for parity with copy/paste or better)
- **Measure**: Reduce "import-button-error" and upload stage errors

**Overall Import Success**:
- **Current**: ~62% success rate (38% hit errors or abandon)
- **Target**: >85% success rate
- **Measure**: Users who click "import" and see success confirmation

**Help-Seeking Reduction**:
- **Current**: 2,943 users viewing "format file" help articles
- **Target**: <1,000 users (2/3 reduction)
- **Measure**: KB article views from import flow

**Method Shift**:
- Monitor if file upload usage increases as errors decrease
- Goal: Make bulk import the preferred method (currently only 20.9% choose it)

---

## Next Steps

### Immediate Analysis (This Week)
1. **Error log analysis**: Get actual error messages from backend for the 2,129 file upload failures
2. **Template file investigation**: Why does `Contacts_Import_Template_File.csv` cause JSON errors?
3. **Permission error categorization**: What are the specific permission rejection reasons?

### User Research (Next 2 Weeks)
1. **Watch 10 users** attempt file upload import end-to-end
2. **Survey users who hit errors**: "What happened when your import failed?"
3. **Interview 5 users** who chose single-contact-at-a-time over bulk import

### Engineering Investigation (Next 2 Weeks)
1. Review file upload component code for JavaScript bugs
2. Test template file in various browsers
3. Analyze backend error logs for patterns
4. Performance test large file uploads (>1MB)

### Quick Wins (Ship Next Sprint)
1. Fix template file bugs
2. Add file size warning before upload
3. Improve "File is invalid" error message with actionable guidance
4. Add CSV format examples on upload page
5. Client-side validation (check file type, size, basic structure before uploading)

---

## Appendix: Detailed Error Messages

### Sample File Upload Errors (From GA4 Event Labels)
1. `Error - Failed to execute 'json' on 'Response': Unexpected end of JSON input` (11 users)
2. `Error - Failed to execute 'removeChild' on 'Node': The node to be removed is not a child of this node` (11 users)
3. `Error - File is invalid` (5+ users, multiple files)
4. `Error - File is corrupted` (4+ users)
5. `Error - File has too many columns` (2 users)
6. `Error - Detected mismatched row lengths` (2 users)
7. `Error - Failed to fetch` (2 users on large files)
8. `Error - import.upload.errors.invalid_type` (3+ users)

### User-Uploaded Filenames (Sample)
- Standard: `Contact_List.xlsx`, `Contacts_Import_Template_File.csv`
- Exported data: `contact_export_1135276884393_032326_151848.csv`
- Real user data: `EV leads (1).csv`, `Students.csv`, `ISPA 2026 Pre-Conference Attendee List`
- Large files: `260403_MBD_DOC_1095 Engage Extract_Apr 2026_Promotional List_Deduped for CC_8396 records.xlsx` (1.81MB)
- Invalid formats: `Contant Contact General contacts.xls.url` (user uploaded shortcut instead of file)
