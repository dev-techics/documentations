---
sidebar_position: 2
---

# Architecture

## Flow

1. Register/Login
   ↓
2. Dashboard/bundles
    - create bundle
    - click to open it in the editor
    ↓
3. Editor
    - import files
    - add highlight/comment/page number etc.
    - export

## Feature Hierarchy

```
src/
├─ api/
│  └─ axiosInstance.ts
├─ app/
│  ├─ hooks.ts
│  └─ store.ts
├─ assets/
│  └─ react.svg
├─ components/
│  ├─ dashboard/
│  │  ├─ redux/
│  │  ├─ DashboardSidebar.tsx
│  │  ├─ SidebarFooterMenu.tsx
│  │  ├─ SidebarMenu.tsx
│  │  └─ Topbar.tsx
│  ├─ landing-page/
│  │  └─ LandingPage.tsx
│  ├─ sidebars/
│  │  ├─ LeftSidebar.tsx
│  │  └─ RightSidebar.tsx
│  ├─ ui/
│  │  ├─ alert-dialog.tsx
│  │  ├─ alert.tsx
│  ├─ ExportButton.tsx
│  ├─ Footer.tsx
│  ├─ Header.tsx
│  └─ NotFound.tsx
├─ context/
│  └─ SidebarContext.tsx
├─ features/
│  ├─ auth/
│  │  ├─ components/
│  │  │  ├─ LoginForm.tsx
│  │  │  ├─ RegisterForm.tsx
│  │  │  └─ UserDropdown.tsx
│  │  ├─ hooks/
│  │  │  └─ useAuthInit.ts
│  │  ├─ redux/
│  │  │  ├─ authApi.ts
│  │  │  └─ authSlice.ts
│  │  ├─ types/
│  │  │  └─ types.ts
│  │  └─ readme.md
│  ├─ auto-index/
│  │  ├─ components/
│  │  │  ├─ IndexInitializer.tsx
│  │  │  ├─ IndexPageWrapper.tsx
│  │  │  └─ IndexPDFPreview.tsx
│  │  ├─ hooks/
│  │  │  ├─ useGenerateIndex.ts
│  │  │  └─ useGenerateIndexPDF.ts
│  │  ├─ utils/
│  │  │  └─ generateIndexPDF.ts
│  │  ├─ autoIndexSlice.ts
│  │  ├─ readme.md
│  │  └─ types.ts
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
│  ├─ editor/
│  │  ├─ components/
│  │  │  ├─ ui/
│  │  │  │  └─ ErrorComp.tsx
│  │  │  ├─ Document.tsx
│  │  │  ├─ Header.tsx
│  │  │  ├─ UploadFile.tsx
│  │  │  └─ ZoomControls.tsx
│  │  ├─ hooks/
│  │  │  └─ PdfWithHeaderFooter.tsx
│  │  ├─ Editor.tsx
│  │  ├─ editorSlice.ts
│  │  ├─ helpers.ts
│  │  └─ types.ts
│  ├─ file-explorer/
│  │  ├─ components/
│  │  │  ├─ ui/
│  │  │  │  └─ input.tsx
│  │  │  ├─ CreateNewFolder.tsx
│  │  │  ├─ FileActionMenu.test.tsx
│  │  │  ├─ FileActionMenu.tsx
│  │  │  ├─ FileExplorerHeader.tsx
│  │  │  ├─ FileItemWrapper.tsx
│  │  │  ├─ FilesTree.tsx
│  │  │  ├─ fileUploadHandler.tsx
│  │  │  ├─ SortableFileItem.tsx
│  │  │  └─ SortableFolderItem.tsx
│  │  ├─ hooks/
│  │  │  └─ hooks.ts
│  │  ├─ FileExplorer.tsx
│  │  ├─ fileTreeSlice.ts
│  │  ├─ index.ts
│  │  └─ types.ts
│  ├─ properties-panel/
│  │  ├─ components/
│  │  │  ├─ Annotations.tsx
│  │  │  ├─ DocumentSettings.tsx
│  │  │  └─ Exports.tsx
│  │  ├─ propertiesPanelSlice.ts
│  │  ├─ sidebar.tsx
│  │  └─ types.ts
│  ├─ sidebar/
│  │  ├─ components/
│  │  │  └─ sidebar/
│  │  │     └─ MainSidebar.tsx
│  │  ├─ EditorSidebar.tsx
│  │  └─ index.ts
│  ├─ toolbar/
│  │  ├─ components/
│  │  │  ├─ ColorPicker.tsx
│  │  │  ├─ Comment.tsx
│  │  │  ├─ CommentCard.tsx
│  │  │  ├─ CommentMarker.tsx
│  │  │  ├─ CommentOverlay.tsx
│  │  │  ├─ Highlight.tsx
│  │  │  ├─ HighlightOverlay.tsx
│  │  │  └─ InputComment.tsx
│  │  ├─ types/
│  │  │  └─ types.ts
│  │  ├─ Toolbar.tsx
│  │  └─ toolbarSlice.ts
│  └─ readme.md
├─ hooks/
│  └─ use-mobile.ts
├─ layouts/
│  ├─ AuthLayout.tsx
│  ├─ DashboardLayout.tsx
│  ├─ EditorLayout.tsx
│  └─ LandingPageLayout.tsx
├─ lib/
│  ├─ pdfCoordinateUtils.tsx
│  └─ utils.ts
├─ pages/
│  ├─ auth/
│  │  ├─ SignInPage.tsx
│  │  └─ SignUpPage.tsx
│  ├─ dashboard/
│  │  ├─ BundlesPage.tsx
│  │  └─ DashboardPage.tsx
│  ├─ editor/
│  │  └─ EditorPage.tsx
│  └─ landing/
│     └─ LandingPage.tsx
├─ routes/
│  ├─ AppRoutes.tsx
│  └─ ProtectedRoutes.tsx
├─ types/
│  └─ types.tsx
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
    fileTree: fileTreeSlice,
    editor: editorSlice,
    propertiesPanel: propertiesPanelSlice,
    toolbar,
    indexGenerator,
    bundleList: bundlesListSlice,
  },
});
```


## Feature Overview
---
### 📁 Project-Based Organization
Create and manage multiple projects to keep related documents grouped together. Each project acts as a dedicated workspace, ensuring files remain structured and easy to navigate.

### 🗂️ File Explorer Sidebar
Browse all project documents through an intuitive file explorer. Quickly open, switch, and manage files without losing context in the editor.

### 📄 Interactive Document Editor
View documents page by page in a clean, distraction-free editor. Designed for precision and readability when working with large or complex files.

### 💬 Page-Level Comments & Annotations
Add comments directly to specific pages and positions within a document. This allows you to attach notes exactly where they matter, improving clarity and traceability.

### 🎯 Accurate Comment Positioning
Comments are placed with exact coordinates relative to the document page, ensuring annotations stay aligned even when documents are revisited.

### 🔍 Easy Navigation & Focus
Seamlessly move between documents, pages, and comments while maintaining full context of your work.

### 🔐 Secure User Access
User authentication ensures that projects and documents are accessible only to authorized users.


