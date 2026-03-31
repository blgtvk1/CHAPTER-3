# LexiaBuddy — Product Requirements Document (PRD)

**Version:** 1.0  
**Status:** Draft  
**Last Updated:** 2026-03-31

---

## 1. Product Overview

**LexiaBuddy** is a single-page web application that converts standard PDF documents into dyslexia-friendly versions. The user uploads a PDF, the app extracts the text, sends it to an AI API for simplification, and returns a new downloadable PDF rendered in the Lexend font at a readable size.

The entire experience is browser-based, requires no login, no installation, and no technical knowledge.

---

## 2. Problem Statement

Dyslexic students encounter standard PDF documents daily — lecture notes, articles, exam instructions — that are written in dense, complex language using standard fonts. These documents create an unnecessary cognitive barrier between the student and the knowledge they need.

Existing solutions either read text aloud (without simplifying it) or require expensive institutional licenses. No tool currently takes a PDF in and gives a simplified, dyslexia-optimized PDF back — for free, instantly, in the browser.

---

## 3. Target Users

| User Type | Description |
|-----------|-------------|
| Primary | Students with dyslexia (primary school through university) |
| Secondary | Special education teachers adapting classroom materials |
| Secondary | Parents converting school documents for their children |
| Secondary | Dyslexic adults processing work documents |

**Common requirement across all users:** Zero technical knowledge assumed. The tool must be self-explanatory on first use.

---

## 4. Goals & Success Criteria

| Goal | Success Signal |
|------|---------------|
| Remove friction for dyslexic readers | User completes upload → convert → download in under 2 minutes |
| Preserve original content | No information is lost or summarized during simplification |
| Zero learning curve | User needs no instructions to complete the flow |
| Accessible design | UI itself uses Lexend font, high contrast, generous spacing |
| Privacy | No user data, no uploaded files stored after processing |

---

## 5. User Flow

```
1. User opens the website
2. User drags & drops a PDF onto Box 1, or clicks to open file picker
   → Only .pdf files accepted; other formats are rejected
3. Convert button (Box 2) becomes active (orange)
4. User clicks "Convert"
5. Box 3 shows a circular loading spinner
6. In the background:
   a. pdf.js extracts raw text from the uploaded PDF
   b. Extracted text is embedded into the Groq prompt
   c. Request sent to Groq API
   d. Groq returns simplified plain text
   e. jsPDF renders the simplified text in Lexend 18pt black on white
   f. PDF blob is generated in memory
7. Box 3 updates: shows "Your PDF is ready." + filename + orange Download button
8. User clicks Download → file saves to their device
```

---

## 6. UI Specification

### Global

| Property | Value |
|----------|-------|
| Background | `rgb(230, 230, 230)` |
| Font family | Google Fonts → Lexend (all elements) |
| Box border | 1px solid black |
| Box border radius | Rounded corners |
| Box shadow | Subtle elevation shadow |
| Primary accent color | Orange |

---

### Box 0 — Top Bar

| Property | Value |
|----------|-------|
| Content | Title: **LexiaBuddy** |
| Alignment | Horizontally centered |
| Font size | 36px |
| "Lexia" color | Orange |
| "Buddy" color | Black |

---

### Box 1 — File Upload Area

| Property | Value |
|----------|-------|
| Accepted file types | `.pdf` only |
| Interaction | Drag & drop OR click to open file picker |
| Placeholder text | "Upload Your PDF…" |
| Placeholder text color | Pale orange |
| Validation | Reject non-PDF files with a clear inline message |
| On valid file selected | Display filename inside the box; enable Box 2 |

---

### Box 2 — Convert Button

| Property | Value |
|----------|-------|
| Label | "Convert" |
| Default state | Grayed out, non-clickable (no PDF uploaded yet) |
| Active state | Orange label, clickable (triggered when valid PDF is in Box 1) |
| On click | Starts the converting procedure; shows spinner in Box 3 |

---

### Box 3 — Result Area

| State | Display |
|-------|---------|
| Default (no conversion yet) | Empty |
| Loading | Circular spinner animation, centered |
| Ready | Text: "Your PDF is ready." + converted filename + orange Download button |
| Error | Short error message (e.g. "Something went wrong. Please try again.") |

---

## 7. Technical Architecture

### Frontend Stack

| Layer | Technology |
|-------|-----------|
| UI | HTML / CSS / JavaScript (single file, no framework required) |
| PDF text extraction | `pdf.js` (pdfjs-dist) |
| PDF generation | `jsPDF` |
| Font | Google Fonts → Lexend (loaded via `@import` or `<link>`) |
| AI API client | `fetch()` — direct call to Groq API from browser |

---

### PDF Text Extraction (pdf.js)

```javascript
async function extractTextFromPDF(file) {
  const arrayBuffer = await file.arrayBuffer();
  const pdf = await pdfjsLib.getDocument(arrayBuffer).promise;

  let fullText = '';
  for (let i = 1; i <= pdf.numPages; i++) {
    const page = await pdf.getPage(i);
    const content = await page.getTextContent();
    const pageText = content.items.map(item => item.str).join(' ');
    fullText += pageText + '\n\n';
  }
  return fullText;
}
```

