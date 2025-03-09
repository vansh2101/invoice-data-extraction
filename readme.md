# Invoice Data Extraction

This project focuses on extracting structured and unstructured data from invoice documents using Gemini-2.0-Flash. The system retrieves key details such as:

- Invoice tables (item descriptions, quantities, prices)
- Buyer and seller details
- Invoice values (subtotal, taxes, total amount)
- Other metadata (invoice number, date, payment terms)

To ensure the reliability of extracted data, a confidence score is computed using *Word Error Rate (WER)* and *Character Error Rate (CER)*.

## File Structure
- **Invoice_Data_Extraction.ipynb:** Contains all the code for the pipeline
- **invoice.png:** sample invoice used for testing
- **invoice_data.json:** output of invoice.png from our pipeline.

## Flow of Thought
1. *File Format Conversion (if needed):* If invoices are in pdf format, we extract all the pages as images.

2. *Gemini Vision Capabilities:* We use `Gemini-2.0-flash` along with structured prompting method to extract both tabular and unstructured data mentioned in the provided invoice. We instruct gemini to output the data in the given format i.e. json object.

3. *Confidence Estimation:*
    - We utilize **Chain-of-Thought prompting** and **Self-Ask prompting** method to provide steps to our data extraction model to compute a confidence score.
    - Gemini-2.0-Flash is used to write a python code to compute WER and CER on the reference and its extracted data.
    - The final confidence score is computed using the formula:
$Confidence Score = 1 + weight * \frac{(WER + CER)}{2}$


## Comparison
### 1. Microsoft's Table Transformer
- A powerful deep-learning model designed to extract structured tabular data from documents.
- Limitations:
    - Focuses primarily on table extraction and struggles with unstructured elements like buyer/seller details or payment terms.
    - Computationally expensive with longer inference times.
 
### 2. PDF to Markdown (pdf2md)
- Converts PDF documents into markdown-formatted text, preserving the structural integrity.
- Limitations:
    - Lacks intelligent parsing—fails to differentiate between invoice metadata and tabular data.
    - Struggles with handwritten or scanned invoices requiring OCR-based extraction.


## Why Use Gemini-2.0-Flash?
### 1. Speed & Efficiency 
Gemini-2.0-Flash is optimized for fast inference and cost efficiency, making it ideal for production use.

### 2. Intelligence
Unlike other models, Gemini models have both intelligence and vision capabilities which helps in reducing errors in case of vision tasks.

### 3. Structured & Unstructured Data Extraction
Structured: Extracts well-defined fields like invoice number, date, and itemized tables.

Unstructured: Retrieves free-text sections such as payment terms and notes.

### 4. Benchmark results
Gemini-2.0-flash outperforms many state-of-the-art models in terms of table extraction from documents making it an ideal choice for this task as well.


## Why Use WER and CER for Confidence Scoring?
### 1. Word Error Rate (WER)
Measures how many words in the extracted text are incorrect compared to the reference.
Formula:

$WER = \frac{\text{word errors}}{\text{total words in reference}}$

- Lower WER → Higher accuracy.

### 2. Character Error Rate (CER)
Measures character-level discrepancies, which helps detect small OCR or extraction errors.

$CER = \frac{\text{character errors}}{\text{total characters in reference}}$

- Useful for identifying minor typos or formatting inconsistencies.

### 3. Combining WER & CER
Using both metrics provides a balanced accuracy measure at both word and character levels.

The final confidence score is computed using their weighted average.

- This ensures a more robust evaluation than relying on a single metric.

