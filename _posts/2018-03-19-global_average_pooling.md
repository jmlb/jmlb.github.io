---
layout: post
title: Scraping Google Image to build a dataset
tags: dataset
category: ml
summary: ""
img_post: 20180308-google_image.png
github-link: na
---



<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


In this post, we show how to create a image dataset for Deep Learning Projects using Google Images. The code is extensively based on this blog post: [pyimagesearch](https://www.pyimagesearch.com/2017/12/04/how-to-create-a-deep-learning-dataset-using-google-images/). Thank you @Adrian Rosebrock for the great article.

I modified the code related to file saving so that we can check if an image has already been downloaded: i.e is it a duplicate. If it is a duplicate, move to the next url, otherwise, add the hash value of the image in a list and save the image on the local drive.



### The first step is to create a text files with links to all images: urls.txt

* run a query on Google Image - for example, I searched for images of *racoons*
* scroll down until you go thru all the files you want
* Open Javascript console: View => Developer => JavaScript Console
* Select Tab console
* Pull down jquery into the JavaScript console

```
var script = document.createElement('script');
script.src = "https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(script);
```

* Grab the URLs

```
var urls = $('.rg_di .rg_meta').map(function() { return JSON.parse($(this).text()).ou; });
```

* write the URLs to file (one per line)

```
var textToSave = urls.toArray().join('\n');
var hiddenElement = document.createElement('a');
hiddenElement.href = 'data:attachment/text,' + encodeURI(textToSave);
hiddenElement.target = '_blank';
hiddenElement.download = 'urls.txt';
hiddenElement.click();
```
That's what the text file contains:

```
http://cdn.natgeotv.com.au/subjects/headers/Animal-Racoon.jpg?v=28&azure=false&scale=both&width=1920&height=960&mode=crop
https://racoonprodbuilds.azureedge.net/images/29122016CrazyRacoon.jpg
https://static.independent.co.uk/s3fs-public/styles/article_small/public/thumbnails/image/2014/08/27/19/Raccoon-Getty.jpg
https://i.ytimg.com/vi/Gryv_Z7MSl0/maxresdefault.jpg
https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Racoon_crossing_road.JPG/220px-Racoon_crossing_road.JPG
https://images.fineartamerica.com/images-medium-large-5/curious-racoon-sabrina-l-ryan.jpg
https://thumbs.dreamstime.com/b/racoon-926302.jpg
....
....
```

### The last step is to download the images from the links in the text file

* Let's first import the libraries that we need:

```
import requests
import cv2
import os
import matplotlib.pyplot as plt
import hashlib
from PIL import Image
import io
import numpy as np
```

* We then provide the name of the text file, and the folder where to save the images:

```
urls = "urls.txt"
output = "images"
```

* We will be saving the images in the folder `images/`. Only unique images are saved, duplicates are skipped. 

```
# open the text file that contains the urls
rows = open(urls).read().strip().split("\n")
#Keep track of the number of images downloaded
n_image = 0
ls_image_hash = list()
step_show_img = 50

def showMe(img_arr):
    '''
    Visualize image
    '''
    plt.imshow(img_arr)
    plt.show()

    
for url in rows:
    #Loop thru all the urls
    try:
    #try to download the image
        r = requests.get(url, timeout=60)
        # save the image locally
        fname = os.path.sep.join([output, "{}.jpg".format(str(n_image).zfill(8))])
        img_pil = Image.open(io.BytesIO(r.content))
        img_arr = np.array(img_pil)
        hash_object = hashlib.md5(img_arr)
        img_hash = hash_object.hexdigest()
        if img_hash in ls_image_hash:
            #This image is a duplicate -- skip
            print("Image is a duplicate - Not saved")
        else:
            #add to list
            ls_image_hash.append(img_hash)
            f = open(fname, "wb")
            f.close()
            n_image += 1
            if n_image % step_show_img == 0:
                showMe(img_pil)
            # update the counter
        print("[INFO] downloaded: {}".format(fname))

 
    except:
        print("[INFO] error downloading {}".format(url))

```