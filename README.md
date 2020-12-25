# Image-PDF-Text-Extract
# Extract Text from Image PDF

# Import libraries
from PIL import Image
import pytesseract
import sys
from pdf2image import convert_from_path
import os

# Path of the pdf
PDF_file = "bill.pdf"
 
# Converting PDF to images and Store all the pages of the PDF in a variable
# We should have to provide a path of Poppler

pages = convert_from_path(PDF_file, 500,poppler_path=r'C:\Program Files\poppler-0.68.0\bin')

# Counter to store images of each page of PDF to image
image_counter = 1

# Iterate through all the pages stored above
for page in pages:
    filename = "page_" + str(image_counter) + ".jpg"
    page.save(filename, 'JPEG')
    image_counter = image_counter + 1

# Recognizing text from the images using OCR 

# Variable to get count of total number of pages
filelimit = image_counter - 1
print("Image Count",filelimit)

# Creating a text file to write the output
outfile = "out_text.txt"

# Open the file in append mode so that
# All contents of all images are added to the same file
f = open(outfile, "a")

# Iterate from 1 to total number of pages
# Recognize the text as string in image using pytesserct
# Finally, write the processed text to the file.
for i in range(1, filelimit + 1):
    filename = "page_" + str(i) + ".jpg"
    pytesseract.pytesseract.tesseract_cmd = 'C:\\Program Files (x86)\\Tesseract-OCR\\tesseract'  
    text = str(((pytesseract.image_to_string(Image.open(filename)))))
    text = text.replace('-\n', '')
    f.write(text)

# Close the file after writing all the text.
f.close()

