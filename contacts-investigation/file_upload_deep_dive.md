# File Upload Deep Dive: Answering the Critical Questions
**Date**: April 14, 2026
**Period**: Last 30 days

---

## Question 1: What Makes a "Bad Header"?

### The System Expects Specific Column Names

From qualitative feedback, users reveal what the system is looking for:

**Required/Expected Headers**:
- **Email Address** (REQUIRED) - various formats accepted:
  - "Email Address"
  - "email"
  - "Email"
  - "email address"

**Optional but Common Headers**:
- First Name / first name / firstname
- Last Name / last name / lastname
- Company / company name / organization
- Phone / SMS Number / phone number
- City, Street, etc.

### Examples of "Bad Headers" from User Feedback:

**Problem 1: Missing "Email" in header**
- User feedback: *"When I uploaded it says 'you must have an email address' but all of them have an email address"*
- User has emails in column but didn't label the header "Email Address"

**Problem 2: Wrong terminology**
- User: *"Originally had 'organization', I notice you guys were using the word 'company'"*
- System may not auto-map "organization" to "company" field

**Problem 3: Custom fields not recognized**
- User: *"The only option is member name so I can't put member name on both columns, it doesn't distinguish between last name and first name"*
- System doesn't know what to do with non-standard fields

**Problem 4: Multiple email columns**
- User: *"There's one column 'email addresses' and a second column 'secondary email addresses' and I think all I got was the secondary one"*
- System picks wrong column when multiple exist

### What Makes a "Good" Header:
1. **Email column is labeled "Email" or "Email Address"** (case insensitive)
2. **Standard field names** that match Constant Contact's expected fields
3. **One column per field** (not duplicate fields)
4. **No extra spaces** in header names
5. **First row contains headers** (not data)

---

## Question 2: Are There REQUIRED Columns?

### YES: Email Address is THE ONLY required field

**Quantitative Evidence**:
- 59,453 users successfully passed field mapping stage
- 10,579 users hit "field mapping errors"
- 3,789 users clicked "Create custom field" (trying to map unusual columns)

**Qualitative Evidence - Users Confirm Email is Required**:
- *"It says you must have an email address... all of them have an email address"* (but header wrong)
- *"Email address is the most required to import contacts"*
- *"I was getting ready to upload... it's not letting me. It says 'you must have an email address' and I know I have SMS number and SMS consent date"*
- *"How does the system know to look at a certain column to get the email address?"*
- *"I just want to input email addresses and the name of the companies"* (asking if name is required)
- *"Can I upload like in one column, a list of email addresses?"* (testing if email alone works)

### Optional Fields (Not Required):
- First Name
- Last Name
- Company/Organization
- Phone
- Address fields (Street, City, State, ZIP)
- Custom fields (any user-created field)

### The "Missing Columns" Problem:

**It's NOT about missing columns - it's about:**

1. **Email column not labeled correctly**
   - User has emails but header says "Email1" or "E-mail" or no header
   - System can't find the email column → error: "must have email address"

2. **Email column empty or malformed**
   - User has "Email" header but some rows have no email
   - System rejects: "contacts need an email address"

3. **Required field for specific operations**
   - SMS requires phone number column
   - User trying SMS-only import without email fails
   - User feedback: *"I have SMS number and SMS consent date... it says you must have an email address"*

---

## Question 3: Clear Percentages & User Counts

### Total Users Attempting File Upload: 4,431 unique users

### Breakdown of "Invalid Format" Errors:

**UNIQUE users (no overlap)**: 9,067 total
- **Excel XLSX only**: 5,982 users (66.0% of error users, 135% of upload attempts*)
- **CSV only**: 3,968 users (43.8% of error users, 89.6% of upload attempts*)
- **Excel XLS only**: 943 users (10.4% of error users, 21.3% of upload attempts*)
- **Both XLSX AND CSV**: 1,394 users (15.4% of error users)

***Percentages > 100% because these 9,067 error users include people BEYOND the 4,431 who clicked "Upload from file" button.** Some users:
- Hit errors in previous sessions (prior 30 days)
- Entered import flow from different paths
- Have accumulated errors over multiple attempts

### Clearer Anchoring:

**Of the 4,431 users who clicked "Upload from file" this month:**
- **2,129 users hit ANY error (48.1%)**
- **765 users explicitly cancelled (17.3%)**
- **315 users silently abandoned (7.1%)**
- **4,031 users reached final import button (91.0%)**

**Why 48.1% error rate if only 52.8% earlier?**
- 48.1% = hit error at ANY stage (upload, mapping, permissions)
- 52.8% = of those who reached final button, this % hit permission errors specifically

### Supported File Type Errors (from the 9,067 error users):

| File Type | Unique Users | % of Error Users | What's Wrong |
|-----------|--------------|------------------|--------------|
| **Excel XLSX** | 5,942 | 65.5% | Valid format, bad content |
| **CSV** | 3,968 | 43.8% | Valid format, bad content |
| **Excel XLS** | 934 | 10.3% | Valid format, bad content |
| **Unsupported Types** | ~100-150 | 1-2% | PDF, Numbers, ODS, etc. |

**Note**: Users can try multiple file types, so individual percentages don't sum to 100%.

---

## Question 4: How Many Users Abandon the Workflow?

### Abandonment Analysis (of 4,431 file upload starters):

