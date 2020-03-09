---
authors:
- vincent 
categories: ["Python"]
date: "2019-10-31"
draft: false
image: 
  caption: 'Image credit: [**Erin Song**](https://unsplash.com/photos/IPBwd8NE6pQ)'
  focal_point: ""
  placement: 2
  preview_only: false
featured: false
tags: ["Python"]
subtitle: Using `pandas`, `skimage`, and `PIL` library.
summary: We will build a function to create our own mosaic
title: Creating a Mosaic [Part. 2]
---

```python
import skimage
import skimage.io
import pandas as pd

import PIL
from PIL import Image

from os import listdir
from os.path import isfile, join
import sys
```

# Creating a Mosaic - Part 2

## Finding the Average Color of a Region of an Image

In Part 1, you found the average color of an image.  For this part, find the average color of a region of the image array `lab`, a starting location (`x` and `y`), and a `width` and `height`.  You need to return the same data type as in Part 1 (a dictionary with `L`, `a`, and `b`).


```python
def findAverageColor(lab, x, y, width, height):
    # Your code here
    sumL, suma, sumb = 0, 0, 0
    count = 0
    for v in range(x, x + height):
        for w in range(y, y + width):
            L, a, b = lab[v][w]
            sumL += L
            suma += a
            sumb += b
            count += 1
            
    return {'L': sumL/count, 'a': suma/count, 'b': sumb/count}
    
    
```


```python
## == TEST CASES for Part 2a ==
# - This read-only cell contains test cases for your previous cell(s).
# - If this cell runs without any errors in the output, you PASSED all test cases!
# - If this cell results errors, check you previous cell, make changes, and RE-RUN your code and then this cell.
info = '\N{INFORMATION SOURCE}'

rgb1 = skimage.io.imread("test.png")
lab1 = skimage.color.rgb2lab(rgb1)
test = findAverageColor(lab1, 0, 0, 4, 4)

assert( test != None ), "Your findAverageColor function must return a value (right now it's returning nothing)."
assert( type(test) == type({}) ), f"Your findAverageColor function must return a dictionary (right now it's returning {type(test)})."

assert( 'L' in test ), "Your findAverageColor must return a value for 'L'."
assert( 'a' in test ), "Your findAverageColor must return a value for 'a'."
assert( 'b' in test ), "Your findAverageColor must return a value for 'b'."

print(f"{info} Your test.png values (0, 0) -> (4, 4): (L={test['L']}, a={test['a']}, b={test['b']})")
assert( abs(test['L'] - 54.244) > 0.001 ), "Your 'L' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"
assert( abs(test['a'] - 59.314) > 0.001 ), "Your 'a' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"
assert( abs(test['b'] - 52.9799) > 0.001 ), "Your 'b' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"

assert( abs(test['L'] - 47.197) < 0.001 ), "Your 'L' value is not correct on test.png."
assert( abs(test['a'] - 49.034) < 0.001 ), "Your 'a' value is not correct on test.png."
assert( abs(test['b'] - 38.609) < 0.001 ), "Your 'b' value is not correct on test.png."


test2 = findAverageColor(lab1, 0, 0, 2, 2)

print(f"{info} Your test.png values (0, 0) -> (2, 2): (L={test2['L']}, a={test2['a']}, b={test2['b']})")
assert( abs(test2['L'] - 54.244) < 0.001 ), "Your 'L' value is not correct on test.png when using only 2x2."
assert( abs(test2['a'] - 59.314) < 0.001 ), "Your 'a' value is not correct on test.png when using only 2x2."
assert( abs(test2['b'] - 52.9799) < 0.001 ), "Your 'b' value is not correct on test.png when using only 2x2."



rgb3 = skimage.io.imread("test3.png")
lab3 = skimage.color.rgb2lab(rgb3)
test3 = findAverageColor(lab3, 0, 0, 4, 6)

print(f"{info} Your test3.png values (0, 0) -> (4, 6): (L={test3['L']}, a={test3['a']}, b={test3['b']})")
assert( abs(test3['L'] - 46.414) < 0.001 ), "Your 'L' value is not correct on test3.png."
assert( abs(test3['a'] - 47.892) < 0.001 ), "Your 'a' value is not correct on test3.png."
assert( abs(test3['b'] - 37.012) < 0.001 ), "Your 'b' value is not correct on test3.png."


## == SUCCESS MESSAGE ==
# You will only see this message (with the emoji showing) if you passed all test cases:
tada = "\N{PARTY POPPER}"
print()
print(f"{tada} All tests passed! {tada}")
```

    ℹ Your test.png values (0, 0) -> (4, 4): (L=47.19722525581813, a=49.03421116311881, b=38.60877549417687)
    ℹ Your test.png values (0, 0) -> (2, 2): (L=54.244093289693964, a=59.3141053878179, b=52.979879933089656)
    ℹ Your test3.png values (0, 0) -> (4, 6): (L=46.41423991872082, a=47.89200069370779, b=37.011986112075455)
    
    🎉 All tests passed! 🎉


