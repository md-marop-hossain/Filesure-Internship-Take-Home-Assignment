# PDF Data Extractor and AI Summarizer
This project provides a Python script to extract structured data from scanned PDF documents (specifically targeting **"Form ADT-1"** corporate filings) using **Optical Character Recognition (OCR)**. It then leverages the **Groq API** and a large language model to generate a concise, non-technical summary of the extracted information.

## **Features**
- **OCR-based Text Extraction:** Handles scanned PDFs where text cannot be directly selected or copied, by converting pages into images and applying OCR.
- **Structured Data Parsing:** Extracts key information such as company name, CIN, registered office, appointment date, auditor details, and more from the OCR'd text.
- **AI-Powered Summarization:** Generates a 3-5 line summary of the extracted data in an easy-to-understand format for a non-technical audience using the Groq API.
- **JSON Output:** Saves the extracted structured data into a JSON file.
- **Text Summary Output:** Saves the AI-generated summary into a text file.
## **Technologies Used**
- **Python:** Core programming language.
- **Tesseract OCR:** OCR engine for text recognition from images.
- **Poppler:** PDF rendering library used by pdf2image.
- **PDF2Image:** Python wrapper to convert PDF pages to images.
- **Pytesseract:** Python wrapper for Tesseract.
- **Pillow (PIL Fork):** Image processing library.
- **Groq API:** For integrating with the **llama3-8b-8192 language model** for summarization.
## **Installation**
To set up the environment, follow these steps:
1. **Install System Dependencies (Tesseract and Poppler):**
```
!apt-get install -y tesseract-ocr
!apt-get install -y poppler-utils
!pip install pytesseract pdf2image Pillow
```
2. **Install Python Libraries:**
```
!pip install pytesseract pdf2image Pillow groq
```
## **Usage**
1. **Place your PDF file:** Ensure your **"Form ADT-1-29092023_signed.pdf"** file is in the same directory as the extractor.py script, or update the pdf_file variable in main() function within the script to point to the correct path.
2. **Set up Groq API Key:**
The script expects your Groq API key to be set as a user secret in your environment (e.g., Google Colab secrets or an environment variable named GROQ_API_KEY).
If you are using Google Colab, you can add it via Runtime -> Manage secrets.
3. **Run the script:**
```
python extractor.py
```
## **Output**
Upon successful execution, the script will:
- Create an output.json file containing the structured data extracted from the PDF.
- Generate an AI-powered summary and print it to the console.
- Save the summary to summary.txt.
- Initiate a download of summary.txt (if run in an environment like Google Colab).
## **How it Works**
The script operates in two main phases:
1. **PDF Data Extraction:**
- **extract_text_with_ocr(pdf_path):** Takes the PDF path, converts each page to an image, and uses Tesseract OCR to extract text.
- **parse_and_clean_adt1_data(text):** Parses the raw OCR text using regular expressions to identify and extract specific fields related to corporate filings (e.g., company name, auditor details). It also cleans up the extracted text.
2. **AI-Generated Summary:**
- The main function calls extract_text_with_ocr and parse_and_clean_adt1_data to get the structured data.
- **generate_summary_with_groq(json_data):** Takes the extracted structured JSON data, constructs a prompt, and sends it to the Groq API using the llama3-8b-8192 model. The model is instructed to generate a concise, non-technical summary suitable for corporate filings.
## **Error Handling**
The script includes basic error handling for:
- **FileNotFoundError:** If the specified PDF file does not exist.
- **RuntimeError:** If OCR extraction fails.
- **userdata.SecretNotFoundError:** If the GROQ_API_KEY is not found.
- **json.JSONDecodeError:** If the output.json file cannot be parsed.
- General exceptions during API calls or file operations.
