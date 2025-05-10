**📘 Complete Documentation: Automate Hugging Face Appointment Bot with Google Sheets using Apps Script**

---

## 🔰 Overview

This documentation guides you step-by-step to connect a Hugging Face Gradio Appointment Bot to a Google Sheet using Google Apps Script. By the end, submitted form data will automatically appear in your Google Sheet.

---

## 📍 Step-by-Step Guide

### ✅ Step 1: Create Google Sheet

1. Go to [Google Sheets](https://sheets.google.com).
2. Create a new sheet.
3. Name your columns in the first row:

   ```
   full name | phone | age | symptoms | appointment date | email
   ```
4. Note your spreadsheet URL.

---

### ✅ Step 2: Set Up Google Apps Script

1. In your Google Sheet, click:
   `Extensions` → `Apps Script`
2. Delete all default code and paste this:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = e.parameter;

  sheet.appendRow([
    data.full_name,
    data.phone,
    data.age,
    data.symptoms,
    data.appointment_date,
    data.email
  ]);

  return ContentService.createTextOutput("Success! Data added.");
}
```

3. Save your script (File → Save) and name it e.g. `PostData`.

---

### ✅ Step 3: Deploy as Web App

1. Click `Deploy` → `Manage deployments`
2. Click `+ New deployment`
3. Choose `Select type` → `Web app`
4. Set the following:

   * **Description:** `Webhook to Sheet`
   * **Execute as:** `Me`
   * **Who has access:** `Anyone`
5. Click `Deploy`
6. **Authorize the script** with your Google account.
7. Copy the Web App URL (e.g., `https://script.google.com/macros/s/.../exec`)

---

## 🧠 Step 4: Hugging Face (Gradio) Code

Paste this in your Hugging Face Space (Python file):

```python
import requests
import gradio as gr

WEBHOOK_URL = "https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec"  # Replace with your actual URL

def clinic_bot(full_name, phone, age, symptoms, appointment_date, email):
    data = {
        "full_name": full_name,
        "phone": phone,
        "age": age,
        "symptoms": symptoms,
        "appointment_date": appointment_date,
        "email": email,
    }

    # Send POST request to Google Apps Script
    try:
        response = requests.post(WEBHOOK_URL, data=data)
        msg = f"Thank you {full_name}!\n\nYour appointment request has been received.\n"
        msg += f"phone: {phone}\nage: {age}\nsymptoms: {symptoms}\n"
        msg += f"appointment date: {appointment_date}\n"
        if email:
            msg += f"confirmation will be sent to: {email}\n"
        msg += "\nWe will contact you shortly!"
        return msg
    except:
        return "Something went wrong while submitting your form."

# Gradio Interface
interface = gr.Interface(
    fn=clinic_bot,
    inputs=[
        gr.Textbox(label="full name"),
        gr.Textbox(label="phone number"),
        gr.Textbox(label="age"),
        gr.Textbox(label="symptoms"),
        gr.Textbox(label="appointment date (e.g., 12 May 2025)"),
        gr.Textbox(label="email (optional)")
    ],
    outputs="text",
    title="clinic appointment bot",
    description="fill in your details to book an appointment!"
)

interface.launch()
```

---

## ✅ Final Checklist

| Task                                              | Status |
| ------------------------------------------------- | ------ |
| Google Sheet created with columns                 | ✅      |
| Apps Script added and deployed                    | ✅      |
| Web App URL copied                                | ✅      |
| Hugging Face bot updated with correct webhook URL | ✅      |
| Form submissions reaching Google Sheet            | ✅      |

---

## 🛠️ Troubleshooting

* **Problem:** `Cannot read properties of null (reading 'appendRow')`

  * **Fix:** Check that you're using `SpreadsheetApp.getActiveSpreadsheet()` and sheet is open

* **Problem:** Data not reaching sheet

  * **Fix:** Make sure Web App is deployed and `Who has access` is set to `Anyone`

* **Problem:** Fields not saving properly

  * **Fix:** Match names exactly. `full_name` in both script and Python.
 
  * _________________________________________
  * https://chatgpt.com/canvas/shared/681f574a3d4c8191974655ea2a149e9c

MY HUGGING FACE BOT HERE ALSO FULLY CONNECTED WITH GOOGLE SHEET 

https://huggingface.co/spaces/abidalirana/AbidCareAI

---

## 🧾 Notes

* Field names are **case-sensitive** (use lowercase consistently).
* You can connect multiple sheets by changing the `openById()` method instead of `getActiveSpreadsheet()`.

If you need multi-sheet examples, just ask. ✅
