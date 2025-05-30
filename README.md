# 📸 Google Sheets to Instagram Carousel Post Automation

## Overview
This n8n workflow reads product data from a **Google Sheet**, generates a professional **Instagram carousel post caption using AI**, uploads up to 4 images, and publishes the post via the **Facebook Graph API** to your linked **Instagram business account**.

It also marks the post as "uploaded" in the sheet to avoid duplicates.

---

## 🔁 Trigger

- **Schedule Trigger**: Runs every 2 hours by default (you can change the interval).

---

## 📋 Step-by-Step Flow

1. ### Google Sheets - "Is Uploaded?" Node
   - Connects to your product sheet.
   - Filters products where the `uploaded?` column is `"no"`.
   - Only returns **1 row** to avoid duplicate posts.

2. ### AI Agent
   - Uses Gemini API to generate a professional caption.
   - Includes:
     - Emojis
     - Hashtags
     - Call to action
   - Prompts users to check the Instagram bio for the website link (no URLs in caption).

3. ### Edit Fields
   - Extracts required fields:
     - Instagram Node ID
     - Product title & description
     - Image URLs: `mainimage`, `carousel1`, `carousel2`, `carousel3`
     - Caption from Gemini

4. ### Image Uploads
   - Up to **4 image upload nodes** post each image to Instagram as a container item.
   - Images must be public, direct JPG/PNG links.
   - Each upload is conditional:
     - If image URL exists → upload via Facebook Graph API
     - If not → skip node

5. ### Merge + Prepare Carousel Media IDs
   - Merges uploaded image nodes.
   - Outputs a single row with all image `media_id`s using this format:
     ```json
     {
       "children": "17932447635024904,17872489326361281"
     }
     ```

6. ### Create Carousel Container
   - Sends the image `media_ids` + Gemini-generated caption to create a carousel container.
   - Links to the Instagram Business Account ID.

7. ### Post to Instagram
   - Publishes the carousel post live on Instagram.

8. ### Google Sheets - Mark Uploaded
   - Updates the corresponding row’s `uploaded?` column to `"yes"` so it doesn’t get posted again.

---

## 🧠 Notes

- ✅ Only posts one product per run.
- 🧠 Caption formatting avoids a "bot-like" feel and follows your branding tone.
- 📷 Ensure image URLs are fully public and link directly to the file (no redirects, cookies, or HTML pages).
- ⚙️ All Facebook/Instagram tokens must be from a Business Account.
- 📌 `Node ID` used in API must match the Instagram Business Account ID linked to your Page.

---

## 🗂 Google Sheets Format Example

| ID | title | description | price | sale_price | mainimage | carousel1 | carousel2 | carousel3 | uploaded? |
|----|-------|-------------|-------|------------|-----------|-----------|-----------|-----------|-----------|
| 1  | Product Name | Short desc | 500 | 400 | img1.jpg | img2.jpg | img3.jpg | img4.jpg | no |

---

## 📺 Facebook API Setup (Video Guide)

To configure your Meta App and get Instagram Graph API access:

👉 **Watch this step-by-step YouTube guide**:  
[https://www.youtube.com/watch?v=AGSyWdjN5A4&ab_channel=Let%27sAutomateIt](https://www.youtube.com/watch?v=AGSyWdjN5A4&ab_channel=Let%27sAutomateIt)

This video walks you through:
- Creating a Meta Developer App
- Getting access tokens
- Setting up the correct permissions
- Linking your Instagram Business Account

---

## Support Development

If you find this project useful and would like to support further development, you can donate Bitcoin to the following address:

Bitcoin Address: `17Uv9ZgoKFXdi18PNf5UighASk53KMjzxp`

Your contributions will be greatly appreciated and will help in maintaining and improving this project. Thank you for your support!
