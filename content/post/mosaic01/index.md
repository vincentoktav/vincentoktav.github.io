---
authors:
- vincent 
categories: ["Python"]
date: "2019-10-23"
draft: false
featured: false
tags: ["Python"]
image: 
  caption: 'Image credit: [**Erin Song**](https://unsplash.com/photos/IPBwd8NE6pQ)'
  focal_point: ""
  placement: 2
  preview_only: false
subtitle: Using `pandas`, `skimage`, and `PIL` library.
summary: We will build a function to create our own mosaic
title: Creating a Mosaic [Part. 1]
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

# Creating a Mosaic - Part 1

To follow through, you can open [my](https://github.com/vincentoktav/vincentoktav.github.io/tree/sources/content/post/mosaic) github repository.

## Programming!

Create `findAverageImageColor` function:


```python
def findAverageImageColor(fileName):
    # Your code here!
    sumL, suma, sumb = 0, 0, 0
    count = 0
    rgb = skimage.io.imread(fileName)
    lab = skimage.color.rgb2lab(rgb)
    width = len(lab)
    height = len(lab[0])
    for x in range(width):
        for y in range(height):
            L, a, b = lab[x][y]
            sumL += L
            suma += a
            sumb += b
            count += 1
            
    return {'L': sumL/count, 'a': suma/count, 'b': sumb/count}
```

### Test Cases

These test cases help you debug your code to make sure you're finding the correct average values for known images.  After you pass these test cases, then you'll start using your own images! :)


```python
## == TEST CASES for Part 1c ==
# - This read-only cell contains test cases for your previous cell(s).
# - If this cell runs without any errors in the output, you PASSED all test cases!
# - If this cell results errors, check you previous cell, make changes, and RE-RUN your code and then this cell.
info = '\N{INFORMATION SOURCE}'
test = findAverageImageColor('test.png')

assert( test != None ), "Your findAverageImageColor function must return a value (right now it's returning nothing)."
assert( type(test) == type({}) ), f"Your findAverageImageColor function must return a dictionary (right now it's returning {type(test)})."

assert( 'L' in test ), "Your findAverageImageColor must return a value for 'L'."
assert( 'a' in test ), "Your findAverageImageColor must return a value for 'a'."
assert( 'b' in test ), "Your findAverageImageColor must return a value for 'b'."

print(f"{info} Your test.png values: (L={test['L']}, a={test['a']}, b={test['b']})")
assert( abs(test['L'] - 54.244) > 0.001 ), "Your 'L' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"
assert( abs(test['a'] - 59.314) > 0.001 ), "Your 'a' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"
assert( abs(test['b'] - 52.9799) > 0.001 ), "Your 'b' value is the value of only orange pixels.  Are you sure you are visiting every pixel?"

assert( abs(test['L'] - 47.197) < 0.001 ), "Your 'L' value is not correct on test.png."
assert( abs(test['a'] - 49.034) < 0.001 ), "Your 'a' value is not correct on test.png."
assert( abs(test['b'] - 38.609) < 0.001 ), "Your 'b' value is not correct on test.png."


test2 = findAverageImageColor('test2.png')

print(f"{info} Your test2.png values: (L={test2['L']}, a={test2['a']}, b={test2['b']})")
assert( abs(test2['L'] - 54.244) < 0.001 ), "Your 'L' value is not correct on test2.png."
assert( abs(test2['a'] - 59.314) < 0.001 ), "Your 'a' value is not correct on test2.png."
assert( abs(test2['b'] - 52.9799) < 0.001 ), "Your 'b' value is not correct on test2.png."


test3 = findAverageImageColor('test3.png')

print(f"{info} Your test3.png values: (L={test3['L']}, a={test3['a']}, b={test3['b']})")
assert( abs(test3['L'] - 46.414) < 0.001 ), "Your 'L' value is not correct on test3.png."
assert( abs(test3['a'] - 47.892) < 0.001 ), "Your 'a' value is not correct on test3.png."
assert( abs(test3['b'] - 37.012) < 0.001 ), "Your 'b' value is not correct on test3.png."


## == SUCCESS MESSAGE ==
# You will only see this message (with the emoji showing) if you passed all test cases:
tada = "\N{PARTY POPPER}"
print()
print(f"{tada} All tests passed! {tada}")
```

    ℹ Your test.png values: (L=47.19722525581813, a=49.03421116311881, b=38.60877549417687)
    ℹ Your test2.png values: (L=54.24409328969397, a=59.3141053878179, b=52.97987993308968)
    ℹ Your test3.png values: (L=46.41423991872082, a=47.89200069370779, b=37.011986112075455)
    
    🎉 All tests passed! 🎉


## Your Images

Once your function works and passes all the test cases, this code loads all files in the `tile-images` folder and stores the average color value of the image in `tile-images.csv`.

- This code is already complete, but depends on a correct `findAverageImageColor` function!
- Make sure your code is complete before running this function!



```python
# What directory includes the images?
imageDir = "tile-images"

# Finds all files inside of `imageDir`:
files = [imageDir + "/" + f for f in listdir(imageDir + "/") if isfile(join(imageDir + "/", f))]

# Use `data` to store data about every image:
data = []

# Loop through every file:
print(f"Starting to process {len(files)} files...")
for file in files:
    try:
        # Run `findAverageImageColor` and store the result:
        result = findAverageImageColor(file)
        d = { 'file': file, 'L': result['L'], 'a': result['a'], 'b': result['b'] }
        data.append(d)
        pct = len(data) / len(files) * 100
        sys.stdout.write(f"\r  ... {len(data)} / {len(files)} total files processed ({pct:.2f}%)")
    except ValueError as e:
        print(f"!! {file} failed to process (this can happen if the image is black/white and has no color data?)")
        print("...if so, remove the image and run again.")
        print(repr(e))
        break
                
# Save the data as a DataFrame and CSV file:
df = pd.DataFrame(data)
df.to_csv('tile-images.csv')
df

tada = "\N{PARTY POPPER}"
print("")
print("")
print(f"{tada} Image data saved as ""tile-images.csv""")
```

    Starting to process 2113 files...
      ... 2113 / 2113 total files processed (100.00%)
    
    🎉 Image data saved as tile-images.csv


## Continue to Part 2

- Part 2 will use the CSV file you saved, `tile-images.csv`, to build your image mosaic!
