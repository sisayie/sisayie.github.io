---
title: 'Step-by-step guide to Developing a Simple Flask Application that Generates QR Codes'
date: 2025-02-28
permalink: /posts/2012/08/Generates-QR-Codes/
tags:
  - QR Codes
  - Python
  - Flask
  - Authomation
---

# Table of content
- [Part I: What is QR Code](#Introduction)
    - [Features of QR Codes](#Key-Features-of-QR-Codes)
    - [Types of QR Codes](#Types-of-QR-Codes)
    - [Structure of a QR Code](#Structure-of-a-QR-Code)
    - [Uses of QR Codes](#Uses-of-QR-Codes)
    - [Advantages of QR Codes](#Advantages-of-QR-Codes)

- [Part II: Generating QR Code](#Generating-QR-Code)
    - [Installing Required Dependencies](#installing-required-dependencies)
    - [Coding](#coding)
    - [Running the Application](#running-the-application)

This article provides an overview of QR Code and a simple python application to generate a QR Code. The first part offers inroduction to QR code and the second part walks you through the steps to the implementation of QR generator in python. If you are interested only to the implementation, you can directly jump to the section [Generating QR Code](#Generating-QR-Code). The full code is available [here](TODO:link).

## [What is QR Code?](#Introduction)
QR codes have become an essential part of many digital interactions, offering an easy way to link offline and online worlds. Whether for business, marketing, or personal use, their ability to store information quickly and be easily scanned makes them a valuable tool. 

[What exactly are QR Codes?](#What-are-QR-Codes?)

A QR code (short for Quick Response code) is a type of two-dimensional barcode that can store a variety of data such as text, URLs, contact information, and other data types. Unlike traditional barcodes, which are one-dimensional and only store information in a single line, a QR code can store much more data due to its two-dimensional structure, allowing for both horizontal and vertical information storage.

[Key Features of QR Codes](#Key-Features-of-QR-Codes)
- Matrix Barcode:
A QR code consists of a grid (matrix) of black and white squares arranged in a square shape. This grid can vary in size, with larger grids storing more data.
- Data Capacity:
QR codes can store much more data than traditional barcodes. For example: A basic QR code can hold around 3,000 characters or 7,000 numeric digits. Depending on the error correction level, it can store text, URLs, phone numbers, or even binary data.
- Error Correction:
QR codes incorporate error correction techniques that allow them to remain readable even if they are partially damaged. This is achieved by using a feature known as the Reed-Solomon error correction algorithm. Different levels of error correction can be applied (from L to H), with higher levels allowing for more damage but reducing the data capacity.
- Speed and Efficiency:
QR codes are designed for quick scanning. Unlike traditional barcodes, which require a scanner to move along the code to read it, QR codes can be scanned from any angle and can be read almost instantly.

[Types of QR Codes](#Types-of-QR-Codes)

- Static QR Codes: These are QR codes whose data cannot be changed once they are created. They can store URLs, text, or other types of information.
- Dynamic QR Codes: These allow for the data to be changed after the code has been printed or distributed. For example, a dynamic QR code might redirect to different URLs based on the context or tracking information.

[Structure of a QR Code](#Structure-of-a-QR-Code)
- Positioning Patterns:
A QR code has three large squares at three corners, which are used to help the scanner locate and orient the code.
- Alignment Patterns:
These help the scanner read the QR code even when it's tilted or distorted.
- Timing Patterns:
These help the scanner detect the size of the QR code matrix and ensure it’s scanned correctly.
- Data and Error Correction Area:
The majority of the QR code consists of data and error correction bits. This is where the actual information is encoded.
- Quiet Zone:
A quiet zone is a border of white space surrounding the QR code, ensuring that the scanner can correctly identify the edges of the code.

[Uses of QR Codes](#Uses-of-QR-Codes)
- URL Links: A common use is to encode URLs (web addresses). When scanned, a user is directed to the website.
- Payment Systems: QR codes are widely used in mobile payment systems, such as WeChat Pay, Alipay, or Apple Pay.
- Event Tickets: QR codes are often used on event tickets, boarding passes, or coupons.
- Product Tracking and Information: Manufacturers can use QR codes for inventory management, shipment tracking, and product information.
- Contact Information: vCards (electronic business cards) can be encoded in QR codes, making it easy for users to scan and save contact info.
- Marketing: QR codes are frequently used in marketing campaigns, advertisements, and posters to link to special offers or promotions.

[Advantages of QR Codes](#Advantages-of-QR-Codes)
- High Data Capacity: QR codes can store much more data compared to traditional barcodes.
- Fast and Easy to Scan: QR codes can be scanned quickly and easily from almost any direction.
- Versatility: They can store various types of data, from simple text to complex URLs or other information.
- Error Tolerance: QR codes can still be read even if they are partially damaged or obscured.

## [Generating QR Code](#Generating-QR-Code)
Now that you understand what a QR code is, let us go through a python application that generates it. We will use a simple Flask application that generates QR codes using the `qrcode` library along with `Flask`. Below is a step-by-step guide to creating the app:

[Step 1: Install dependencies](#installing-required-dependencies)

First, you need to install the necessary libraries. Open your terminal or command prompt and run the following commands:

`pip install Flask qrcode[pil]`

Flask is the web framework and qrcode[pil] is the library used to generate QR codes with support for images (Pillow is included in this package).

[Step 2: Create the Flask application](#coding)

Now, let's write the Flask application. Create a file called app.py and paste the following code inside it:
```Python
from flask import Flask, render_template, request, redirect, url_for
import qrcode
from io import BytesIO
from flask import send_file

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/generate', methods=['POST'])
def generate_qr():
    # Get the data from the form input
    qr_data = request.form.get('qr_data')
    
    # Generate the QR code
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_L,
        box_size=10,
        border=4,
    )
    qr.add_data(qr_data)
    qr.make(fit=True)
    
    # Create an image from the QR Code instance
    img = qr.make_image(fill='black', back_color='white')
    
    # Save the QR code to a BytesIO object
    img_io = BytesIO()
    img.save(img_io, 'PNG')
    img_io.seek(0)
    
    # Return the image as a response
    return send_file(img_io, mimetype='image/png')

if __name__ == '__main__':
    app.run(debug=True)

```

The `'/'` route displays the homepage where users can input data for the QR code.
The `'/generate'` route is where the QR code is generated when the form is submitted. It uses the `qrcode` library to generate the QR code, then returns the image as a PNG file.

The `qrcode.QRCode` class is used to create a QR code object, and we add the data to be encoded using `add_data()`.
We generate the QR code image using `make_image()`, which we then save to a `BytesIO` object and send back as an image response.

Step 3: Create the HTML template

Create a folder named `templates` in the same directory as your `app.py`, and inside it, create a file named `index.html`. This will be the homepage where users can input the data to generate the QR code.

Here is the content of `index.html`:
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Generator</title>
</head>
<body>
    <h1>QR Code Generator</h1>
    <form action="{{ url_for('generate_qr') }}" method="POST">
        <label for="qr_data">Enter data for QR code:</label>
        <input type="text" id="qr_data" name="qr_data" required>
        <button type="submit">Generate QR Code</button>
    </form>
</body>
</html>
```
This simple HTML form sends a POST request to the Flask application with the text input (the data you want to encode in the QR code).

[Step 4: Run the application](#running-the-application)

Now, go back to your terminal and run the Flask app with this command:

`python app.py`

Your Flask application should be running at `http://127.0.0.1:5000/`. When you visit that URL, you will see a form where you can input some text, and the app will generate a QR code for that text when you submit the form.

---
Congratulations! 

You have created a Flask application that generates QR codes! The QR code is dynamically generated and sent back as an image in response to a form submission. 
You want to create your QR code or connect with me, scan the QR code below.
<p>
<img src="https://sisayie.github.io/images/SisayChalaQRCode.png" alt="https://sisayie.github.io/">
</p>