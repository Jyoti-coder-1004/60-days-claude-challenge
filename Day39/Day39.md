# Day 39: Premium Browser-Based PDF Splitter & Merger

## Project Overview
A client-side, premium browser-based PDF utility built entirely from scratch with a glassmorphic aesthetic. This tool allows users to safely split and merge PDF documents completely locally within the browser, utilizing Web Workers and modern libraries (`pdf-lib`, `pdfjs-dist`) for ultra-fast compilation without uploading files to any external backend server.

---

## 💻 Source Code (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Suite - Premium Client-Side Splitter & Merger</title>
    <!-- pdf-lib and PDF.js CDN libraries -->
    <script src="[https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js](https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js)"></script>
    <script src="[https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.4.120/pdf.min.js](https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.4.120/pdf.min.js)"></script>
    <style>
        :root {
            --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
            --panel-bg: rgba(30, 41, 59, 0.7);
            --border-color: rgba(255, 255, 255, 0.1);
            --accent-primary: #6366f1;
            --accent-hover: #4f46e5;
            --text-main: #f8fafc;
            --text-sub: #94a3b8;
            --danger: #ef4444;
            --success: #10b981;
        }

        .light-mode {
            --bg-gradient: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
            --panel-bg: rgba(255, 255, 255, 0.85);
            --border-color: rgba(0, 0, 0, 0.08);
            --accent-primary: #4f46e5;
            --accent-hover: #4338ca;
            --text-main: #0f172a;
            --text-sub: #475569;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', system-ui, sans-serif; }
        body { background: var(--bg-gradient); color: var(--text-main); min-height: 100vh; display: flex; flex-direction: column; transition: background 0.3s; }
        
        header { display: flex; justify-content: space-between; align-items: center; padding: 1.5rem 2rem; border-bottom: 1px solid var(--border-color); backdrop-filter: blur(10px); }
        .logo { font-size: 1.5rem; font-weight: 700; letter-spacing: -0.05em; display: flex; align-items: center; gap: 0.5rem; }
        .logo span { color: var(--accent-primary); }

        .tabs { display: flex; gap: 1rem; justify-content: center; margin: 2rem 0; }
        .tab-btn { background: var(--panel-bg); border: 1px solid var(--border-color); color: var(--text-sub); padding: 0.75rem 2rem; border-radius: 50px; cursor: pointer; font-weight: 600; transition: all 0.2s; backdrop-filter: blur(5px); }
        .tab-btn.active { background: var(--accent-primary); color: white; border-color: var(--accent-primary); box-shadow: 0 4px 12px rgba(99, 102, 241, 0.3); }

        main { flex: 1; max-width: 1200px; width: 100%; margin: 0 auto; padding: 0 2rem 3rem 2rem; }
        .tool-panel { display: none; background: var(--panel-bg); border: 1px solid var(--border-color); border-radius: 24px; padding: 2.5rem; backdrop-filter: blur(16px); box-shadow: 0 8px 32px rgba(0,0,0,0.2); animation: fadeInUp 0.4s ease-out forwards; }
        .tool-panel.active { display: block; }

        .dropzone { border: 2px dashed var(--border-color); border-radius: 16px; padding: 3rem 2rem; text-align: center; cursor: pointer; transition: all 0.2s; background: rgba(0,0,0,0.1); }
        .dropzone:hover { border-color: var(--accent-primary); background: rgba(99, 102, 241, 0.05); }
        .dropzone p { margin-top: 1rem; color: var(--text-sub); }

        .preview-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 1.5rem; margin-top: 2rem; max-height: 380px; overflow-y: auto; padding-right: 0.5rem; }
        .thumbnail-card { background: rgba(0,0,0,0.2); border: 1px solid var(--border-color); border-radius: 12px; padding: 0.5rem; position: relative; text-align: center; }
        .thumbnail-card canvas { width: 100%; height: auto; border-radius: 8px; background: white; }
        .thumbnail-card span { display: block; font-size: 0.8rem; margin-top: 0.5rem; color: var(--text-sub); }
        .thumbnail-card .remove-btn { position: absolute; top: -8px; right: -8px; background: var(--danger); border: none; border-radius: 50%; width: 22px; height: 22px; color: white; font-size: 12px; cursor: pointer; display: flex; align-items: center; justify-content: center; }

        .controls-group { margin-top: 2rem; background: rgba(0,0,0,0.15); padding: 1.5rem; border-radius: 16px; border: 1px solid var(--border-color); }
        label { display: block; font-weight: 600; margin-bottom: 0.5rem; font-size: 0.9rem; text-transform: uppercase; color: var(--text-sub); }
        input[type="text"], input[type="number"] { width: 100%; background: rgba(0,0,0,0.3); border: 1px solid var(--border-color); padding: 0.75rem 1rem; border-radius: 8px; color: var(--text-main); font-size: 1rem; outline: none; }
        input:focus { border-color: var(--accent-primary); }

        .btn-action { background: var(--accent-primary); color: white; border: none; padding: 1rem 2.5rem; border-radius: 12px; font-size: 1rem; font-weight: 600; cursor: pointer; width: 100%; margin-top: 1.5rem; transition: background 0.2s; }
        .btn-action:hover { background: var(--accent-hover); }

        .loader { display: none; align-items: center; justify-content: center; gap: 0.75rem; margin-top: 1rem; color: var(--text-sub); }
        .spinner { width: 20px; height: 20px; border: 3px solid transparent; border-top-color: var(--accent-primary); border-radius: 50%; animation: spin 0.8s linear infinite; }

        @keyframes spin { to { transform: rotate(360deg); } }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <header>
        <div class="logo">🗂️ PDF<span>Suite</span></div>
        <button class="tab-btn" onclick="document.body.classList.toggle('light-mode')">🌓 Toggle Theme</button>
    </header>

    <div class="tabs">
        <button class="tab-btn active" onclick="switchTab('split', this)">✂️ Split PDF</button>
        <button class="tab-btn" onclick="switchTab('merge', this)">🔗 Merge PDF</button>
    </div>

    <main>
        <!-- SPLIT PANEL -->
        <div id="split-panel" class="tool-panel active">
            <div class="dropzone" onclick="document.getElementById('split-input').click()">
                <input type="file" id="split-input" accept="application/pdf" style="display: none;" onchange="handleSplitUpload(this.files[0])">
                <h3>Select or Drop Target PDF</h3>
                <p>Processes completely offline & locally</p>
            </div>
            <div id="split-preview-grid" class="preview-grid"></div>
            <div class="controls-group">
                <label for="split-ranges">Split Method Ranges (e.g. 1-2, 3-5 or 2, 4, 6)</label>
                <input type="text" id="split-ranges" placeholder="Enter custom ranges or precise comma-separated pages">
                <button class="btn-action" onclick="executeSplit()">Process & Download Splits</button>
                <div id="split-loader" class="loader"><div class="spinner"></div> Splitting documents pipeline active...</div>
            </div>
        </div>

        <!-- MERGE PANEL -->
        <div id="merge-panel" class="tool-panel">
            <div class="dropzone" onclick="document.getElementById('merge-input').click()">
                <input type="file" id="merge-input" multiple accept="application/pdf" style="display: none;" onchange="handleMergeUpload(this.files)">
                <h3>Upload Multiple PDF Files</h3>
                <p>Drag & drop multi-file inputs to configure compile row</p>
            </div>
            <div id="merge-preview-grid" class="preview-grid"></div>
            <div class="controls-group">
                <div style="display:flex; justify-content:space-between; color: var(--text-sub); margin-bottom:1rem;">
                    <span>Queued Files: <strong id="merge-count" style="color:var(--text-main)">0</strong></span>
                    <span>Aggregated Pages: <strong id="merge-pages" style="color:var(--text-main)">0</strong></span>
                </div>
                <button class="btn-action" onclick="executeMerge()">Merge & Download PDF</button>
                <div id="merge-loader" class="loader"><div class="spinner"></div> Assembling binary array blocks...</div>
            </div>
        </div>
    </main>

    <script>
        pdfjsLib.GlobalWorkerOptions.workerSrc = '[https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.4.120/pdf.worker.min.js](https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.4.120/pdf.worker.min.js)';
        
        let splitFileBytes = null;
        let splitFileName = "";
        let mergeQueue = [];

        function switchTab(type, btn) {
            document.querySelectorAll('.tab-btn').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tool-panel').forEach(p => p.classList.remove('active'));
            btn.classList.add('active');
            document.getElementById(`${type}-panel`).classList.add('active');
        }

        async function handleSplitUpload(file) {
            if (!file) return;
            splitFileName = file.name.replace(".pdf", "");
            const reader = new FileReader();
            reader.onload = async function(e) {
                splitFileBytes = new Uint8Array(e.target.result);
                renderThumbnails(splitFileBytes, 'split-preview-grid');
            };
            reader.readAsArrayBuffer(file);
        }

        async function renderThumbnails(bytes, containerId) {
            const container = document.getElementById(containerId);
            container.innerHTML = "";
            try {
                const loadingTask = pdfjsLib.getDocument({data: bytes});
                const pdf = await loadingTask.promise;
                
                for(let i = 1; i <= pdf.numPages; i++) {
                    const page = await pdf.getPage(i);
                    const viewport = page.getViewport({scale: 0.3});
                    
                    const card = document.createElement('div');
                    card.className = 'thumbnail-card';
                    
                    const canvas = document.createElement('canvas');
                    const context = canvas.getContext('2d');
                    canvas.height = viewport.height;
                    canvas.width = viewport.width;
                    
                    await page.render({canvasContext: context, viewport: viewport}).promise;
                    
                    card.appendChild(canvas);
                    card.appendChild(document.createTextNode(`Page ${i}`));
                    container.appendChild(card);
                }
            } catch (err) { alert("Error displaying document visual canvas arrays."); }
        }

        async function executeSplit() {
            if(!splitFileBytes) return alert("Upload a valid file before initializing pipeline operations.");
            const rangeStr = document.getElementById('split-ranges').value.trim();
            if(!rangeStr) return alert("Provide validation indices pattern ranges.");

            const loader = document.getElementById('split-loader');
            loader.style.display = 'flex';

            try {
                const sourcePdf = await PDFLib.PDFDocument.load(splitFileBytes);
                const totalPages = sourcePdf.getPageCount();
                const ranges = rangeStr.split(',');

                for (let range of ranges) {
                    const cleanRange = range.trim();
                    let pageIndices = [];

                    if (cleanRange.includes('-')) {
                        const [start, end] = cleanRange.split('-').map(Number);
                        for (let i = start; i <= end; i++) { if(i <= totalPages) pageIndices.push(i - 1); }
                    } else {
                        const pNum = Number(cleanRange);
                        if (pNum <= totalPages) pageIndices.push(pNum - 1);
                    }

                    if (pageIndices.length > 0) {
                        const newPdf = await PDFLib.PDFDocument.create();
                        const copiedPages = await newPdf.copyPages(sourcePdf, pageIndices);
                        copiedPages.forEach(p => newPdf.addPage(p));
                        const savedBytes = await newPdf.save();
                        downloadBlob(savedBytes, `${splitFileName}_split_${cleanRange}.pdf`);
                    }
                }
            } catch(e) { alert("Execution aborted: verify string structural index parameters format match."); }
            loader.style.display = 'none';
        }

        async function handleMergeUpload(files) {
            for(let file of files) {
                const reader = new FileReader();
                await new Promise((resolve) => {
                    reader.onload = async function(e) {
                        const bytes = new Uint8Array(e.target.result);
                        const doc = await PDFLib.PDFDocument.load(bytes);
                        mergeQueue.push({ name: file.name, bytes, pages: doc.getPageCount() });
                        resolve();
                    };
                    reader.readAsArrayBuffer(file);
                });
            }
            updateMergeUI();
        }

        function updateMergeUI() {
            const grid = document.getElementById('merge-preview-grid');
            grid.innerHTML = "";
            let totalPages = 0;
            
            mergeQueue.forEach((item, index) => {
                totalPages += item.pages;
                const card = document.createElement('div');
                card.className = 'thumbnail-card';
                card.style.padding = "1.5rem 0.5rem";
                
                const deleteBtn = document.createElement('button');
                deleteBtn.className = 'remove-btn';
                deleteBtn.textContent = '×';
                deleteBtn.onclick = () => { mergeQueue.splice(index, 1); updateMergeUI(); };
                
                const title = document.createElement('div');
                title.style.fontSize = '0.85rem';
                title.style.fontWeight = '600';
                title.style.overflow = 'hidden';
                title.textContent = item.name;

                const counts = document.createElement('span');
                counts.textContent = `${item.pages} Pages`;

                card.appendChild(deleteBtn);
                card.appendChild(title);
                card.appendChild(counts);
                grid.appendChild(card);
            });

            document.getElementById('merge-count').textContent = mergeQueue.length;
            document.getElementById('merge-pages').textContent = totalPages;
        }

        async function executeMerge() {
            if(mergeQueue.length < 2) return alert("Queue at least 2 separate documents to run merging operation configurations.");
            const loader = document.getElementById('merge-loader');
            loader.style.display = 'flex';

            try {
                const targetPdf = await PDFLib.PDFDocument.create();
                for(let fileObj of mergeQueue) {
                    const currentDoc = await PDFLib.PDFDocument.load(fileObj.bytes);
                    const indexes = Array.from({length: currentDoc.getPageCount()}, (_, i) => i);
                    const pages = await targetPdf.copyPages(currentDoc, indexes);
                    pages.forEach(p => targetPdf.addPage(p));
                }
                const mergedBytes = await targetPdf.save();
                downloadBlob(mergedBytes, "compiled_archive_package.pdf");
            } catch(err) { alert("Failed structural payload mapping compile sequences."); }
            loader.style.display = 'none';
        }

        function downloadBlob(bytes, filename) {
            const blob = new Blob([bytes], {type: "application/pdf"});
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = filename;
            link.click();
        }
    </script>
</body>
</html>