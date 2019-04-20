# image_compression

![Demo](https://github.com/mukeshkdangi/image_compression/blob/master/Webp.net-gifmaker.gif)

## Motivation
- [x] This assignment will helped us gain an understanding of issues that relate to image compression, by comparing and contrasting the frequency space representations using the Discrete Cosine Transform and the Discrete Wavelet Transform. We  read an RGB file and convert the file to an 8x8 block based DCT representation (as used in the JPEG implementation) and a DWT representation (as used in the JPEG2000 implementation). 
- [x] Depending on the second parameter n we decode both the representations using only n coefficients and display them side to side to compare your results. Remember all input files will have the same format as explained to the class website. They will be of size 512x512 (intentionally square and a power of 2 to facilitate easy encoding and decoding). Your algorithm, whether encoding or decoding, should work on each channel independently.

## Uses : 
```
javac *.java
```
DCT/DWT
```
java ImageCompression Lenna.rgb  26214
java ImageCompression Lenna.rgb   13107
```
Question 6 [Progressive Analysis of DC]
```
java ImageCompression Lenna.rgb   -1 
```

## Inputs and description 
- [x] Input to my program will be 2 parameters where:
- [x]	The first parameter is the name of the input image file. (file format description provided and is similar to the first assignment format of RGB)
- [x]	The second parameter is an integral number that defines the number of coefficients to use for decoding. The interpretation of this parameter of decoding is different for both the DCT and DWT cases so as to use the same number of coefficients. Please see the implementation section for an explanation
 
- [x] Typical invocations to your to your program would look like

- [x] MyExe Image.rgb 262144
Here you are making use of all the coefficients to decode because the total number of coefficients for each channel are going to be 512*512= 262144. Hence the output for each DCT and DWT should be exactly the same as the image with no loss. 

- [x] MyExe Image.rgb 131072
Here you are making use of 131072 (half of the total number) of coefficients for decoding. While the number of coefficients are the same for both DCT and DWT decoding, the exact coefficient indexes are different. Refer to the implementation section for this.

- [x] MyExe Image.rgb 16384
Here you are making use of 16384 (1/16th of the total number) of coefficients for decoding. While the number of coefficients are the same for both DCT and DWT decoding, the exact coefficient indexes are different. Refer to the implementation section for this.

- [x] Implementation 
Your implementation should read the input file and convert (for each channel separately) a DCT representation and a DWT representation.

- [x] Encoding:
For the DCT conversion, break up the image into 8x8 contiguous blocks of 64 pixels each and then perform a DCT for each block for each channel. For the DWT conversion, convert each row (for each channel) into low pass and high pass coefficients followed by the same for each column applied to the output of the row processing. Recurse through the process as explained in class through rows first then the columns next at each recursive iteration, each time operating on the low pass section. 

- [x] Decoding:
Based on the input parameter of the number of coefficients to use, you need to appropriately decode by zeroing out the unrequested coefficients (just setting the coefficients to zero) and then perform an IDCT or an IDWT. The exact coefficients to zero out are different for both the DCT and DWT cases and explained next.

- [x] For a DCT, you want to select the first m coefficients in a zig zag order for each 8x8 block such that m = round(n/4096) where n is the number of coefficients given as input. 4096 is the number of 8x8 blocks in a 512x512 image. Thus, m represents the first few coefficients to use for each 8x8 block during decoding. 
- [x] So for the second test run above (n=131072), you will use m = round(131072/4096) = 32. Each block will be decoded using the first 32 coefficients in zigzag order. The remaining coefficients in each 8x8 block can be set to zero prior to decoding. For the third test run above (n=16384), you will use m = round (16384/4096) = 4. 

- [x] For a DWT, you want to select the first n coefficients in a zig zag order for the image where n is the number of coefficients given as input. DWT encodes the entire image so, here you will use the first n coefficients in zig zag order. The remaining coefficients will be set to zero prior to decoding. The zig zag ordering naturally prioritizes the low frequency coefficients compared to the higher frequency coefficients. Remember to follow the reverse sequence when decoding, that is first column and second row.


- [x] Progressive Analysis of DCT vs DWT (40 points)
Here you will create an animation which will take incremental steps of decoding in order to study the output quality of your DCT vs DWT implementation. For this invocation we will put n= -1, because the incremental number of coefficients are predefined as shown below.

- [x] MyExe Image.rgb -1
This suggests the program has predefined incremental settings on the number of coefficients to use and is going to loop through them incrementally. 

- [x] We have 4096 8x8 blocks in the image. So let’s start by using one coefficient of each block and then incrementing it on each iteration. Total number of progressive iterations is 64

> For the DCT decoding, you will use 
•	first iteration - the DC coefficient for each block (total 4096 coefficients)
•	second iteration – the DC, AC1 coefficient of each block (total 8192 coefficients)
•	third iteration – the DC, AC1, AC2 coefficient of each block (total 12288  coefficients)
…
…
•	sixty forth iteration – the DC, AC1, AC2 …. AC63 coefficient of each block (total 512*512= 262144 coefficients)


> For the DWT decoding, you will use 
•	first iteration - the first 4096 coefficients in zigzag order.
•	second iteration – the first 8192 coefficients in zigzag order.
•	third iteration – the first 12288  coefficients in zigzag order
….
….
•	sixty forth iteration – all the total 512*512= 262144 coefficients.