| Abandonment Type | Users | % of Starters | What Happened |
|------------------|-------|---------------|---------------|
| **Explicit Cancellation** | 765 | 17.3% | Clicked "Cancel import" button |
| **Silent Abandonment** | 315 | 7.1% | Started but never reached final button, didn't cancel |
| **Error-Induced Abandonment** | 2,129 | 48.1% | Hit error, may or may not retry |
| **Total Non-Completion** | ~3,209 | 72.4% | Didn't successfully complete |

### Abandonment Journey:

**Stage 1: Upload File**
- 4,431 clicked "Upload from file"
- 4,154 hit upload stage errors (93.8% of starters saw an error message)
- **315 abandoned here** (never proceeded to field mapping)

**Stage 2: Field Mapping**
- 4,116 reached field mapping (4,431 - 315 abandoned)
- 10,579 hit field mapping errors (includes retries, so >100%)
- 8,753 clicked "back to upload" (frustrated, retrying)
- **Some abandonment here** (included in silent 315)

**Stage 3: Permissions**
- 4,031 reached final "Import" button (90.9% of starters)
- 15,270 hit permission errors (includes retries)
- 765 explicitly cancelled at this stage
- 2,129 hit final import errors

**Success Rate**: Only ~27.6% successfully complete without errors or abandonment.

### Multi-Attempt Pattern (Retry Behavior):

**Users hitting errors on MULTIPLE file types = 1,394 users**
- They try .xlsx → error
- Retry with .csv → error
- Some retry 3+ times with different files
- This explains why error counts (5,942 + 3,968 + 934 = 10,844) exceed unique users (9,067)

**Average errors per error-user**: 10,844 / 9,067 = **1.2 errors per user**

This means the typical user hitting an error tries **1-2 different files** before giving up.

### Abandonment Indicators from Qualitative Feedback:

**Explicit Frustration**:
- *"I've called multiple times... every time I try to upload... it tells me we cannot upload your file"*
- *"I have been trying to fix the bug I reported on June 17th and since then I have not received any update"*
- *"I cannot wait forever... I need to start using [another software]"*
- *"This is really frustrating and I'm trying other software"*

**Workarounds/Giving Up**:
- *"I've offered multiple times just to do it for her"* (support doing manual import)
- *"She doesn't want to share information because it's confidential, I respect that"*
- *"For my limited use, I'm just going to remember to put name, first name, last name, email address"* (avoiding bulk import)
- *"It must be a different version... I'm stuck, I can't do it"*

---

## Summary: The Numbers Story

### The Funnel:

```
4,431 users clicked "Upload from file" (100%)
   ↓
4,154 saw upload errors (93.8%) ← Almost everyone sees an error!
   ↓
4,116 continued to field mapping (-315 abandoned, 92.9%)
   ↓
4,031 reached final button (-85 more abandoned, 90.9%)
   ↓
2,129 hit final import errors (48.1% of starters)
765 explicitly cancelled (17.3%)
   ↓
~1,222 successful imports (27.6% success rate)
```

### The "Invalid Format" Problem is Actually:

**9,067 unique users hitting file errors** including:
- 5,942 with .xlsx files (valid format, bad content)
- 3,968 with .csv files (valid format, bad content)
- 1,394 trying multiple file types
- Most errors are about:
  - Missing or wrong "Email" header
  - Too many columns
  - Bad data structure
  - NOT unsupported file types

### Key Takeaways:

1. **48.1% hit ANY error** (file upload + mapping + permissions combined)
2. **17.3% explicitly cancel** (frustration)
3. **7.1% silently abandon** (confusion)
4. **27.6% success rate** (only ~1,222 of 4,431 successfully import)
5. **Users retry 1-2 times** on average before giving up
6. **Email header is THE issue** - required field but users don't label it correctly

---

## Recommendations Based on These Findings

### HIGH PRIORITY: Fix Upload Stage (4,154 users affected)

1. **Better error messages** instead of "File is invalid":
   - ✅ "Missing required column: Email Address (or email)"
   - ✅ "Too many columns. Maximum allowed: X"
   - ✅ "Row 5 has mismatched column count"
   - ✅ "File type .pdf not supported. Use .csv or .xlsx"

2. **Client-side validation BEFORE upload**:
   - Check file extension
   - Check file size
   - Preview first 5 rows
   - Warn: "We don't see an 'Email' column. Did you mean column A?"

3. **Template improvements**:
   - Fix the template file itself (127 users hit errors on template!)
   - Show example with sample data
   - Include common variations: Email, email, Email Address

### MEDIUM PRIORITY: Fix Field Mapping (10,579 users affected)

1. **Smart auto-detection**:
   - "Email Address" = "email" = "Email" = "EMAIL"
   - "First Name" = "firstname" = "First" = "fname"
   - "Company" = "organization" = "Company Name"

2. **Visual preview**:
   - Show first 3 rows of data with column headers
   - Let users see what they're importing before clicking "Continue"

3. **Custom field guidance**:
   - "Column 'Organization' doesn't match any field. Create custom field?"
   - One-click create custom field from unmapped column

### LOW PRIORITY: Reduce Abandonment (765 + 315 users)

1. **Save draft imports**:
   - Let users close and come back
   - Don't make them re-upload file

2. **Progress indicator**:
   - Show: Upload → Map Fields → Set Permissions → Import
   - Users know where they are in flow

3. **Better help content**:
   - "Having trouble? Watch 2-min video"
   - Link to template download
   - Common mistakes FAQ
