# Invoice Data Extraction

This project focuses on extracting structured and unstructured data from invoice documents using Gemini-2.0-Flash. The system retrieves key details such as:

- Invoice tables (item descriptions, quantities, prices)
- Buyer and seller details
- Invoice values (subtotal, taxes, total amount)
- Other metadata (invoice number, date, payment terms)

To ensure the reliability of extracted data, a confidence score is computed using *Word Error Rate (WER)* and *Character Error Rate (CER)*.

## Extraction Process
1. Preprocessing (if needed): If invoices are in image format, OCR (e.g., Tesseract or Google Vision API) is applied to extract raw text.

2. Structured Prompting with Gemini-2.0-Flash: A well-formed prompt is used to extract relevant details from the invoice in JSON format.

3. Self-Ask Prompting for Confidence Estimation:
    - Gemini-2.0-Flash is used to compute WER and CER to quantify extraction accuracy.
    - Format consistency checks (e.g., valid invoice date, subtotal + taxes = total) are performed.
    - The final confidence score is computed using the formula:
`Confidence Score = 1 + (WER + CER / 2)`

## Why Use Gemini-2.0-Flash?
### 1. Speed & Efficiency 
Gemini-2.0-Flash is optimized for fast inference and cost efficiency, making it ideal for real-time invoice data extraction.

### 2. Structured & Unstructured Data Extraction
Structured: Extracts well-defined fields like invoice number, date, and itemized tables.

Unstructured: Retrieves free-text sections such as payment terms and notes.

### 3. Self-Ask Prompting for Confidence Estimation
Gemini-2.0-Flash can analyze its own output by performing additional computations, such as WER and CER, to determine reliability.


## Why Use WER and CER for Confidence Scoring?
### 1. Word Error Rate (WER)

Measures how many words in the extracted text are incorrect compared to the reference.

Formula:

`WER = \frac{\text{word errors}}{\text{total words in reference}}`

- Lower WER → Higher accuracy.

### 2. Character Error Rate (CER)

Measures character-level discrepancies, which helps detect small OCR or extraction errors.

Formula:

`CER = \frac{\text{character errors}}{\text{total characters in reference}}`

- Useful for identifying minor typos or formatting inconsistencies.

### 3. Combining WER & CER

Using both metrics provides a balanced accuracy measure at both word and character levels.

The final confidence score is computed using their average:

`1 - \frac{\text{WER} + \text{CER}}{2}`

- This ensures a more robust evaluation than relying on a single metric.

