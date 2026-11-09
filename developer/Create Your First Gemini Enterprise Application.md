# Gemini Enterprise & Google Cloud Assessment Solutions

### 1. Weekly Sales Reports Sync Frequency
A company stores its weekly sales reports in a designated Cloud Storage bucket. To ensure their Gemini Enterprise application always reflects the latest data, which synchronization frequency should they use?
* [ ] Multiple ingestion types
* [✓] **Periodic**
* [ ] Streaming
* [ ] One time

> **Explanation:** Weekly sales reports are uploaded at regular, fixed intervals. **Periodic synchronization** is designed exactly for this scenario, pulling new data on a set schedule without the continuous overhead of streaming.

---

### 2. Video Overview from Specific Text Files
A researcher has a small set of specific text files and wants to create a video overview that summarizes only those sources. Which tool is best suited for this task?
* [✓] **Gemini Notebook**
* [ ] Idea Generation Agent
* [ ] Deep Research Agent
* [ ] General Gemini Enterprise Search

> **Explanation:** **Gemini Notebook** allows you to upload a specific, closed set of text files as sources. It specializes in summarizing those documents and can generate multimodal overviews based strictly on the uploaded material.

---

### 3. Global Food Marketing Trends Report
Cymbal Foods needs a comprehensive report on global food marketing trends by analyzing hundreds of external websites. Which feature should they use?
* [ ] Google Calendar data sync
* [ ] Gemini Notebook grounded questions
* [✓] **Deep Research agent**
* [ ] Search internal sources enhancement

> **Explanation:** The **Deep Research agent** is built to autonomously browse, synthesize, and analyze hundreds of external web sources to build comprehensive, deep-dive market reports.

---

### 4. Analyzing a Pie Chart PNG
A marketing team lead needs to quickly understand the primary findings of a customer sentiment pie chart saved as a PNG file. How can they use Gemini Enterprise to achieve this?
* [ ] Request a Deep Research report on pie chart design trends
* [✓] **Ask the AI assistant to ‘Summarize the customer sentiment pie chart.’**
* [ ] Transcribe the chart into a spreadsheet and connect it as a data store
* [ ] Use the Google Calendar data store to find the meeting where the chart was shown.

> **Explanation:** Gemini Enterprise features native multimodal capabilities. This allows it to **directly read, interpret, and analyze images** (like a PNG chart) when a user uploads it and asks a direct question.

---

### 5. Initial Configuration Security
A company’s Google Cloud administrator is performing the initial one-time configuration for a Gemini Enterprise application and needs to ensure the organization’s workforce can securely authenticate and access their internal data. Which task should they perform first?
* [✓] **Choose an identity provider**
* [ ] Define a data synchronization frequency
* [ ] Connect the organization’s data stores
* [ ] Enable the Gemini Code Assist API

> **Explanation:** Secure authentication must be established before handling corporate data. The administrator must **choose an identity provider (IdP)** first to verify user identities and establish proper access controls.

---

### 6. Shared Google Drive File Permissions
An administrator is concerned that connecting a shared Google Drive to the Gemini Enterprise application will allow everyone in the company to read private, unshared documents. How does Gemini Enterprise handle file permissions?
* [✓] **Users can only access documents already shared with them via existing permissions**
* [ ] It automatically grants read access to all employees for indexed files
* [ ] All connected files become public to any user with the application URL
* [ ] It requires the IT department to manually re-assign permissions for every file

> **Explanation:** Gemini Enterprise respects and strictly enforces **existing Access Control Lists (ACLs)**. A user will never see search results or summaries derived from a file they do not explicitly have permission to view in Google Drive.

---

### 7. Filtering PDF Search Results with Structured Data
A retail company has detailed product descriptions in PDF files and also has a corresponding set of structured data (product ID, price, stock level) for each PDF. To ensure that users can filter search results by price or stock level, which data import option should they choose?
* [ ] Media/Catalog Assets
* [ ] Documents
* [✓] **Documents with Metadata (RAG)**
* [ ] Tabular/E-commerce

> **Explanation:** When you need to pair unstructured files (PDFs) with structured attributes (price, stock level) for advanced filtering and Retrieval-Augmented Generation, **Documents with Metadata** is the correct ingestion option.

---

### 8. Unmapped Product Question in Gemini Notebook
You ask Gemini Notebook a question about a product that is not mentioned in any of the sources you uploaded to the current notebook. How will the tool respond?
* [✓] **It will generate an answer based only on the content of the uploaded documents**
* [ ] It will search your connected Google Drive to find the answer
* [ ] It will automatically browse the web to fill in the missing information
* [ ] It will ask you to add your Google Calendar as a source

> **Explanation:** Gemini Notebook relies strictly on **grounded generation** from your provided sources. If the information is missing from the uploaded files, it will explicitly restrict its answer to those documents (stating it cannot find the information) to prevent hallucinations.
