---
sidebar_position: 2
---

# Architecture


## Project File Structure

```
src/
├─ api/
├─ app/
├─ assets/
├─ components/
├─ context/
├─ features/
│  ├─ bundles-list/
│  │  ├─ api/
│  │  │  └─ bundlesApi.ts
│  │  ├─ components/
│  │  │  ├─ BundleCard.tsx
│  │  │  ├─ BundleRow.tsx
│  │  │  ├─ BundlesFilterbar.tsx
│  │  │  ├─ BundlesHeader.tsx
│  │  │  └─ CreateBundleDialog.tsx
│  │  ├─ redux/
│  │  │  └─ bundlesListSlice.ts
│  │  ├─ types/
│  │  │  └─ types.ts
│  │  └─ Bundles.tsx
│  └─ readme.md
├─ hooks/
├─ layouts/
├─ lib/
├─ pages/
├─ routes/
├─ types/
├─ App.css
├─ App.tsx
├─ index.css
└─ main.tsx

```

## State management

The application uses Redux for centralized state management with the following main slices:

```
const store = configureStore({
  reducer: {
    auth: authReducer,
    fileTree: fileTreeReducer,
    editor: editorReducer,
    propertiesPanel: propertiesPanelReducer,
    toolbar: toolbarReducer,
    indexGenerator: indexGeneratorReducer,
    bundleList: bundlesListReducer,
  },
});
```


## Feature Overview
---
### 📁 Project-Based Organization
Create and manage multiple projects/bundle to keep related documents grouped together. Each project/bundle acts as a dedicated workspace, ensuring files remain structured and easy to navigate.

### 🗂️ File Explorer Sidebar
Browse all project documents through an intuitive file explorer. Quickly open, switch, and manage files without losing context in the editor.

### 📄 Interactive Document Editor
View documents page by page in a clean, distraction-free editor. Designed for precision and readability when working with large or complex files. Know more about [ Document Editor ](/docs//Case-Builder/Frontend/Features/file-editor.md)feature

### 🟡 Highlights
Add coloured hightlights in any page by selecting text. The highlights will appear at the top of the selected text. Know how the [highlight feature](/docs//Case-Builder/Frontend/Features/highlight.md) is implemented.

### 💬 Page-Level Comments & Annotations
Add comments directly to specific pages and positions within a document. This allows you to attach notes exactly where they matter, improving clarity and traceability. Know more about comment feature [here](/docs//Case-Builder/Frontend/Features/comment.md)

### 🎯 Accurate Comment Positioning
Comments are placed with exact coordinates relative to the document page, ensuring annotations stay aligned even when documents are revisited.

### 🔍 Easy Navigation & Focus
Seamlessly move between documents, pages, and comments while maintaining full context of your work.

### 🔐 Secure User Access
User authentication ensures that projects and documents are accessible only to authorized users.

## Convension (Best Practice)
- Use kebab case for folder name
- Use PascalCase in component name
