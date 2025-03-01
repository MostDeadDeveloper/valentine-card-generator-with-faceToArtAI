# valentine-card-generator-with-faceToArtAI  (Modified christmAIs for valentines)

[![Documentation Status](https://readthedocs.org/projects/christmais-2018/badge/?version=latest)](https://christmais-2018.readthedocs.io/en/latest/?badge=latest)
[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
![python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)

**valentine-card-generator-with-faceToArtAI ** is a special gift to your loved one, a valentine card generator with a personalized Abstract Art using your crush face and a random abstract art for the valentines!

This app does the following using your crush's picture and an abstract art as input:
- accepts a face image that has been converted to line art by another tool
- applying [neural style transfer](https://arxiv.org/abs/1508.06576) to the resulting image
- put the art and your personalized message in a small HTML template to gift it to your crush on valentines!

The results of some of the art looks like this:

## Samplle: Initial Image + Abstract Art = AI Generated Abstract ART

<span>
 <img src="https://drive.google.com/uc?export=view&id=1uTy9dsCzbnaGZgFnIeDg7DtV-5eX7H-l" width=250 height=200/>
 <img src="https://drive.google.com/uc?export=view&id=1mMzhAgjVIX7PtXHzbvfohq-zfayU8_2S" width=250 height=200/>&nbsp;&nbsp;&nbsp;
 <img src="https://drive.google.com/uc?export=view&id=1bTdpnBWla5CaK10kEAZyba48TU6QCv8p" width=250 height=200/>
 <br/><br/>
</span>

<span>
 <img src="https://drive.google.com/uc?export=view&id=1PRKE3B042_U44TXiuHh7Enpujbrugeys" width=250 height=200/>
 <img src="https://drive.google.com/uc?export=view&id=1Ih3HEpsH7sRx9oZlJbNvkay4_ViTMQEC" width=250 height=200/>&nbsp;&nbsp;&nbsp;
 <img src="https://drive.google.com/uc?export=view&id=1N0-oa8kydabE1ldJxf02X7fnhlii5hzZ" width=250 height=200/>   
 <br/><br/>
</span>

<span>
 <img src="https://drive.google.com/uc?export=view&id=1rQsreBGDOKE1_5AAjndu5sxQCBejbq_M" width=250 height=200/>
 <img src="https://drive.google.com/uc?export=view&id=1S8y7pMzluZteycbewVZQt5XKRHgToazm" width=250 height=200/>&nbsp;&nbsp;&nbsp;
 <img src="https://drive.google.com/uc?export=view&id=1qf5QzR7XkndGmtzMtGOBI4RZ8_YC-ik1" width=250 height=200/>   
 <br/><br/>
</span>

## Sample: The Final Output! The Actual Valentine Card!
<span>
  <img src="https://drive.google.com/uc?export=view&id=1FD-ismyJR8CbWqMeFvs7Bn7hbIDZ8Dui" width=750 height=300/>
</span>

## Setup and Installation

Please see `requirements.txt` and `requirements-dev.txt` for all Python-related
dependencies. Notable dependencies include:

- numpy==1.14.6
- scikit_learn==0.20.0
- Pillow==5.3.0
- matplotlib==2.1.0
- tensorflow=1.13.1
- gensim=3.6.0
- magenta=0.4.0

The build steps (what we're using to do automated builds in the cloud) can be
seen in the
[Dockerfile](https://github.com/thinkingmachines/christmAIs/blob/master/Dockerfile).
For local development, it is **recommended to setup a virtual environment**. To
do that, simply run the following commands:

```shell
git clone git@github.com:thinkingmachines/christmAIs.git
cd christmAIs
make venv
```

### Automated Install

We created an automated install script to perform a one-click setup in your
workspace. To run the script, execute the following command:

```shell
source venv/bin/activate  # Highly recommended
./install-christmais.sh
```

This will first install `magenta` and its dependencies, download file
dependencies (`categories.txt`, `model.ckpt`, and `chromedriver`), then clone
and install this package.

### Manual Install

For manual installation, please follow the instructions below:

#### Installing magenta

The style transfer capabilities are dependent on the
[magenta](https://github.com/tensorflow/magenta) package. As of now, magenta is
only supported in Linux and Mac OS. To install magenta, you can perform the
[automated install](https://github.com/tensorflow/magenta#automated-install)
or do the following steps:

```shell
# Install OS dependencies
apt-get update && \
apt-get install -y build-essential libasound2-dev libjack-dev

# Install magenta
venv/bin/pip install magenta

```

#### Installing everything else

You can then install the remaining dependencies in `requirements.txt`. Assuming
that you have create a virtual environment via `make venv`, we recommend that
you simply run the following command:

```shell
make build # or `make dev`
```

This will also download (via `wget`) the following files:
- **categories.txt** (683 B): contains the list of Quick, Draw! categories to compare a string upon (will be saved at `./categories/categories.txt`).
- **arbitrary_style_transfer.tar.gz** (606.20 MB): contains the model checkpoint for style
    transfer (will be saved at `./ckpt/model.ckpt`).
- **chromedriver** (5.09 MB): contains the web driver for accessing the HTML output for
    Sketch-RNN (will be saved at `./webdriver/chromedriver`).

#### Generating the documentation

Ensure that you have all dev dependencies installed:

```shell
git clone git@github.com:thinkingmachines/christmAIs.git
make venv
make dev
```

Then to build the actual docs

```shell
cd christmAIs/docs/
make html
```

This will generate an `index.html` file that you can view in your browser.

## Usage

Select First an **image of your crush** and one **abstract art of your choice** will do.

Convert the image of your crush to a sketch, there are  a lot of online tools for this. I personally used these two:
 - [AI Draw](https://ai-draw.tokyo/en/)
 - [VanceAI - VancePortrait](https://vanceai.com/photo-to-sketch/)

Install the following repository here and follow the installation process above, feed the two images to the script below.

a script has been provided, `christmais_time.py` to easily generate your stylized images.
In order to use it, simply run the following command:

```shell
python -m christmais.tasks.christmais_time     \
    --input=<Input string to draw from>        \
    --style=<Path to style image>              \
    --output=<Unique name of output file>      \
    --model-path=<Path to model.ckpt>          \
    --categories-path=<Path to categories.txt> \
    --webdriver-path=<Path to webdriver>
```

If you followed the setup instructions above, then the default values for the
paths should suffice, you only need to supply `--input`, `--style`, and
`--output`.

The Following are the written values:
- **input=** - just input the word "basket" here
- **style** - the location of your abstract art, put it in the ROOT location of the repository. Any format will do.
- **output** - the face of your crush that has been pre-processed to a sketch. Ensure that the name of the file is "test.png" ans is IN PNG Format. Currently PNG is only the supported format for the images of your crush. Add this image in the generated_png folder of your project, that should have been generated during installation.

Example Command Script
```shell
python -m christmais.tasks.christmais_time \
    --input="basket" \
    --style=style.png \
    --output=test
```

This will then generate the output image in `./artifacts/`:

![alt text](https://drive.google.com/uc?export=view&id=1bTdpnBWla5CaK10kEAZyba48TU6QCv8p)

After the script has finshed compiling, Head to the ValentinesTemplate Folder and click the meme.html file.

Make a small message for your crush and see the results of your work! It should look something like this. 

<span>
  <img src="https://drive.google.com/uc?export=view&id=1FD-ismyJR8CbWqMeFvs7Bn7hbIDZ8Dui" width=750 height=300/>
</span>

### Happy Valentines and Good Luck With Your Crush!


## References
-  Thinking Machines Data Science, christmAIs: text-to-abstract art generation for the holidays! (https://github.com/thinkingmachines/christmAIs)
- Annica Chiu, Base Template for Valentine Card (https://github.com/chiuannica/valentinesdaycard)
- Pennington, Jeffrey, Socher, Richard, et al. (2014). “Glove: Global Vectors for Word Representation”. In: *Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP)*, pp. 1532-1543.
- Ha, David and Eck, Douglas (2017). “A Neural Representation of Sketch Drawings”. In: *arXiv.:1704.03477*.
- Ghiasi, Golnaz et al. (2017). “Exploring the structure of real-time, arbitrary neural artistic stylization network”. In: *arxiv:1705.06830*.
- Magenta demonstration (`sketch-rnn.js`):https://github.com/hardmaru/magenta-demos/tree/master/sketch-rnn-js
