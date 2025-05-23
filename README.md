# Download book from Gallica and make PDF

Install requirements
```
pip install -r requirements.txt

```
## Edit link in __links.txt__

Then run
```
python main.py

```

---

# OCR pdf

## Installing OCRmyPDF

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
# Choose the -best version
sudo apt-cache search tesseract-ocr
sudo apt-cache search tesseract-ocr | grep Chinese
# get tesseract-ocr-chi-tra-vert-best - tesseract-ocr language files for Chinese - Traditional (vertical) (best)
sudo apt-cache search tesseract-ocr | grep Viet
# get tesseract-ocr-vie-best - tesseract-ocr language files for Vietnamese (best)

# Tesseract Chinese traditional vertical
sudo apt-get install tesseract-ocr-chi-tra-vert-best tesseract-ocr-chi-tra-vert tesseract-ocr-chi-tra-best

# Or just one line
sudo apt-get install tesseract-ocr-chi-tra-vert-best tesseract-ocr-vie* tesseract-ocr-fra* tesseract-ocr-eng

# If pngquant is installed, OCRmyPDF will use it to perform quantize paletted images to reduce their size
sudo apt install pngquant

# Other packages
 sudo apt install ghostscript \
  fonts-droid-fallback \
  jbig2dec \
  unpaper

```

## Run ocrmypdf example:

```
export TMPDIR=$HOME/tmpdir

# Traditional Chinese vertical
# export OPTIONS=' -l chi_tra_vert --jobs 2 --redo-ocr  --tesseract-timeout 600 --output-type pdf '

# French, Vietnemse, Chinese Traditional
export OPTIONS=' -l fra+vie+chi_tra --redo-ocr --tesseract-timeout 600  --output-type pdf '

FILE="your_pdf_file.pdf"
ocrmypdf $OPTIONS "$FILE" "ocr_${FILE}"

# Or you can run this in a folder to make ocr for all pdf files
mkdir -p ocr
find . -printf '%p\n' -name '*.pdf' -exec ocrmypdf $OPTIONS '{}' ocr/'{}' \;

```

## Optimize

### Installing the JBIG2 encoder

https://ocrmypdf.readthedocs.io/en/latest/jbig2.html#jbig2-lossy

https://github.com/ocrmypdf/OCRmyPDF/blob/main/.docker/Dockerfile

```

sudo apt install autotools-dev automake libtool libleptonica-dev gcc

git clone https://github.com/agl/jbig2enc
cd jbig2enc
./autogen.sh
./configure --host=x86_64-pc-linux-gnu
make
sudo make install

```

## Run ocrmypdf example:

```
export TMPDIR=$HOME/tmpdir

# Traditional Chinese vertical
# export OPTIONS=' -l chi_tra_vert --jobs 2 --redo-ocr  --tesseract-timeout 600 --optimize 2 --jbig2-lossy --output-type pdf '

# French, Vietnemse, Chinese Traditional
export OPTIONS=' -l fra+vie+chi_tra --redo-ocr --tesseract-timeout 600  --output-type pdf '

FILE="your_pdf_file.pdf"
ocrmypdf $OPTIONS "$FILE" "ocr_${FILE}"

# Or you can run this in a folder to make ocr for all pdf files
mkdir -p ocr
find . -printf '%p\n' -name '*.pdf' -exec ocrmypdf $OPTIONS '{}' ocr/'{}' \;

```
