# NotebookLM PDF 轉可編輯投影片流程（繁體中文 OCR）

本文件提供一套從 NotebookLM 輸出的 PDF，將內嵌圖片中的繁體中文辨識為可編輯文字，並轉成投影片（PPTX）的流程與範例腳本。

## 目標
1. 將 PDF 圖片中的繁體中文做 OCR，變成可編輯文字。
2. 把每頁 PDF 內容整理成投影片，文字可編輯、圖片可保留。

## 依賴安裝

### 系統工具
- **Tesseract OCR**（含繁體中文語言包）
- **Ghostscript**（供 ocrmypdf 使用）

macOS (Homebrew):
```bash
brew install tesseract tesseract-lang ghostscript
```

Ubuntu/Debian:
```bash
sudo apt-get update
sudo apt-get install -y tesseract-ocr tesseract-ocr-chi-tra ghostscript
```

### Python 套件
```bash
python -m pip install ocrmypdf pymupdf pdfplumber pytesseract python-pptx
```

## 步驟 1：先為 PDF 建立可搜尋的 OCR 文字層

```bash
ocrmypdf -l chi_tra --force-ocr --deskew input.pdf output_ocr.pdf
```

- `chi_tra`：繁體中文語言。
- `--force-ocr`：即使 PDF 內已有文字也重新辨識影像。
- `--deskew`：自動校正傾斜。

## 步驟 2：將 OCR 後 PDF 轉為 PPTX（文字可編輯）

以下是範例腳本，會：
1. 讀取每頁文字（從 OCR 文字層）。
2. 擷取圖片，並對圖片做繁體中文 OCR。
3. 建立投影片，加入可編輯文字與圖片。

### 使用方式
```bash
python pdf_to_pptx.py output_ocr.pdf output.pptx
```

## 注意事項
- OCR 正確率會受到圖片清晰度、字體與排版影響。
- 建議先以 `ocrmypdf` 做品質提升再輸出投影片。
- 產出後仍可在 PowerPoint 內人工微調。
