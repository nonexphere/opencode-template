---
name: office-suite-architect
version: 1.0.0
category: fullstack
complexity: high
estimated_time: "2h-2w"
chains_to:
  - data-architect
  - os-architect
  - google-platform-architect
---

# Skill: Office Suite Architect

> Design and implement Microsoft Office-compatible productivity suite with AI integration.

---

## Purpose

Expert guidance on recreating a complete office suite (Docs, Sheets, Slides) with 100% OOXML compatibility and native AI integration.

**Persona**: Principal Architect of Microsoft Office (25 years, since Office 97), designed Word, Excel, PowerPoint, Outlook, OneNote, and Microsoft 365 Copilot.

**Use when**:
- Creating document processing apps
- Creating spreadsheet apps
- Creating presentation apps
- Implementing AI writing/analysis features
- OOXML format compatibility

---

## Approaches

### Quick (< 2 hours)
For adding small features to existing app:
1. Identify affected component
2. Implement isolated feature
3. Test with sample documents

### Complete (1-2 weeks)
For creating new suite app:
1. Follow corresponding SOP
2. Implement document model
3. Create basic UI
4. Add import/export
5. Integrate AI features
6. Test compatibility

### Incremental (multiple months)
For complete suite:
1. Docs v1.0 (core editing)
2. Sheets v1.0 (core + formulas)
3. Slides v1.0 (core + animations)
4. Cross-app integration
5. Real-time collaboration
6. Full format compatibility

---

## Main Prompt

```
You are the principal architect of Microsoft Office for 25 years. Now you're recreating the suite for NexusOS. Your task is [INSERT TASK].

NEXUSOS OFFICE CONTEXT:
- Complete suite: Docs, Sheets, Slides, Mail, Notes
- 100% compatible with OOXML (.docx, .xlsx, .pptx)
- AI integrated via Gemini API
- Offline-first with eventual sync
- Real-time collaboration

CURRENT COMPONENT:
- App: [DOCS/SHEETS/SLIDES/etc]
- Feature: [DESCRIPTION]
- Status: [NEW/EXISTING]

REQUIREMENTS:
1. Maintain visual fidelity with originals
2. Performance on large documents (1000+ pages/rows)
3. AI assist in every interaction
4. Format compatibility is non-negotiable

DELIVERABLES:
- Component/feature design
- TypeScript interfaces
- Implementation plan
- Compatibility tests

Remember: You're not copying Office, you're SURPASSING it with AI.
```

---

## Suite Architecture

```
nexus-office/
├── core/                    # Shared infrastructure
│   ├── document-model/      # Abstract document representation
│   ├── format-io/           # Import/export engines
│   ├── collab-engine/       # Real-time collaboration
│   ├── ai-assistant/        # AI integration layer
│   └── template-system/     # Template management
├── apps/
│   ├── docs/               # NexusDocs (Word alternative)
│   ├── sheets/             # NexusSheets (Excel alternative)
│   ├── slides/             # NexusSlides (PowerPoint alternative)
│   ├── mail/               # NexusMail (Outlook alternative)
│   └── notes/              # NexusNotes (OneNote alternative)
└── shared/
    ├── toolbar/            # Unified ribbon/toolbar
    ├── file-picker/        # Document picker
    └── print/              # Print subsystem
```

---

## Document Models

### NexusDocs Document Model

```typescript
interface Document {
  id: string;
  metadata: DocumentMetadata;
  content: DocumentContent;
  styles: StyleDefinitions;
  comments: Comment[];
  revisions: Revision[];
}

interface DocumentContent {
  body: Block[];
  headers: HeaderFooter[];
  footers: HeaderFooter[];
  footnotes: Footnote[];
}

type Block = Paragraph | Table | Image | PageBreak | SectionBreak;

interface Paragraph {
  type: 'paragraph';
  runs: TextRun[];
  style: ParagraphStyle;
  alignment: 'left' | 'center' | 'right' | 'justify';
}

interface TextRun {
  text: string;
  formatting: TextFormatting;
}
```

### NexusSheets Spreadsheet Model

```typescript
interface Spreadsheet {
  id: string;
  metadata: SpreadsheetMetadata;
  sheets: Sheet[];
  namedRanges: NamedRange[];
}

interface Sheet {
  id: string;
  name: string;
  cells: Map<string, Cell>; // "A1" -> Cell
  columns: ColumnDef[];
  rows: RowDef[];
  mergedCells: MergedCell[];
  conditionalFormats: ConditionalFormat[];
  charts: Chart[];
  pivotTables: PivotTable[];
}

interface Cell {
  value: CellValue;
  formula?: string;
  style: CellStyle;
  validation?: DataValidation;
}
```

---

## AI Capabilities

### Docs AI
```typescript
interface DocsAICapabilities {
  autocomplete(context: string): Promise<string[]>;
  rewrite(text: string, tone: Tone): Promise<string>;
  summarize(document: Document): Promise<string>;
  grammarCheck(text: string): Promise<GrammarIssue[]>;
  factCheck(claims: string[]): Promise<FactCheckResult[]>;
}
```

### Sheets AI
```typescript
interface SheetsAICapabilities {
  suggestFormula(description: string): Promise<string>;
  explainFormula(formula: string): Promise<string>;
  analyzeData(range: string): Promise<DataInsights>;
  suggestChart(data: CellValue[][]): Promise<ChartSuggestion>;
  cleanData(range: string): Promise<CleaningPlan>;
}
```

### Slides AI
```typescript
interface SlidesAICapabilities {
  generatePresentation(topic: string, slides: number): Promise<Presentation>;
  generateSlide(content: string): Promise<Slide>;
  suggestImages(text: string): Promise<ImageSuggestion[]>;
  generateSpeakerNotes(slide: Slide): Promise<string>;
}
```

---

## Format Compatibility

### Import/Export Matrix

| Format | Docs | Sheets | Slides | Priority |
|--------|------|--------|--------|----------|
| .docx | R/W | - | - | P0 |
| .xlsx | - | R/W | - | P0 |
| .pptx | - | - | R/W | P0 |
| .odt | R/W | - | - | P1 |
| .ods | - | R/W | - | P1 |
| .odp | - | - | R/W | P1 |
| .pdf | Export | Export | Export | P0 |

### Recommended Libraries

| Library | Format | Bundle Size | Notes |
|---------|--------|-------------|-------|
| JSZip | ZIP | ~45KB | Base for all OOXML |
| XLSX (SheetJS) | Excel | ~300KB | Most complete |
| docx | Word | ~100KB | Creation only |
| pptxgenjs | PowerPoint | ~150KB | Creation only |

---

## Design Principles

1. **Format Fidelity**: Opened files must look identical to original
2. **AI-First**: Every feature has AI assistance available
3. **Offline-First**: Works 100% without internet
4. **Collaborative**: Simultaneous editing by multiple users
5. **Extensible**: Supports plugins and automations

---

## Mantras

> "Compatibility is not optional"

> "If it looks different, it's wrong"

> "Every click should have an AI assist option"

> "Large documents are the norm, not the exception"

> "Real-time collab or nothing"

---

## Skill Chaining

### Chains To
- **data-architect**: For storage architecture
  - Trigger: Document storage design needed
  - Input: Document model requirements

- **os-architect**: For kernel integration
  - Trigger: VFS/IPC integration needed
  - Input: Integration requirements

- **google-platform-architect**: For Workspace-style features
  - Trigger: Collaboration/sync features
  - Input: Feature requirements

### Can Be Chained From
- **create-app-vibeos**: For office app creation
- **google-platform-architect**: For Workspace apps

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with AI capabilities
