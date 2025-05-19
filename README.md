# CodeReader

## Overview

CodeReader is a native iOS framework designed for barcode detection in images.  
Simply provide an image, and the framework will accurately detect and return the locations of all present barcodes.

## Installation

To add a package dependency to your Xcode project, select File > Add Packages and enter its repository URL. Or once you have your own Swift package, adding CodeReader as a dependency is as easy as adding it to the dependencies value of your Package.swift.

```
dependencies: [
    .package(url: "https://github.com/eagle-lab-apps/code-reader-ios", .upToNextMajor(from: "1.0.0"))
]
```

## Usage

### Importing the Framework
To use **CodeReader**, first import it into your project:
```
import CodeReader
```

### Detecting Barcodes in an Image
To scan an image and detect all barcodes, use the following method:

```
let reader = BarcodeReader()
let image = UIImage(named: "barcode_sample")

let barcodes = reader.scan(image: image) { barcodes in
    barcodes?.forEach {
        print("Barcode found at: \($0.boundingBox)")
    }
}
```
If you want to detect barcodes and map their coordinates relative to an `UIImageView`, you can pass the `UIImageView` instance as the optional parameter.  
This converts the detected bounding boxes to match the coordinate system of the provided image view:

```
let imageView = UIImageView(image: image)
reader.scan(image: image, in: imageView) { barcodes in
    barcodes?.forEach {
        print("Barcode found at: \($0.boundingBox) in imageView coordinates")
    }
}
```
**Note:** 
- Barcodes are sorted from top to bottom in the detected list
- To use image view coordinates correctly, ensure that the `imageView`'s content mode is set to `.scaleAspectFit`

### Handling the Detection Result
The framework returns an array of detected barcodes, each containing:
- `type`: String representation of the barcode format detected in the image.
- `boundingBox`: The CGRect representing the barcode's location in the image.

Example:
```
for barcode in barcodes {
    print("Barcode Type: \(barcode.type")")
    print("Barcode Position: \(barcode.boundingBox)")
}
```

### Debug Mode

The framework includes a debug mode that, when activated, logs messages in the console to help with debugging and development.  
To enable debug mode, use the `setDebugMode` method and pass `true` as the argument:

```
let reader = BarcodeReader()
reader.setDebugMode(enabled: true)
```

## Suported Barcodes

### 1D(linear) Barcodes:
- Code 39
- Code 39 (Extended)
- Code 93
- Code 128
- EAN-8
- EAN-13
- UPCE (Universal Product Code - E)
- Interleaved 2 of 5
- CodaBar

### 2D Barcodes:
- QR Code
- Aztec
- Data Matrix
- PDF417

