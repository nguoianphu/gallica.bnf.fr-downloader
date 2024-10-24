# Download book from Gallica and make PDF

```
pip install -r requirements.txt

```
## Edit link in __links.txt__

```
python main.py

```

---

# OCR pdf

Installing OCRmyPDF

```
sudo apt update
sudo apt install ocrmypdf
# Fonts for Chinese
sudo apt-get install fonts-arphic-ukai fonts-arphic-uming fonts-ipafont-mincho fonts-ipafont-gothic fonts-unfonts-core


# Tesseract
echo "deb https://notesalexp.org/tesseract-ocr5/$(lsb_release -cs)/ $(lsb_release -cs) main" \
| sudo tee /etc/apt/sources.list.d/notesalexp.list > /dev/null
sudo apt update --allow-insecure-repositories

# https://ocrmypdf.readthedocs.io/en/latest/languages.html#lang-packs
# Display a list of all Tesseract language packs
sudo apt-cache search tesseract-ocr

# Tesseract Chinese traditional vertical
sudo apt-get install tesseract-ocr-chi-tra-vert-best tesseract-ocr-chi-tra-vert tesseract-ocr-chi-tra-best

```

Run ocrmypdf example:

```
export TMPDIR=$HOME/tmpdir
export OPTIONS=' -l chi_tra_vert --jobs 2 --redo-ocr  --tesseract-timeout 600 --output-type pdf '

FILE="your_pdf_file.pdf"
ocrmypdf $OPTIONS "$FILE" "ocr_${FILE}"

# Or you can run this in a folder to make ocr for all pdf files
find . -printf '%p\n' -name '*.pdf' -exec ocrmypdf $OPTIONS '{}' '{}' \;

```