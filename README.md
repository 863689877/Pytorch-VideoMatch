# Pytorch VideoMatch

This is an unofficial Pytorch implementation of [`VideoMatch: Matching based Video Object Segmentation`](https://arxiv.org/pdf/1809.01123.pdf)

Currently only single object segmentation (DAVIS 2016) is supported. I also didn't implement online model update (see [ch 3.3](https://arxiv.org/pdf/1809.01123.pdf))
and early stop with regards to validation set IOU.

`main.py` is the main entry point to this project, run `python3 main.py -h` to see all the options.

Both in train and eval mode you can change both encoder type and upsample factor for enlarging encoder features. Encoder weights from `torchvision` are used, so model weights will be
downloaded automatically if you don't have them. Try setting the upsample factor as high as possible (before you get `RuntimeError: CUDA out of memory`) since
this controls resulting segmentation resolution/quality.

Code was written for Python 3.6.8 and Pytorch 1.2.0.

## Train
For training first download [DAVIS dataset](https://davischallenge.org/davis2016/code.html) and store it in project root or use `-d` flag to set DAVIS path.

To stop the training and save the model (needs `-s` option) just press `CTRL+C` (SIGINT).

Currently only batch size of 1 is supported for training because of the implementation nature of cosine similarity in [videomatch.py](videomatch.py).

Example:
```bash
python3 main.py -c 0 -m train -t train -s models/vm_vgg.pth -l 1e-5
```
## Eval
You can also generate segmentation image results for evaluation and video result.

In case you have enabled the result video visualization (flag `-v`) you need to manually close matplotlib figure or
the evaluation won't continue. Either matplotlib has a bug or I'm doing something wrong...

Example:
```bash
python3 main.py -c 0 -m eval -t val -u -o models/vm_vgg.pth -j vm_results -v
```

## Other

You can run `videomatch.py` with reference image path, reference mask path and multiple test image paths to visualize foreground and background probabilities.
Example:
```bash
python3 videomatch.py DAVIS/JPEGImages/480p/bear/00000.jpg DAVIS/Annotations/480p/bear/00000.png DAVIS/JPEGImages/480p/bear/00001.jpg
```

To visualize image augmentation run `preprocess.py`
Example:
```bash
python3 preprocess.py DAVIS/JPEGImages/480p/bear/00000.jpg DAVIS/Annotations/480p/bear/00000.png
```