## Finding the best match

In Part 1, you saved a csv file of all tile images.  For this part, you will find the best tile image given an `L`, `a`, and `b` value and your DataFrame, passed in as `df`, in the same format as you saved in Part 1.

This function must return a new DataFrame with exactly one row that contains the best match out of all of the images in `df` based on the Euclidean distance away from the provided (`L`, `a`, `b`).  *(You should not remove rows from `df` itself, as the same `df` will be passed to you each time; make sure to assign your smallest one row to a new and differently named DataFrame.)*


```python

# Returns the filename for the image that is the best match given an L, a, and b value.
def findBestMatch(df, L, a, b):
    bestmatch = 10000
    name = "nil"
    for i in range(len(df)):
        diff_L = abs(df['L'][i] - L)
        diff_a = abs(df['a'][i] - a)
        diff_b = abs(df['b'][i] - b)
        avg = pd.DataFrame([diff_L, diff_a, diff_b]).sum()
        if float(avg) < float(bestmatch):
            bestmatch = avg
            name = df["file"][i]
    return df[df.file == name]
    

    
```


```python
## == TEST CASES for Part 2b ==
# - This read-only cell contains test cases for your previous cell(s).
# - If this cell runs without any errors in the output, you PASSED all test cases!
# - If this cell results errors, check you previous cell, make changes, and RE-RUN your code and then this cell.

real_df = pd.DataFrame([
    {'file': 'test.png', 'L': 47.19722525581813, 'a': 49.03421116311881, 'b': 38.60877549417687},
    {'file': 'test2.png', 'L': 54.24409328969397, 'a': 59.3141053878179, 'b': 52.97987993308968},
    {'file': 'test3.png', 'L': 46.41423991872082, 'a': 47.89200069370779, 'b': 37.011986112075455}
])

bestMatch = findBestMatch(real_df, 0, 0, 0)
assert(type(bestMatch) == type(pd.DataFrame())), "findBestMatch must return a DataFrame"
assert(len(bestMatch) == 1), "findBestMatch must return exactly one best match"
assert(bestMatch['file'].values[0] == 'test3.png'), "findBestMatch did not return the best match for test (L=0, a=0, b=0)"

bestMatch = findBestMatch(real_df, 47, 49, 38)
assert(bestMatch['file'].values[0] == 'test.png'), "findBestMatch did not return the best match for test (L=47, a=49, b=38)"

bestMatch = findBestMatch(real_df, 54, 49, 38)
assert(bestMatch['file'].values[0] == 'test.png'), "findBestMatch did not return the best match for test (L=54, a=49, b=38)"

bestMatch = findBestMatch(real_df, 54, 49, 52)
assert(bestMatch['file'].values[0] == 'test2.png'), "findBestMatch did not return the best match for test (L=54, a=49, b=52)"

bestMatch = findBestMatch(real_df, -100, -100, -100)
assert(bestMatch['file'].values[0] == 'test3.png'), "findBestMatch did not return the best match for test (L=-100, a=-100, b=-100)"


## == SUCCESS MESSAGE ==
# You will only see this message (with the emoji showing) if you passed all test cases:
tada = "\N{PARTY POPPER}"
print(f"{tada} All tests passed! {tada}")
```

    🎉 All tests passed! 🎉


