# PayNow QR Code Generator Usage Guide

This guide explains how to use the `PaynowQR` class to generate PayNow QR codes in Kotlin.

[Generated with Claude 3.5 Sonnet]

## Table of Contents
1. [Installation](#installation)
2. [Basic Usage](#basic-usage)
3. [Configuration Options](#configuration-options)
4. [Example](#example)
5. [Output](#output)
6. [Note on QR Code Generation](#note-on-qr-code-generation)

## Installation

Ensure you have the `PaynowQR` class in your Kotlin project. You can copy the class definition into your project or include it as a separate file.

## Basic Usage

To use the `PaynowQR` class, follow these steps:

1. Import the `PaynowQR` class in your Kotlin file.
2. Create a `Map<String, Any>` with the necessary configuration options.
3. Instantiate a `PaynowQR` object with the configuration map.
4. Call the `output()` method to get the QR code string.

Here's a basic example:

```kotlin
val qr = PaynowQR(mapOf(
    "uen" to "201403121W",
    "amount" to 10.50
))
val qrString = qr.output()
println(qrString)
```

## Configuration Options

The `PaynowQR` class accepts the following configuration options:

- `uen` (String, required): The Unique Entity Number of the receiving company.
- `amount` (Double, optional): The payment amount. If not provided, defaults to 0.
- `editable` (Boolean, optional): Whether the payment amount is editable. Defaults to false if amount is provided, true otherwise.
- `expiry` (String, optional): The expiry date of the QR code in "YYYYMMDD" format. If not provided, defaults to 5 years from the current date.
- `company` (String, optional): The name of the receiving company. Defaults to "COMPANY" if not provided.
- `refNumber` (String, optional): A reference number for the transaction.

## Example

Here's a more comprehensive example demonstrating all available options:

```kotlin
val qr = PaynowQR(mapOf(
    "uen" to "201403121W",
    "amount" to 10.50,
    "editable" to true,
    "expiry" to "20251231",
    "company" to "Acme Pte Ltd",
    "refNumber" to "INV-001"
))
val qrString = qr.output()
println(qrString)
```

## Output

The `output()` method returns a string representing the PayNow QR code data. This string contains all the necessary information encoded according to the PayNow QR code specification.

Example output:
```
00020101021226550009SG.PAYNOW010200...
```

## Note on QR Code Generation

The `PaynowQR` class generates the data string for a PayNow QR code, but it does not create the actual QR code image. To create a scannable QR code, you'll need to use a QR code generation library that can convert this string into an image.

For example, you might use a library like ZXing to generate the QR code image:

```kotlin
import com.google.zxing.BarcodeFormat
import com.google.zxing.qrcode.QRCodeWriter
import java.awt.image.BufferedImage

fun generateQRCodeImage(text: String, width: Int, height: Int): BufferedImage {
    val qrCodeWriter = QRCodeWriter()
    val bitMatrix = qrCodeWriter.encode(text, BarcodeFormat.QR_CODE, width, height)
    val image = BufferedImage(width, height, BufferedImage.TYPE_INT_RGB)
    for (x in 0 until width) {
        for (y in 0 until height) {
            image.setRGB(x, y, if (bitMatrix[x, y]) 0xFF000000.toInt() else 0xFFFFFFFF.toInt())
        }
    }
    return image
}

// Usage
val qrString = PaynowQR(mapOf("uen" to "201403121W", "amount" to 10.50)).output()
val qrImage = generateQRCodeImage(qrString, 300, 300)
// Save or display the qrImage as needed
```

Remember to add the necessary dependencies for the QR code generation library you choose to use.