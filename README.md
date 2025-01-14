# CuReD: Deep Learning Optical Character Recognition for Cuneiform Text Editions and Legacy Materials

Cuneiform documents, the earliest known form of writing, are prolific textual sources of the ancient past. Experts publish editions of these texts in transliteration using specialized typesetting, but most remain inaccessible for computational analysis in traditional printed books or legacy materials. Off-the-shelf OCR systems are insufficient for digitization without adaptation. We present CuReD (Cuneiform Recognition-Documents), a deep learning-based human-in-the-loop OCR pipeline based on the [Kraken OCR model](https://github.com/mittagessen/kraken) for digitizing scanned transliterations of cuneiform texts. CuReD has a character error rate of 9\% on clean data and 11\% on representative scans. We digitized a challenging sample of cuneiform documents and index cards from the University of Pennsylvania Museum, demonstrating the feasibility of our platform for enabling computational analysis and bolstering machine-readable cuneiform text datasets. Our results show that such an OCR platform allows new insights into ancient records and supports the digitization of cuneiform texts across various ancient languages. This repository contains code for generating data and training OCR on Latin transliteration of Cuneiform texts trained on editions of Akkadian and Sumerian texts.

The model can be used through the code and instructions in this repository, or through a GUI at the [Babylonian Engine portal](https://ben-digpasts.com/cured).

## Requirements

* [Kraken](https://github.com/mittagessen/kraken) installed as follows:

Clone the Kraken Github repo and install its Conda environment from within the repo with:

`conda env create -f environment.yml`

Activating the kraken environment (`conda activate kraken`) gives access to the `kraken` and `ketos` command-line tools.

**Note:** If you want to fine-tune the model, you may want to use Kraken with CUDA (GPU acceleration). In this case you should instead install it with:

`conda env create -f environment_cuda.yml`

Note: This also creates an environment with name `kraken`. If you want both environments, edit the `.yml` file to change the environment name (e.g. to `kraken-cuda`).

## Usage Instructions

Activate the Kraken Conda environment and then run:

`kraken -i "IMAGE_FILENAME" OUTPUT_FILENAME binarize segment ocr -m MODEL_FILENAME`

Using the following values:
* IMAGE_FILENAME: Use the filename of the image you want to perform OCR on.
* OUTPUT_FILENAME: Filename of text file to write output to
* MODEL_FILENAME: Should point to the model file you want to use in `models/`

**Note:** In the output text, we use a single left curly brace { to indicate a superscript, which is significant in the common transliteration of Akkadian. We recommend to post-process by adding } after every character which is preceeded by {. For example, "{dAMAR.UTU" should be converted to "{d}AMAR.UTU".

## Fine-Tuning Model

For instructions on fine-tuning the existing model on new data, see `Finetuning.md`.

## Repository Structure

### Models

The `models/` folder contains fine-tuned OCR models. `model.mlmodel` is the original model trained on [SAA](https://oracc.museum.upenn.edu/saao/) and tested on [RINAP](https://oracc.museum.upenn.edu/rinap/) texts. `dillard.mlmodel` was fine-tuned on typewriter text in Dillard (1975)[^1] and might perform better in that use case. `latest.mlmodel` was further fine-tuned on the card catalog of the [lexicography index card collection of Pennsylvania Sumerian Dictionary](https://doi.org/10.5282/ubm/data.417) by Å. W. Sjöberg.

### Tests

In `tests/`, we have provided a sample paragraph (`test.png`, from Dietrich 2003) and OCR results of the default model in `output.txt`. You may run OCR on it and verify that you get the same output.

### Data

Under `data/`, we share all texts from Dillard (1975)[^1] OCR'ed using CuReD and manually corrected. The texts are shared in transliteration and in Unicode cuneiform. We also share a sample of the cards already digitized from the [lexicography index card collection of Pennsylvania Sumerian Dictionary](https://doi.org/10.5282/ubm/data.417) using CuReD and manually corrected by our team.

[^1]: Raymond B. Dillard (1975) Neo-babylonian Texts from the John Frederick Lewis Collection of the Free Library of Philadelphia, PhD dissertation, available at <https://repository.upenn.edu/dropsietheses/141/>