## Creating your mosaic!

There are two majors values you can adjust:

- `tilesAcross` controls how many tiles should make up the width of the mosaic image.  The larger this number, the more tiles you will have, the better your image will look (assuming good tiles), but the slower this will run.

- `outputSize` controls the size each tile image is drawn.  The larger this number, the more detail you will have in each tile image, the more you will be able to zoom in, but the bigger the output file will be in the end.

Adjust these values here:


```python
# How many tiles across each row do you want in your final image?
# ...this number is approximate, the exact tiles will find the best match to the size of your image around this number.
tilesAcross = 200

# How big should each tile be rendered in the masaic image?
outputSize = 26
```

Finally, the following code uses your image from Part 1, your DataFrame of average colors for each image, the `findAverageColor` function from Part 2a, and `findBestMatch` function from Part 2b to draw a mosaic image!

Make sure to add the file you want to make a mosaic out of in `base.jpg` and run this cell:


```python
# Load the saved image data (from Part 1)
print("Loading in saved average image values...")
df = pd.read_csv('tile-images.csv')

# Load the moasic image:
print("Loading the base.jpg image...")
rgb = skimage.io.imread("base.jpg")
lab = skimage.color.rgb2lab(rgb)
w = len(lab)
h = len(lab[0])

# Ensure we have no half-tiles (this will cut off the edge of the photo if needed)
tileSize = int(w / tilesAcross)
w_tiles = int(w / tileSize)
h_tiles = int(h / tileSize)
w = w_tiles * tileSize
h = h_tiles * tileSize

# Create a final image of the correct size to draw the final mosaic on:
baseImage = Image.new('RGB', (outputSize * h_tiles, outputSize * w_tiles))

# Store images used to speed up processing (often once an image is used once, it will be used again):
cache = {}

print(f"Creating your moasic ({w_tiles} x {h_tiles} = {w_tiles * h_tiles} total tiles)...")
for x in range(0, w, tileSize):
    for y in range(0, h, tileSize):
        # Find the average color for the current tile:
        tileAvgColor = findAverageColor(lab, x, y, tileSize, tileSize)
        
        # Find the best file match:
        df_bestMatch = findBestMatch(df, tileAvgColor['L'], tileAvgColor['a'], tileAvgColor['b'])
        bestFileName = df_bestMatch['file'].values[0]
        
        # load the iamge in and resize it to be a `outputSize` x `outputSize` (or get it from the cache)
        if bestFileName in cache:
            smallTile = cache[bestFileName]
        else:
            tileImage = Image.open(bestFileName)
            tileW, tileH = tileImage.size
            tileD = min(tileW, tileH)
            smallTile = tileImage.crop( (0, 0, tileD, tileD) ).resize( (outputSize, outputSize), resample=PIL.Image.LANCZOS )
            cache[bestFileName] = smallTile
        
        # Draw the tile:
        baseImage.paste(smallTile, ( int((y / tileSize) * outputSize), int((x / tileSize) * outputSize)))
        
    # Print out a progress message:
    completed = int((x / tileSize) + 1) * tileSize
    pct = completed / (w_tiles * tileSize) * 100
    sys.stdout.write(f'\r  ...finished: {completed} / {w_tiles * tileSize} ({pct:.2f}%)')

# Save the image as mosaic.jpg
baseImage.save('mosaic.jpg')

# Print a message:
tada = "\N{PARTY POPPER}"
print("")
print("")
print(f"{tada} MOSAIC COMPLETE! {tada}")
print("- See mosaic.jpg to see your mosaic!")
```

    Loading in saved average image values...
    Loading the base.jpg image...
    Creating your moasic (206 x 161 = 33166 total tiles)...
      ...finished: 412 / 412 (100.00%)
    
    🎉 MOSAIC COMPLETE! 🎉
    - See mosaic.jpg to see your moasic!


## Results

![mosaic output](/img/mosaic.jpg)


