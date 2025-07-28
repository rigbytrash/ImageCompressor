# Image Compression and Conversion Tool

This is a C-based image compression and conversion tool that works with three custom grayscale image file formats: **ebf** (text-based), **ebu** (binary), and **ebc** (compressed binary). The tool provides utilities to convert between formats, compare images, and echo/validate image files.

## Features

- **Format Conversion**: Convert between ebf, ebu, and ebc formats
- **Image Comparison**: Compare two images of the same format for equality
- **File Validation**: Echo/validate image files by reading and rewriting them
- **Compression**: Compress images using 5-bit pixel packing (ebf → ebc)
- **Error Handling**: Comprehensive error checking for file format validation

## Available Programs

The project compiles into 10 executable programs:

### Conversion Tools
- `ebf2ebu` - Convert ebf (text) to ebu (binary) format
- `ebu2ebf` - Convert ebu (binary) to ebf (text) format  
- `ebu2ebc` - Convert ebu (binary) to ebc (compressed) format
- `ebc2ebu` - Convert ebc (compressed) to ebu (binary) format

### Comparison Tools
- `ebfComp` - Compare two ebf files for identical content
- `ebuComp` - Compare two ebu files for identical content
- `ebcComp` - Compare two ebc files for identical content

### Echo/Validation Tools
- `ebfEcho` - Read and rewrite an ebf file (validation)
- `ebuEcho` - Read and rewrite an ebu file (validation)
- `ebcEcho` - Read and rewrite an ebc file (validation)

## Build Instructions

```bash
make          # Build all executables
make clean    # Remove compiled files
```

## Usage

All programs follow a similar usage pattern:
```bash
./program_name input_file output_file
```

Examples:
```bash
./ebf2ebu sample_images/square.ebf square.ebu
./ebfComp sample_images/square.ebf sample_images/dodo.ebf
./ebfEcho sample_images/square.ebf output.ebf
```

## Error Codes and Messages

| Value  | String | Condition |
| ------------- | ------------- | ------------- |
| 1  | ERROR: Bad Argument Count | Program given wrong # of arguments |
| 2 | ERROR: Bad File Name (fname) | Program fails to open file |
| 3 | ERROR: Bad Magic Number (fname) | File has an illegal magic number |
| 4 | ERROR: Bad Dimensions (fname) | File has illegal dimensions |
| 5 | ERROR: Image Malloc Failed | Malloc failed to allocate memory |
| 6 | ERROR: Bad Data (fname) | Reading in data failed |
| 7 | ERROR: Output Failed (fname) | Writing out data failed |
| 100 | ERROR: Miscellaneous (describe) | Any other error which is detected |


## Program Output Messages

| Message | Condition |
| ------------- | ------------- |
| `Usage: executablename file1 file2` | Program run with incorrect arguments |
| `ECHOED` | Echo program successfully validates and rewrites file |
| `IDENTICAL` | Comparison program finds files are identical |
| `DIFFERENT` | Comparison program finds files are different |
| `CONVERTED` | Conversion program successfully converts file |

## File Format Specifications

All three file formats store grayscale images with pixel values ranging from 0-31 (5 bits per pixel).

## ebf files (Text Format)

ebf files store images in human-readable text format:

ebf files store images in human-readable text format:

```
eb              - Magic number (always "eb")
height width    - Dimensions as space-separated integers
p1 p2 p3 ...    - Pixel values (0-31) separated by spaces
```

**Example**: `sample_images/square.ebf` contains an 8x8 square pattern.

## ebu files (Binary Format)

ebu (ebf uncompressed) files store the same data in binary format for more efficient storage:

```
eu              - Magic number (always "eu")
height width    - Dimensions in ASCII text
<binary data>   - Pixel values stored as binary bytes
```

## ebc files (Compressed Binary Format)

ebc (ebf compressed) files use bit-packing compression. Since pixels only need 5 bits (0-31), multiple pixels are packed into single bytes:

```
ec              - Magic number (always "ec")
height width    - Dimensions in ASCII text  
<packed data>   - Compressed pixel data (5 bits per pixel packed into bytes)
```

The compression algorithm packs 8 pixels into every 5 bytes (8 × 5 = 40 bits = 5 bytes), achieving approximately 62.5% of the original ebu file size.

## Sample Images

The `sample_images/` directory contains example ebf files:
- `square.ebf` - Simple 8x8 square pattern
- `dodo.ebf` - 40x40 complex image
- `duck.ebf`, `feep.ebf`, `gradient.ebf`, etc. - Various test images
