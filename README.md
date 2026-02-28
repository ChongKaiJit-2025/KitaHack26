# Food Guardian: Your 24-Hour Stomach Concierge

## 1. Technical Architecture
The Food Guardian prototype is designed as a lightweight, client-side web application leveraging cloud-based AI for intelligent analysis.

*   **Frontend Interface:** Built with **Vanilla HTML5, CSS3, and JavaScript**, ensuring a fast, bloat-free experience without the need for complex frameworks.
*   **AI Engine (Core Logic):** Powered by the **Google Gemini 2.5 Flash Lite API**. The app makes asynchronous REST API calls directly from the client to process multimodal inputs (images and text) to determine food freshness and expiry.
*   **Voice Processing:** Utilizes the native browser **Web Speech API** (`webkitSpeechRecognition` for Speech-to-Text and `window.speechSynthesis` for Text-to-Speech) for seamless voice interactions.
*   **Data Management:** Currently relies on an in-memory data structure (`inventory` array) to track items during the user's session.

## 2. Implementation Details
The application serves as a single-page dashboard where users can interact with early-warning systems for their food.

*   **Multimodal Input Pipeline:** Users can communicate via text, voice, or image uploads (Scanning). When an image is uploaded, it is converted to a Base64 string and sent alongside a strict prompt to the Gemini API.
*   **Structured AI Prompting:** The Gemini API is instructed to act as a "Stomach Concierge." The prompt enforces a specific output format: it provides conversational feedback to the user but strictly appends a hidden data string `DATA|ItemName|ExpiryHours|Status` at the end if an item is confirmed.
*   **Dynamic Inventory System:** The frontend parses this AI string and creates an entry in the "Live Log." A script dynamically calculates the "Stomach Window" (hours remaining) based on the current time and the AI's estimation, updating the UI with color-coded warnings (Green for safe, Orange for approaching expiry, Red/DANGER for spoiled).
*   **Waste Tracking Analytics:** A simple logic aggregator calculates items saved and estimates "Waste Prevented" in kilograms based on the scanned inventory.

## 3. Challenges Faced
*   **Prompt Engineering & Data Formatting:** Getting the LLM to consistently provide friendly, concise feedback to the user while strictly adhering to the `DATA|...` string format at the end of its response required extensive prompt tuning. Sometimes the model would try to format the data block in markdown, which broke our frontend parser. We had to enforce strict rules against markdown formatting in the prompt.
*   **Client-Side Constraints:** Making direct API calls from the client-side JavaScript meant we had to manage the image payload size carefully. Converting high-resolution images to Base64 could cause performance hitches, requiring us to manage the payload efficiency. 
*   **Web Speech API Compatibility:** Relying on `webkitSpeechRecognition` meant dealing with browser-specific quirks. It works flawlessly on Chromium-based browsers but required graceful fallbacks and error handling for unsupported environments.

## 4. Future Roadmap
*   **Persistent Storage Integration:** Move from in-memory arrays to `localStorage` or a lightweight backend database (e.g., Firebase or Supabase) so users don't lose their inventory when the browser closes.
*   **Real-time Camera Feed:** Upgrade the "Scan Food" feature from a static file upload button to a live WebRTC camera feed for instant, continuous scanning.
*   **Automated Push Notifications:** Implement Service Workers to send push notifications to the user's phone or desktop when an item's "Stomach Window" is about to close (e.g., "Drink that milk within 2 hours!").
*   **AI Recipe Generation:** Add a feature where the concierge suggests zero-waste recipes using *only* the specific ingredients that are currently marked orange (close to their expiry time) in the Live Log.



**=====REMINDER(IMPORTANT)=====**

To run the code, remember to get your Google API Keys from https://aistudio.google.com/app/apikey and paste it in the quote as shown below.

![alt text](<Screenshot 2026-02-28 195425-1.png>)

***Use PC platform(MacOS/Windows) for the best experience.
