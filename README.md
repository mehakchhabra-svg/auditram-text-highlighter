AuditRAM Text Highlighter

This project is a Python-based solution for the AuditRAM Coding Assignment.
It allows a user to upload a file, search for a specific text string, and generate a new output file where all matched text is visually highlighted using red, unfilled bounding boxes, without modifying the original document.

Features
Supports:
PDF
DOCX
XLSX
Image formats (PNG, JPG, JPEG)

Searches the entire document

Draws red rectangular boxes around detected text

Output saved as a new annotated PDF

Original file remains unchanged

Works for scanned files using OCR

Technical Approach
1. File Type Handling
File Type	Handling Method
PDF	Render PDF pages into images
Images	OCR directly
DOCX/XLSX	converted to PDF using LibreOffice
All types	are processed visually for bounding box detection

3. Text Detection (OCR)
We use Tesseract OCR to:
Detect words
Extract confidence values
Extract bounding box coordinates (x, y, width, height)
Each word’s position is stored and processed line by line for search matching.

3. Text Search Logic
Case-insensitive search
Text reconstructed line-wise
Multiple occurrences detected
Character match mapped back to bounding box

4. Bounding Box Overlay
Using PyMuPDF:
Each match is converted from image coordinates into PDF coordinates
A rectangle annotation is added
Properties:
Color: Red
Fill: None (transparent)
Border-only overlay

6. Output Generation
A new file is generated with the suffix:
*_boxed.pdf
Example:
report.pdf → report_boxed.pdf

Installation
Step 1: Install Python Dependencies
pip install -r requirements.txt

Step 2: Install Tesseract OCR
Windows:

Download installer from:
https://github.com/tesseract-ocr/tesseract

Ensure path is added to environment variables.

Check:

tesseract --version

Linux:
sudo apt install tesseract-ocr

macOS:
brew install tesseract

Step 3 (Optional but Recommended): Install LibreOffice

Used for converting DOCX/XLSX to PDF automatically.

Windows:

Install from:

https://www.libreoffice.org

Linux:
sudo apt install libreoffice

How to Run
python auditram_highlighter.py "your_file" "text_to_search"

Example:
python auditram_highlighter.py sample.pdf "Revenue"


Output:

sample_boxed.pdf

Output

Highlighted PDF created
Red outline bounding boxes
Original document remains unchanged

⚠ Limitations

OCR accuracy depends on input document quality

DOCX/XLSX conversion requires LibreOffice installed

Low resolution scans may reduce text detection accuracy
Enhancements Possible

Native text detection for digital PDFs

Fuzzy text matching for OCR errors

Web-based UI (Flask)

Sensitivity tuning

Detailed Technical Approach

The system employs a unified visual processing pipeline for all file types, ensuring reliable bounding box detection.

File Ingestion
The system first determines the file type. Images are read directly, PDFs are rendered page by page into images, and DOCX/XLSX files are converted into PDFs using LibreOffice in headless mode.

OCR Processing
Each page image is processed using Tesseract OCR via pytesseract, which outputs individual words with their bounding box coordinates.

Text Reconstruction
OCR tokens are grouped line by line to reconstruct full, readable text lines. The search string is matched against reconstructed content.

Bounding Box Calculation
When a match is found:

Word coordinate boxes are unioned

A final rectangle is computed per match instance

Overlay Rendering
PyMuPDF is used to draw red unfilled rectangle annotations on the PDF.

Output File
The annotated PDF is saved without altering the original input file.

This architecture guarantees visual clarity, audit suitability, and format independence.
