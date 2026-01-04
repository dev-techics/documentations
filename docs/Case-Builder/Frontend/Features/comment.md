---
sidebar_position: 3
---

# Comment System

## How It Works

### Overview

The comment system allows users to attach comments to specific text selections inside a document.

**User flow:**

1. The user selects text inside a document.
2. A floating toolbar appears above the selected text.
3. Clicking the **Comment** button opens a comment input panel on the right side of the document.
4. When the user submits the comment:
   - The comment is saved with positional metadata.
   - A comment icon appears on the right side of the document.
   - The icon is vertically aligned with the selected text.

---

## Internal Flow

### 1. Text Selection & Toolbar Interaction

When the user selects text, the toolbar becomes visible.  
Clicking the **Comment** button creates a temporary state called `pendingComment` inside `toolbarSlice`.

This state represents a comment that is **not yet saved**, but has all the required context.

```json
{
  "pendingComment": {
    "fileId": "file-1",
    "pageNumber": 1,
    "selectedText": "lorem ipsum",
    "position": {
      "x": 0,
      "y": 218.34093349823,
      "pageY": 218.34093349823
    }
  }
}
```

#### Key points:

`fileId` → Identifies the document file

`pageNumber` → Page where the text was selected

`selectedText` → The highlighted text

`position` → Coordinates relative to the document container at the time of selection

### 2. Saving the Comment
When the user submits the comment form:

The pendingComment data is transformed into a persistent comment object.

The comment is stored in the comments state (and later persisted to the backend).

```json
{
  "id": "comment-1764679262043-0.8340228338648052",
  "fileId": "file-1",
  "pageNumber": 1,
  "text": "sfsdfsd",
  "selectedText": "Vestibulum ",
  "position": {
    "x": 0,
    "y": 218.30628204345703,
    "pageY": 218.30628204345703
  },
  "createdAt": "2025-12-02T12:41:02.043Z",
  "updatedAt": "2025-12-02T12:41:02.043Z",
  "resolved": false
}
```
Important:
The position data is preserved exactly as it was during selection to ensure accurate rendering later.
## Component Architecture

### 1. `TextHighlightableDocument.tsx`

The main document component that handles text selection and renders the PDF with overlays.

**Key Responsibilities:**

- Renders the PDF using `react-pdf`
- Detects text selection via `handleMouseUp`
- Dispatches pending highlight/comment states
- Renders `CommentOverlay` for each page
- Renders `InputComment` form when a comment is being created

**Comment-Related Code:**

```tsx
{/* Comment Overlays */}
{pageData && (
  <CommentOverlay
    fileId={file.id}
    pageHeight={pageData.height}
    pageNumber={pageNumber}
    scale={scale}
  />
)}

{/* Comment Input Form */}
{CommentPosition.x !== null &&
  CommentPosition.y !== null &&
  pendingHighlight?.fileId === file.id && <InputComment />}

```

### 2. `Comment.tsx` (Toolbar Button)

The comment button in the toolbar that triggers comment creation.

**Key Responsibilities:**

- Listens for clicks on the "Comment" button
- Calculates the comment position relative to the page
- Creates `pendingComment` state with position data
- Opens the comment input form
- Hides the toolbar

**Position Calculation Logic:**

```tsx
const pageElement = document.querySelector(
  `[data-page-number="${pendingHighlight.pageNumber}"]`
);
const pageRect = pageElement.getBoundingClientRect();
const pageY = position.y; // Toolbar's Y position

dispatch(setPendingComment({
  fileId: pendingHighlight.fileId,
  pageNumber: pendingHighlight.pageNumber,
  selectedText: pendingHighlight.text,
  position: {
    x: 0,
    y: position.y || 0,  // Viewport position
    pageY                // Document-relative position
  }
}));

```

### 3. `CommentOverlay.tsx`

Renders all comment markers for a specific page.

**Key Responsibilities:**

- Filters comments for the current file and page
- Maps over filtered comments and renders `CommentMarker` components
- Provides page context (height, scale) to each marker

**Filtering Logic:**

```tsx
const pageComments = comments.filter(
  (comment) =>
    comment.fileId === fileId &&
    comment.pageNumber === pageNumber
);

```

### 4. `CommentMarker.tsx`

Renders an individual comment marker with expand/collapse functionality.

**Key Responsibilities:**

- Positions the marker icon based on saved coordinates
- Handles expand/collapse of comment card
- Provides edit, resolve, and delete functionality
- Displays comment metadata (author, timestamp, resolved status)

---

## Position Calculation: The Critical Part

The most complex aspect is positioning comments correctly across different zoom levels and pages.

### Challenge

Comments must:

1. Render at the correct vertical position on the page
2. Work across multiple pages in the document
3. Scale properly when the user zooms in/out
4. Remain aligned with the originally selected text

### Solution

We store both **viewport position** (`y`) and **document-relative position** (`pageY`) when creating a comment. When rendering, we convert the stored position to a **percentage** of the page height, making it scale-independent.

### The Calculation (in `CommentMarker.tsx`)

```tsx
const pageNumber = comment.pageNumber;
const pageY = comment.position.pageY;
let topPercentage: number | null = null;

if (comment.pageNumber === 1) {
  // For first page: simple percentage
  topPercentage = (pageY / pageHeight) * 100;
} else {
  // For other pages: account for previous pages' heights
  topPercentage = (
    (pageY - ((pageNumber - 1) * pageHeight)) / pageHeight
  ) * 100;
}

```

**Why This Works:**

1. **First Page (Page 1):**
    - `pageY` is already relative to the page top
    - Simple division by page height gives us the percentage
2. **Subsequent Pages:**
    - `pageY` includes heights of all previous pages
    - We subtract `(pageNumber - 1) * pageHeight` to get position relative to current page
    - Then divide by page height to get percentage
3. **Scale Independence:**
    - Percentages work at any zoom level
    - `pageHeight` adjusts with scale automatically
    - Comment position scales proportionally

### Example Calculation

**Scenario:** Comment on page 2 at pixel position 1060 (with page height 842px)

```
pageY = 1060
pageNumber = 2
pageHeight = 842

topPercentage = ((1060 - ((2 - 1) * 842)) / 842) * 100
              = ((1060 - 842) / 842) * 100
              = (218 / 842) * 100
              = 25.89%

```

The comment renders at 25.89% down the second page, regardless of zoom level.
<!-- ### How Comments Are Rendered
#### Rendering Architecture
Comments are rendered inside the document pages, not globally.

- The `<CommentOverlay />` component is mounted inside:
```tsx
<Document>
  {/* pages */}
  <CommentOverlay />
</Document>
```
- This ensures that comments stay aligned with their respective pages.
---
#### CommentOverlay Logic
The `CommentOverlay` component:

1. Filters comments by:
    - Current document
    - Current page
2. Iterates over the filtered comments.
3. Renders a comment icon positioned horizontally aligned with the selected text.
---
#### Positioning Strategy (The Tricky Part)
Accurate positioning is the most critical part of the system.

**How positioning works:**
- When a comment is created:
    - The (x, y, pageY) coordinates are captured relative to the document container.
- When the document reloads:
    - These saved coordinates are reused to calculate the render position.
- Because both:
    - Selection time
    - Render time
    use the same reference container, the comment icon aligns correctly even after reloads.

This approach ensures:
- Consistent positioning across renders
- No dependency on text re-measurement
- Stable alignment even when zoom or layout changes slightly
---
### Summary
- pendingComment stores temporary selection and position data
- Saved comments persist the exact same positional metadata
- CommentOverlay renders comments inside the document pages
- Positions are always calculated relative to the document container -->
