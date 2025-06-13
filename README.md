# 📸 Google Sheets to Instagram Carousel Post Automation via Locally Hosted n8n

## Overview

This n8n workflow reads product data from a **Google Sheet**, generates a professional **Instagram carousel post caption using AI**, uploads up to 4 images, and publishes the post via the **Facebook Graph API** to your linked **Instagram Business Account**.

It also marks the post as `"uploaded"` in the sheet to avoid duplicates.

---

## 🔁 Trigger

- **Schedule Trigger**: Runs every 2 hours by default (can be changed to fit your posting frequency).

---

## 📋 Step-by-Step Flow

### 1. Google Sheets – "Is Uploaded?" Filter

- Connects to your product sheet.
- Filters rows where the `uploaded?` column is set to `"no"`.
- Returns **only 1 row** at a time to prevent batch duplicates.

### 2. Gemini AI Agent

- Uses Google’s **Gemini 1.5 Flash** (free) to generate professional captions.
- Captions include:
  - Emojis
  - Hashtags
  - Pricing callout
  - Soft CTA prompting users to "Check our bio for the link" (no direct URLs in post)

### 3. Edit Fields

- Prepares and formats:
  - Product title, description, and price
  - Image URLs from `mainimage`, `carousel1`, `carousel2`, `carousel3`
  - Gemini-generated caption
  - Instagram Page ID

### 4. Image Uploads (Flexible)

- Supports **1 to 4 image URLs** from the Google Sheet.
- Each upload is handled by a **separate Facebook Graph API node**.
- Upload is **conditionally skipped** if the image URL is empty.
- This makes the workflow dynamic — it will still post even if only 1 or 2 images are available.
- Uploaded image container IDs are collected for use in the next step.

### 5. Merge + Prepare Carousel Media IDs

- Merges successful image uploads into one dataset.
- Outputs image media IDs in a `children` field like:

```json
{
  "children": "17932447635024904,17872489326361281"
}
```

### 6. Create Carousel Container

- Sends the `children` media IDs + caption to the Graph API.
- This creates the Instagram Carousel Container tied to your business account.

### 7. Post to Instagram

- Final API call publishes the carousel post to your Instagram feed.

### 8. Google Sheets – Mark as Uploaded

- Updates the `uploaded?` column of the posted row to `"yes"` to prevent reposting.

---

## 📄 Google Sheets Format

The spreadsheet should look like this:

| ID | title | description | price | sale_price | mainimage | carousel1 | carousel2 | carousel3 | uploaded? |
|----|-------|-------------|-------|------------|-----------|-----------|-----------|-----------|-----------|
| 1  | Product Name | Short description | 500 | 400 | img1.jpg | img2.jpg | img3.jpg | img4.jpg | no |

---

## 🤖 Gemini API (Free AI Caption Generator)

This project uses Google’s **Gemini 1.5 Flash model** — a free, fast, and powerful AI caption generator.

To use it:

1. Visit the official Gemini API key page:  
   https://ai.google.dev/gemini-api/docs/api-key

2. Click “Get API Key” and sign in with your Google account.

3. Paste the generated API key into your n8n Gemini credential.

Captions are automatically created for each post!

---

## 📺 Facebook/Instagram API Setup (Video Guide)

If you're new to Meta’s Graph API, this guide is highly recommended:

**Watch here:**  
https://www.youtube.com/watch?v=AGSyWdjN5A4&ab_channel=Let%27sAutomateIt

It covers:
- Creating a Meta Developer App
- Setting up permissions
- Access token generation
- Linking Instagram and Facebook Business accounts

---

### 🔎 How to Find Your Instagram Page ID (Node)

To find the value required for the `Node` field inside your **Create Carousel Container** step:

1. Go to the Meta Developer Portal:  
   👉 https://developers.facebook.com/apps/

2. Select your App or create a new one.

3. Navigate to https://developers.facebook.com/apps/your-app-id/instagram-business/API-Setup/

---

## ⚠️ Disclaimer

This was a quick fix for a specific workflow need.  
It may not be the most scalable or universal solution — but it **works reliably for our use case**.  
Feel free to fork, improve, or adapt it to your setup.

---

⭐ Star the repo if this helped!

---

## Support Development

If you find this project useful and would like to support further development, you can donate Bitcoin to the following address:

Bitcoin Address: `17Uv9ZgoKFXdi18PNf5UighASk53KMjzxp`

Your contributions will be greatly appreciated and will help in maintaining and improving this project. Thank you for your support!
