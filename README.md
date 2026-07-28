# Behavior Data Collection — Static GitHub Pages Edition

This edition runs entirely in the user's browser. It does not require Python, a local server, package installation, or a database.

## Publish with GitHub Pages

1. Upload the contents of this folder to the root of a GitHub repository.
2. In **Settings → Pages**, select **Deploy from a branch**.
3. Select the `main` branch and `/ (root)` folder.
4. Open the Pages address GitHub provides.

Keep these paths unchanged:

- `index.html`
- `assets/jszip.min.js`
- `templates/behavior-record-template.xlsx`

## Privacy design

- Student data is entered and processed in browser memory.
- Loading a workbook uses the browser's local file reader.
- Exporting modifies the included Excel template locally and downloads the result.
- No student data is posted to GitHub or to an application server.
- The repository must contain only the blank application and blank workbook template.
- Do not commit completed student workbooks to the repository.

The blue Archway loading animation is requested directly from the WSOC Portal Assets repository. It receives only a normal image request; no student record is included in that request. If the image cannot load, the application continues working.

## Workbook

The exported workbook includes native Excel charts, Student Dashboard, Daily Summary, Behavior Trends, Clinical Notes, Signatures audit trail, Day Records, Interval Data, Definitions & Directions, and hidden application metadata.

## Signatures

Drawn signatures are captured in the browser and are embedded in the exported workbook as PNG picture objects on the Signatures sheet. Before being embedded, each signature is composited on a canvas with a caption printed beneath it that reads `Staff — Signed: <date and time>` (or `Supervisor — Signed: <date and time>`). The stamped date is the timestamp captured at the moment the staff or supervisor lifted the pen, so a printed copy of the workbook always shows when each signature was applied. The raw drawn signature is still preserved in the hidden application metadata so it reloads correctly into the app.

## Import/export

The blank Excel template is embedded directly inside `index.html`. Export works on GitHub Pages and when `index.html` is opened directly from the downloaded folder. Import and export use the included `assets/jszip.min.js` file and run entirely inside the browser. Keep the folder structure intact when uploading.

## Excel compatibility

- Legacy merged placeholder cells that overlapped the Daily Summary Excel table are cleared during export, so `table3.xml` is not left dangling.
- Daily Summary is exported as a formatted range rather than the incompatible `table3` structured table.
- The signature drawing part is referenced from the Signatures sheet exactly once, in the correct schema position (before `<tableParts>`), fixing an earlier "We found a problem with some content" repair warning caused by a duplicate `<drawing>` reference.
- Signature picture anchors use `oneCellAnchor` with a fixed pixel extent, so no negative anchor offsets are ever written.

## Code notes (maintenance)

The workbook engine is implemented once, using JSZip. An earlier build shipped a second, unused ExcelJS-based engine alongside it; that dead code path has been removed. All import/export goes through the JSZip engine only.
