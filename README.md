# 🤖 Autonomous AI Job Hunter
### 1. The Automation Architecture
<img width="956" height="346" alt="Screenshot 2026-10-01 090522" src="https://github.com/user-attachments/assets/8bb3eb2e-f3c9-4d65-9c69-7296e94ef461" />
### 2. The Final Output (Automated Database)
After the AI finishes analyzing and filtering the jobs in the background, all perfectly matched roles are automatically written and categorized directly into a Google Sheets tracking database:
<img width="1208" height="631" alt="image" src="https://github.com/user-attachments/assets/c7abb561-6e3b-4969-8ffa-4d5bf66b282a" />

An automated, dual-persona job recruitment pipeline built with n8n. This workflow scrapes live job boards, analyzes postings against multiple resumes simultaneously using AI, and automatically categorizes them into a tracking database.

## 🌟 Features
* **Automated Web Scraping**: Pulls the newest developer job postings from JobStreet three times a day via Apify.
* **Dual-Persona AI Analysis**: Uses Google Gemini to analyze each job description in parallel against two distinct resumes (Quality Assurance and Software Development) to determine exact role alignment.
* **Intelligent Rate Limiting**: Features a custom-built loop and delay architecture to strictly adhere to Google's Free Tier API rate limits (15 RPM) and prevent server timeouts.
* **Database Deduplication**: Automatically cross-references new jobs with a Google Sheets database to ensure no duplicates are added and existing "Fit" jobs are never overwritten by "Unfit" updates.
* **Zero-Touch Automation**: Runs completely autonomously in the background on a Cron schedule.

## 🛠️ Tech Stack
* **n8n**: Workflow automation and visual node orchestration
* **Apify**: Web scraping (JobStreet API)
* **Google Gemini API**: Large Language Model for resume alignment analysis
* **Google Cloud Workspace**: Google Drive API (PDF storage) and Google Sheets API (Database)

## ⚙️ Architecture & Data Flow
1. **Trigger**: A cron schedule fires at 8:00 AM, 12:00 PM, and 6:00 PM.
2. **Scrape & Fetch**: Triggers an Apify actor to scrape fresh jobs while simultaneously downloading PDF resumes from Google Drive.
3. **Parallel Pacing Loops**: Job data is passed into two separate pacing loops (10-second delays) to safely bypass LLM rate limits.
4. **AI Decision Engine**: Gemini reads the raw job description and acts as a strict filter, analyzing the text against the PDF resumes.
5. **Merge & Store**: Matches are automatically appended to a "Fit" Google Sheet. Rejections are passed through a deduplication Merge node before being appended to an "Unfit" sheet for historical tracking.
