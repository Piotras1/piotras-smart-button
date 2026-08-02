# Installation Guide for v2.0 Test Version (testV2)

### This guide will walk you through the manual installation process of the piotras-smart-button card version 2.0.

### ⚠️ VERY IMPORTANT (Step 1):
**If you currently have the official version of this card installed via HACS, you must completely uninstall it from HACS first (and ensure no old resource entries remain in your Dashboard resources, as they will cause conflicts). Having two versions simultaneously will cause the card to break and fail to run the new syntax correctly!**

#### 2. Download files from the repository
**Go to the [testV2 directory](https://github.com/Piotras1/piotras-smart-button/tree/main/testV2) in the GitHub repository[cite: 1] and download the following three files to your local disk:**  
- piotras-smart-button.js
- piotras-smart-button-editor.js
- piotras-smart-button-loader.js

#### 3. Place the files on your Home Assistant disk
- Move the downloaded files to the appropriate directory on your Home Assistant (via a file manager, such as File Editor or Samba Share):
to the directory **config/www/piotras-smart-button/**
- If the **piotras-smart-button** directory does not exist inside the `www` folder, create it.

#### 4. Add the Resource in Home Assistant
**For Home Assistant to recognize the new card, you must register the loader file:**
- Go to in your Home Assistant: **Settings** -> **Dashboards** -> **Resources** tab.
- Click **Add Resource** in the bottom right corner.
- In the **URL** field, enter exactly:  **/local/piotras-smart-button/piotras-smart-button-loader.js?v=2.0**
- In the **Type** field, select **JavaScript Module**.
- Click **Save**.

#### 5. Refresh your browser
- You're all set! Now refresh your browser (preferably with cache clearing, e.g., Ctrl + F5, or restart the HA mobile app), create a new card of type `custom:piotras-smart-button`, and enjoy the new features of version 2.0!