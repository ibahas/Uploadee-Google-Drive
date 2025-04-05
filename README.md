# Colab Multi-Destination File Uploader (Upload.ee & Google Drive)

This project provides an interactive file browser and uploader GUI within a Google Colaboratory notebook. It allows users to:

*   Browse the `/content/` directory structure in Colab.
*   Select multiple files using checkboxes.
*   Upload the selected files to either:
    *   **Upload.ee:** Using simulated browser requests, dynamic ID fetching, and progress polling.
    *   **Google Drive:** Using the official Google Drive API via Colab's built-in authentication.

The interface is built using `ipywidgets`.

## Features

*   **Interactive File Browser:** Navigate directories within `/content/`.
*   **Multiple File Selection:** Select one or more files using checkboxes.
*   **Select All / Deselect All:** Convenience buttons for file selection within the current view.
*   **Dual Destination Upload:**
    *   **Upload.ee:** Handles dynamic session ID fetching and cookie management.
    *   **Google Drive:** Integrates with Colab's authentication flow. Option to specify a target folder ID (uploads to root 'My Drive' if omitted or invalid).
*   **Individual Progress Bars:** Shows upload progress for the currently uploading file.
*   **Sequential Uploads:** Uploads selected files one after another.
*   **Status Logging:** Provides feedback on authentication, fetching details, upload progress, success/failure, and relevant links in an output area.
*   **Responsive UI Elements:** Buttons and input fields enable/disable based on context (e.g., destination selected, authentication status).

## Requirements

*   **Google Colaboratory:** This script is specifically designed to run in a Colab environment.
*   **Python 3:** Standard in Colab.
*   **Python Libraries:**
    *   `ipywidgets` (usually pre-installed in Colab)
    *   `requests`
    *   `google-api-python-client`
    *   `google-auth-httplib2`
    *   `google-auth-oauthlib`

## Setup

1.  **Get the Notebook:** Clone this repository or download the `.ipynb` file to your Google Drive or upload it directly to Colab.
2.  **Install Dependencies:** Before running the main code, execute the following cell in your Colab notebook to install the necessary libraries:
    ```python
    !pip install --upgrade requests google-api-python-client google-auth-httplib2 google-auth-oauthlib --quiet
    ```
3.  **Run the Main Code:** Execute the large Python code cell containing the file browser and uploader logic.

## Usage

1.  **Initialization:** After running the main code cell, wait for the "Initializing file browser..." and "File browser ready." messages. The UI widgets will appear below the cell.
2.  **Browsing:**
    *   Click on folder icons (`📁 FolderName`) to navigate into that directory.
    *   Click the "⬆️ .." button to go up one level.
    *   The "Current:" path indicates your location.
3.  **Selecting Files:**
    *   Click the checkbox next to each file (`📄 FileName`) you want to upload.
    *   Selected filenames will appear in the "Selected:" list box below the file browser.
    *   Use the "Select All" and "Deselect All" buttons to quickly manage selections *within the currently viewed directory*.
4.  **Choosing Destination:**
    *   Use the "Destination:" dropdown to select either `Upload.ee` or `Google Drive`.
5.  **Google Drive Options (if selected):**
    *   The "Authenticate GDrive" button will become enabled. Click it.
    *   A Colab popup will appear asking for authorization to access your Google Drive. Follow the prompts and grant permission. Log messages will confirm success or failure. Authentication is usually required once per session.
    *   Optionally, enter a specific Google Drive **Folder ID** in the "GDrive Folder:" text box. You can get this ID from the URL in your browser when viewing the target folder in Google Drive (it's the long alphanumeric string after `/folders/`). If left blank or invalid, files will be uploaded to the root "My Drive".
6.  **Uploading:**
    *   Once files are selected and the destination/authentication is set, click the "Upload Selected Files" button.
7.  **Monitoring:**
    *   The upload button will disable during the process.
    *   The progress bar will appear, showing the progress for the *currently uploading file*. The description will indicate the filename and destination.
    *   Detailed logs will appear in the "Messages & Upload Log:" area, showing:
        *   Fetching of Upload.ee details (if applicable).
        *   Start/End of each file upload.
        *   Success messages with relevant links (finished page URL for Upload.ee, view link for Google Drive).
        *   Error messages if uploads fail.
    *   A final summary showing the total attempted, successful, and failed uploads will be printed.
8.  **Completion:** Once all selected files are processed, the upload button will re-enable.

## Notes & Limitations

*   **Upload.ee Reliability:** Upload.ee support relies on the current structure and endpoints of their website (`ubr_link_upload.php`, `/progress`, `/cgi-bin/ubr_upload.pl`). If Upload.ee changes its site significantly, this functionality may break and require code updates. Success detection depends on parsing the specific redirect script in the response.
*   **Google Drive Authentication:** You typically need to re-authenticate Google Drive each time you start a new Colab session or if the session times out.
*   **Sequential Uploads:** Files are uploaded one by one, not in parallel.
*   **Error Handling:** Basic error handling is included, but complex server-side issues or network interruptions might result in uncertain statuses. Check the logs carefully.
*   **Colab Environment:** The file browser is limited to the `/content/` directory accessible within the Colab runtime.
*   **Large Files:** Uploading very large files might be slow or hit timeouts depending on network conditions and potential server limits. The timeout for Upload.ee uploads is currently set high (1800 seconds / 30 minutes).

## License

Feel free to use, modify, and distribute this code. (Consider adding a specific license like MIT if desired).