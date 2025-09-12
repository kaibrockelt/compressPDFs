# compressPDFs
A batch processor for squashing the file sizes of all PDFs in a given folder. Doesn't touch originals, outputs in subfolders.

No need to send your files to a server on the internet just to compress. Keep your sensitive data private! This tool works locally on your machine. 

## Setup
### 1 Get ghostScript 
Make sure you have ghostscript on your machine  
**mac OS**  
````
brew install ghostscript
````
**Debian linux and alike, as well as linux subsystem for Windows**  
````
sudo apt install ghostscript
````
**fedora, redhead and alike linux**  
````
sudo dnf install ghostscript
````
### 2 Clone this repo
````
git clone git@github.com:kaibrockelt/compressPDFs.git
````
### 3 Move to a PATH-accessible folder and make executable:
Moving the file  
````
sudo mv ./compressPDFs/compressPDFs /usr/local/bin/
````

making it executable

````
sudo chmod +x /usr/local/bin/compressPDFs`
````
## Usage
to use it in default mode ("normal" compression, grayscale output), call compressPDFs passing a folder to look for PDFs  
working in the current folder looks like this
````
% compressPDFs .
Compressing Fahrzeugschein Audi.pdf (gray, normal)... Success! (744 KB →  696 KB,  7 % Saved)
Compressing Scanned Document.pdf (gray, normal)... Success! (624 KB →  80 KB,  88 % Saved)
---
DONE! 2 PDF(s) processed, 0 failed.
Results in: ./compressed_gray_normal
`````

Pointing at both absolute and relative paths works.
`````
compressPDFs /absolute/path/to/pdfs/
compressPDFs ./relative/path/
`````
## Output
All compressed pdfs will end up in a subfolder "compressed+mode+compression", like `compressed_gray_normal` for the default.




## Tweaking compressPDFs with paramters
compressPDF has two built in parameters:
- `--compression [brutal | aggressive | normal | slight | least]`  
  defines how far we go with compression. This will control the downsampling of imagedata

- `--mode [color | gray | bw]`  
  changes the colorspace output, and thus can also affect the file size


  **you can use one or both of them them like so:**  
`````
compressPDFs ./mySubFolder/ --mode bw --compression aggressive

`````

### Here's an overview of the parameters available:  
#### **Compression Modes**
   Mode      | PDFSETTINGS | Image Resolution (DPI) | JPEG Quality | Use Case                     |
 |-----------|-------------|------------------------|--------------|------------------------------|
 | **brutal**    | `/screen`   | 80                     | 30           | Minimal size (e.g., web previews) |
 | **aggressive**| `/ebook`    | 100                    | 50           | Small files (e.g., mobile devices) |
 | **normal**    | `/ebook`    | 150                    | 70           | Balanced quality/size        |
 | **slight**    | `/ebook`    | 200                    | 80           | High quality (e.g., printing) |
 | **least**     | `/ebook`    | 300                    | 90           | Near-lossless (archiving)     |

*Resolution applies to color/gray/mono images. `PDFSETTINGS` controls internal optimizations (e.g., font subsetting).*

---

#### **Color Modes**
 | Mode      | Color Space       | Conversion Strategy          | Notes                                  |
 |-----------|-------------------|------------------------------|----------------------------------------|
 | **color** | `DeviceRGB`       | `/RGB`                       | Preserves full color                   |
 | **gray**  | `DeviceGray`      | `Gray` + `OverrideICC=true`  | Converts all colors to grayscale       |
 | **bw**    | `DeviceGray`      | `Gray` + FlateEncode filters | Optimized for black/white (e.g., scans) |

*All modes use `-dCompatibilityLevel=1.4` for broad compatibility.*


## How you can contribute
If you have better or new proposals for modes or compression levels, feel free to drop a PR. 

## what i might still work on:
I might drop a "twin" of it for singular PDF files that would work like so:
`````
compressPDF ./folder/MySinglePDF.pdf --mode gray --compression slight
`````

# License:
fully open source. 
If you feel like you have to, you can buy me a ~beer~ [coffee](https://buymeacoffee.com/cleanbites).
But besides that, enjoy freely. Copy, republish, use commercially,.. i don't care.