---

### Groq API Integration

**Endpoint:** `https://api.groq.com/openai/v1/chat/completions`

**Model:** `openai/gpt-oss-120b`  
**Max tokens:** `65536`  
**Provider:** `generic-chat-completion-api`

**API Key placeholder:**
```javascript
const GROQ_API_KEY = "YOUR_API_KEY_HERE"; // ← Fill this in before deployment
```

**Request structure:**
```javascript
async function simplifyWithGroq(extractedText) {
  const prompt = `You are a text simplification assistant for children with dyslexia.

Your ONLY job is to rewrite the text I give you. Do NOT summarize, do NOT skip content, do NOT add new information.

Rewrite rules:
- Replace complex or uncommon words with simple, everyday alternatives
- Break long sentences into short ones (maximum 15 words per sentence)
- Each paragraph should express only one idea
- Use bullet points or numbered lists where it helps clarity
- Keep all the original information and meaning
- Keep the same logical order as the original text
- Do not add titles, headers, or section labels unless they already exist in the original
- Do not include any explanation, preamble, or closing note — output ONLY the rewritten text

The reader is a child with dyslexia. Write as simply and clearly as possible.

Here is the text to rewrite:

${extractedText}`;

  const response = await fetch('https://api.groq.com/openai/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${GROQ_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: 'openai/gpt-oss-120b',
      max_tokens: 65536,
      messages: [{ role: 'user', content: prompt }]
    })
  });

  const data = await response.json();
  return data.choices[0].message.content;
}
```

---

### PDF Generation (jsPDF + Lexend)

```javascript
async function generatePDF(simplifiedText, originalFilename) {
  const doc = new jsPDF();

  // Load Lexend font (base64 embedded or via CDN)
  // doc.addFont('Lexend-Regular.ttf', 'Lexend', 'normal');
  doc.setFont('Lexend');
  doc.setFontSize(18);
  doc.setTextColor(0, 0, 0); // Black text

  const margin = 15;
  const pageWidth = doc.internal.pageSize.getWidth();
  const maxLineWidth = pageWidth - margin * 2;
  const lines = doc.splitTextToSize(simplifiedText, maxLineWidth);

  let y = margin;
  const lineHeight = 10;

  for (const line of lines) {
    if (y + lineHeight > doc.internal.pageSize.getHeight() - margin) {
      doc.addPage();
      y = margin;
    }
    doc.text(line, margin, y);
    y += lineHeight;
  }

  const outputFilename = originalFilename.replace('.pdf', '_lexiabuddy.pdf');
  doc.save(outputFilename);
  return outputFilename;
}
```

---

## 8. Full Conversion Pipeline

```
User uploads PDF
      ↓
extractTextFromPDF(file)
      ↓ raw text string
simplifyWithGroq(extractedText)   ←  Groq API: openai/gpt-oss-120b
      ↓ simplified text string
generatePDF(simplifiedText, filename)   ← jsPDF + Lexend 18pt black
      ↓
PDF blob → shown in Box 3 with Download button
```

---

## 9. Groq API Config Reference

```json
{
  "custom_models": [
    {
      "model_display_name": "GPT-OSS 120B [Groq]",
      "model": "openai/gpt-oss-120b",
      "base_url": "https://api.groq.com/openai/v1",
      "api_key": "YOUR_API_KEY_HERE",
      "provider": "generic-chat-completion-api",
      "max_tokens": 65536
    }
  ]
}
```

---

## 10. Error Handling

| Scenario | Behavior |
|----------|----------|
| Non-PDF file dropped | Reject with inline message: "Please upload a PDF file." |
| PDF has no extractable text (scanned image PDF) | Show: "This PDF contains no readable text. Please try a text-based PDF." |
| Groq API error / timeout | Show: "Something went wrong. Please try again." |
| Empty Groq response | Show: "The AI returned an empty response. Please try again." |
| PDF generation failure | Show: "Could not create the PDF. Please try again." |

---

## 11. Out of Scope (v1)

- User accounts or history
- Multi-language support (v1 handles whatever language is in the PDF)
- Scanned / image-only PDF support (requires OCR)
- Batch processing of multiple PDFs
- Adjustable font size or line spacing controls
- Server-side processing (v1 is fully client-side)

---

## 12. Open Questions

| # | Question | Owner |
|---|----------|-------|
| 1 | How to embed Lexend font into jsPDF? (base64 TTF vs CDN approach) | Dev |
| 2 | What is the max PDF size Groq's 65536 token limit can handle? | Dev |
| 3 | Should the output filename include a suffix like `_lexiabuddy`? | Design |
| 4 | Is a CORS proxy needed for browser-side Groq API calls? | Dev |

---

## 13. Next Steps

- [ ] Finalize Groq API key and insert into `GROQ_API_KEY` placeholder
- [ ] Confirm Lexend font embedding strategy for jsPDF
- [ ] Build and test the single-page HTML implementation
- [ ] Test with real dyslexia-oriented PDFs (varied length and complexity)
- [ ] Usability test with at least one dyslexic student or educator
