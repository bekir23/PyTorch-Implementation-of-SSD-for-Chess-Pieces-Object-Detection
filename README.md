This is a **[PyTorch](https://pytorch.org) Implementation of SSD: Single Shot MultiBox Detector for Chess Pieces Object Detection**.

I'm using `PyTorch 1.9.0` in `Python 3.7.10`.

#### Description

This data contains images with twelve different types of objects.

```python
{'white-king', 'white-queen', 'white-bishop', 'white-knight', 'white-rook', 'white-pawn', 'black-king', 'black-queen', 'black-bishop', 'black-knight', 'black-rook', 'black-pawn'}
```


#### Download

Specfically, you will need to download the following VOC type dataset –

- [Chess Pieces Dataset](https://public.roboflow.com/object-detection/chess-full/23) (80MB)


#### Parse raw data

See `create_data_lists()` in [`utils.py`]

This parses the data downloaded and saves the following files –

- A **JSON file for each split with a list of the absolute filepaths of `I` images**, where `I` is the total number of images in the split.

- A **JSON file for each split with a list of `I` dictionaries containing ground truth objects, i.e. bounding boxes in absolute boundary coordinates, their encoded labels, and perceived detection difficulties**. The `i`th dictionary in this list will contain the objects present in the `i`th image in the previous JSON file.

- A **JSON file which contains the `label_map`**, the label-to-index dictionary with which the labels are encoded in the previous JSON file. 



#### Data Transforms

See `transform()` in [`utils.py`]

This function applies the following transformations to the images and the objects in them –

- Randomly **adjust brightness, contrast, saturation, and hue**, each with a 50% chance and in random order.

- With a 50% chance, **perform a _zoom out_ operation** on the image. This helps with learning to detect small objects.

- Randomly crop image, i.e. **perform a _zoom in_ operation.** This helps with learning to detect large or partial objects. Some objects may even be cut out entirely. Crop dimensions are to be between `0.3` and `1` times the original dimensions. The aspect ratio is to be between `0.5` and `2`. Each crop is made such that there is at least one bounding box remaining that has a Jaccard overlap of either `0`, `0.1`, `0.3`, `0.5`, `0.7`, or `0.9`, randomly chosen, with the cropped image. In addition, any bounding boxes remaining whose centers are no longer in the image as a result of the crop are discarded. There is also a chance that the image is not cropped at all.

- With a 50% chance, **horizontally flip** the image.

- **Resize** the image to `300, 300` pixels. This is a requirement of the SSD300.

- Convert all boxes from **absolute to fractional boundary coordinates.** At all stages in our model, all boundary and center-size coordinates will be in their fractional forms.

- **Normalize** the image with the mean and standard deviation of the ImageNet data that was used to pretrain our VGG base.




# Training

Before you begin, make sure to save the required data files for training and evaluation. To do this, run the contents of [`create_data_lists.py`].


To **train your model from scratch**, run this file –

`python train.py`

To **resume training at a checkpoint**, point to the corresponding file with the `checkpoint` parameter at the beginning of the code.

If you do not want to train from scratch. You can download this pretrained model [here](https://drive.google.com/open?id=1bvJfF6r_zYl2xZEpYXxgb7jLQHFZ01Qe). Then you can point to the corresponding file with the `checkpoint` parameter at the beginning of the code.


# Inference

See [`detect.py`]

Point to the model you want to use for inference with the `checkpoint` parameter at the beginning of the code.

Then, you can use the `detect()` function to identify and visualize objects in an RGB image.

You can give test folder path and output folder path as shown below. Thus, you can have all test image results in output folder.

`python detect.py "test_folder_path" "output_folder_path"`





