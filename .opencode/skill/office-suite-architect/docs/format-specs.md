# Document Format Specifications

**Document ID:** `office-suite-architect/docs/format-specs`  
**Purpose:** Specifications for supported file formats

---

## 1. OOXML Overview

Office Open XML (OOXML) is the standard format for Microsoft Office since 2007.

### Common Structure

All OOXML files are ZIPs containing:

```
[file].docx/.xlsx/.pptx
├── [Content_Types].xml      # MIME types
├── _rels/
│   └── .rels               # Root relationships
├── docProps/
│   ├── app.xml             # Application properties
│   └── core.xml            # Core properties (author, title)
└── word|xl|ppt/            # App-specific content
    ├── document.xml|workbook.xml|presentation.xml
    ├── styles.xml
    ├── theme/
    └── _rels/
```

---

## 2. DOCX Structure

```
document.docx
├── [Content_Types].xml
├── _rels/.rels
├── docProps/
│   ├── app.xml
│   └── core.xml
└── word/
    ├── document.xml        # Main content
    ├── styles.xml          # Style definitions
    ├── fontTable.xml       # Font mappings
    ├── settings.xml        # Document settings
    ├── numbering.xml       # List numbering
    ├── footnotes.xml       # Footnotes
    ├── comments.xml        # Comments
    ├── theme/
    │   └── theme1.xml      # Theme colors/fonts
    ├── media/              # Embedded images
    └── _rels/
        └── document.xml.rels
```

### Key Elements (document.xml)

```xml
<w:document xmlns:w="...">
  <w:body>
    <w:p>                           <!-- Paragraph -->
      <w:pPr>                       <!-- Paragraph properties -->
        <w:pStyle w:val="Heading1"/>
      </w:pPr>
      <w:r>                         <!-- Run (text span) -->
        <w:rPr>                     <!-- Run properties -->
          <w:b/>                    <!-- Bold -->
          <w:i/>                    <!-- Italic -->
        </w:rPr>
        <w:t>Hello World</w:t>      <!-- Text content -->
      </w:r>
    </w:p>
    <w:tbl>                         <!-- Table -->
      <w:tr><w:tc><w:p>...</w:p></w:tc></w:tr>
    </w:tbl>
  </w:body>
</w:document>
```

---

## 3. XLSX Structure

```
spreadsheet.xlsx
├── [Content_Types].xml
├── _rels/.rels
├── docProps/
└── xl/
    ├── workbook.xml        # Workbook structure
    ├── styles.xml          # Cell styles
    ├── sharedStrings.xml   # String table
    ├── theme/
    ├── worksheets/
    │   ├── sheet1.xml      # Sheet data
    │   └── sheet2.xml
    ├── charts/             # Embedded charts
    ├── drawings/           # Shapes, images
    ├── pivotTables/
    └── _rels/
```

### Key Elements (sheet1.xml)

```xml
<worksheet xmlns="...">
  <sheetData>
    <row r="1">                     <!-- Row 1 -->
      <c r="A1" t="s">              <!-- Cell A1, type=string -->
        <v>0</v>                    <!-- Index in sharedStrings -->
      </c>
      <c r="B1" t="n">              <!-- Cell B1, type=number -->
        <v>42</v>
      </c>
      <c r="C1">                    <!-- Cell C1 with formula -->
        <f>A1+B1</f>
        <v>42</v>                   <!-- Cached result -->
      </c>
    </row>
  </sheetData>
</worksheet>
```

---

## 4. PPTX Structure

```
presentation.pptx
├── [Content_Types].xml
├── _rels/.rels
├── docProps/
└── ppt/
    ├── presentation.xml    # Presentation structure
    ├── presProps.xml       # Presentation properties
    ├── theme/
    ├── slideMasters/
    ├── slideLayouts/
    ├── slides/
    │   ├── slide1.xml
    │   └── slide2.xml
    ├── notesSlides/        # Speaker notes
    ├── media/              # Images, videos
    └── _rels/
```

### Key Elements (slide1.xml)

```xml
<p:sld xmlns:p="..." xmlns:a="...">
  <p:cSld>
    <p:spTree>              <!-- Shape tree -->
      <p:sp>                <!-- Shape -->
        <p:nvSpPr>          <!-- Non-visual properties -->
          <p:cNvPr id="1" name="Title"/>
        </p:nvSpPr>
        <p:spPr>            <!-- Shape properties -->
          <a:xfrm>          <!-- Transform (position/size) -->
            <a:off x="0" y="0"/>
            <a:ext cx="9144000" cy="1143000"/>
          </a:xfrm>
        </p:spPr>
        <p:txBody>          <!-- Text body -->
          <a:p>
            <a:r>
              <a:t>Slide Title</a:t>
            </a:r>
          </a:p>
        </p:txBody>
      </p:sp>
    </p:spTree>
  </p:cSld>
</p:sld>
```

---

## 5. Parsing Libraries

### Recommended for Browser

| Library | Format | Bundle Size | Notes |
|---------|--------|-------------|-------|
| JSZip | ZIP | ~45KB | Base for all OOXML |
| XLSX (SheetJS) | Excel | ~300KB | Most complete |
| docx | Word | ~100KB | Creation only |
| pptxgenjs | PowerPoint | ~150KB | Creation only |

### Parsing Strategy

```typescript
import JSZip from 'jszip';

async function parseDocx(file: File): Promise<Document> {
  const zip = await JSZip.loadAsync(file);
  
  // Validate it's valid OOXML
  const contentTypes = await zip.file('[Content_Types].xml')?.async('string');
  if (!contentTypes) throw new Error('Invalid OOXML');
  
  // Core files
  const documentXML = await zip.file('word/document.xml')?.async('string');
  const stylesXML = await zip.file('word/styles.xml')?.async('string');
  
  // Parse and return
  return {
    content: parseDocumentXML(documentXML),
    styles: parseStylesXML(stylesXML),
  };
}
```

---

> **Caution:** OOXML is complex. Start with 80% of use cases and add support incrementally.
