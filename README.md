# Volume Modifier for Audio Files

## Project Description
This project modifies the volume of an audio file by applying a scaling factor to the audio samples. It reads an input `.wav` file, scales the volume of each audio sample, and writes the result to an output `.wav` file.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Algorithm Explanation](#algorithm-explanation)
- [Code Explanation](#code-explanation)

## Installation
No special installation is required for this project. 

## Usage
To compile and run the project, use the following commands:
```sh
make volume
./volume input.wav output.wav factor
```
You will need to provide an input .wav file, an output .wav file, and a scaling factor for the volume.

## Algorithm Explanation
The algorithm works as follows:

Read the command-line arguments to get the input file, output file, and volume factor.
Open the input and output files.
Copy the header of the .wav file unchanged.
Read each audio sample, scale it by the factor, and write it to the output file.
Close the files.
## Code Explanation
Including Libraries and Defining Constants
``` C
#include <math.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

// Number of bytes in .wav header
const int HEADER_SIZE = 44;
typedef int16_t BYTE;
```
#include <math.h>: Includes the math library for mathematical operations.
#include <stdint.h>: Includes the standard integer types.
#include <stdio.h>: Includes the standard input/output library.
#include <stdlib.h>: Includes functions for memory allocation and process control.
HEADER_SIZE: Defines the size of the .wav file header.
BYTE: Defines a type for 16-bit integers to represent audio samples.
Main Function
``` C
int main(int argc, char *argv[])
{
    if (argc != 4)
    {
        printf("Usage: ./volume input.wav output.wav factor\n");
        return 1;
    }

    FILE *input = fopen(argv[1], "r");
    if (input == NULL)
    {
        printf("Could not open file.\n");
        return 1;
    }

    FILE *output = fopen(argv[2], "w");
    if (output == NULL)
    {
        printf("Could not open file.\n");
        return 1;
    }

    float factor = atof(argv[3]);
    BYTE b;
    BYTE header[HEADER_SIZE];

    fread(header, sizeof(BYTE), HEADER_SIZE, input);
    fwrite(header, sizeof(BYTE), HEADER_SIZE, output);

    while (fread(&b, sizeof(b), 1, input) != 0)
    {
        b *= factor;
        fwrite(&b, sizeof(b), 1, output);
    }

    fclose(input);
    fclose(output);
}
```
Explanation:
Command-Line Argument Check:
``` C
if (argc != 4)
{
    printf("Usage: ./volume input.wav output.wav factor\n");
    return 1;
}
```
Ensures that the user provides exactly three command-line arguments: input file, output file, and volume factor.

Open Input File:

``` C
FILE *input = fopen(argv[1], "r");
if (input == NULL)
{
    printf("Could not open file.\n");
    return 1;
}
```
Opens the input file for reading. If the file cannot be opened, prints an error message and returns 1.

Open Output File:

``` C
FILE *output = fopen(argv[2], "w");
if (output == NULL)
{
    printf("Could not open file.\n");
    return 1;
}
```
Opens the output file for writing. If the file cannot be opened, prints an error message and returns 1.

Convert Volume Factor:

``` C
float factor = atof(argv[3]);
```
Converts the volume factor from a string to a float.

Copy Header:

``` C
BYTE header[HEADER_SIZE];
fread(header, sizeof(BYTE), HEADER_SIZE, input);
fwrite(header, sizeof(BYTE), HEADER_SIZE, output);
```
Reads the header from the input file and writes it to the output file. The header is 44 bytes long and contains metadata about the audio file.

Process Audio Samples:

``` C
BYTE b;
while (fread(&b, sizeof(b), 1, input) != 0)
{
    b *= factor;
    fwrite(&b, sizeof(b), 1, output);
}
```
Reads each audio sample from the input file, scales it by the factor, and writes it to the output file. The fread function reads the samples, and the fwrite function writes the scaled samples.

Close Files:

``` C
fclose(input);
fclose(output);
```
Closes the input and output files.
