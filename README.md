# Geographically Split YFCC for Domain Generalization (Geo-YFCC)
![Samples from GeoYFCC](header.png "Samples from GeoYFCC")

This repository contains a link to the dataset and instructions to download the Geo-YFCC dataset presented in the paper "[Adaptive Methods for Real-World Domain Generalization](https://arxiv.org/pdf/2103.15796.pdf)", appearing at CVPR 2021. This dataset contains a subset of the popular [YFCC100M dataset ](http://projects.dfki.uni-kl.de/yfcc100m/), that are partitioned based on the images' country of origin. Note that in order to limit the hosting space required, the dataset available here only contains the metadata and domain assignments, and *not* the original images. To obtain the original images, we urge the user to download them directly via the YFCC100M link (or use [this API](https://pypi.org/project/yfcc100m/)). 

Information about the dataset: We use geotags to partition images based on their country of origin. For the label space, we consider the 4K categories from ImageNet-5K not present in ILSVRC12. These categories are selected in order to eliminate biased prior knowledge from pre-training on ILSVRC12. For each of the 4K labels, we select the corresponding images from YFCC100M based on a simple keyword-filtering of image tags. This provides us 1261 categories with at least 1 image present, and each category is present in at least 5 countries. We group images by their country of origin and only retain countries that have at least 10K images. For any domain with more than 20K images, we randomly sub-sample to limit it to 20K images. Therefore, each domain (i.e., country) has anywhere between 10K-20K images, giving us a total of 1,147,059 images from 1,261 categories across 62 countries (domains), and each image is associated with a class label and country
(domain). We randomly partition the data in to 45 training, 7 validation and 15 test domains (by country). For each domain, we sample 3K points to create a per-domain test set and use the remaining points for training and validation.

The metadata file is available at [this Google Drive link](https://drive.google.com/file/d/1HvpAeEc37R9nLcI79iSeVCX2PYg3AgXZ/view?usp=sharing). To download this via CLI, we suggest using [gdown](https://pypi.org/project/gdown/). 
```
pip install gdown
gdown http://drive.google.com/uc?id=1HvpAeEc37R9nLcI79iSeVCX2PYg3AgXZ
echo "db7419355b1e9827a2cf8f480ee36120  GeoYFCC.tar.gz" | md5sum -c -
```
The file is downloaded correctly if the above code ends with `OK`. The md5sum for the correctly downloaded file should match `db7419355b1e9827a2cf8f480ee36120`. The file is tar-zipped and unzipped results in a `pickle` file that stores a `pandas` dataframe, described below.

| Column | Description |
| ----------- | ----------- |
| `yfcc_row_id` | Corresponding row within YFCC100M (present in the `yfcc100m_dataset` file from YFCC100M) |
| `label_ids` | Labels, since this dataset is multi-label this is a list |
| `country` | Plaintext name of domain (country) |
| `country_id`| Serialization of domain (`country`) from 0-61 |
| `in_5k_label_ids` | Corresponding labels in the ImageNet-5K dataset |
| `is_train` | Boolean specifying whether row is in the training image split |
| `yfcc_metadata` | Copy of the original YFCC metadata for image |

The default domain splits are `0-43` as training, `44-50` as validation domains, and `51-61` as test domains. Each domain is further split into a `train` and `test` split which is specified in the `is_train` field.

If you find this dataset relevant to your research, please consider citing our work below.
```
@InProceedings{dubey2021adaptive,
  title={Adaptive Methods for Real-World Domain Generalization},
  author={Dubey, Abhimanyu and Ramanathan, Vignesh and Pentland, Alex and Mahajan, Dhruv},
  booktitle = {IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  month = {June},
  year = {2021}
}
```

