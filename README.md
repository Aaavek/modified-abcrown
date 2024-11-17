# Modified alpha-beta-crown

In the context of veryfing nural networks [alpha-beta CROWN](https://github.com/Verified-Intelligence/alpha-beta-CROWN) is used to assess the robustness of networks against input pertubutions of the form $\ |I - I'| < \epsilon $.

Our implementation of the alpha-beta-crown checks some more specification where instead of all the pertubutions that satisfies the above spec, only ones where the changes of the image are limited to a $k \times k $ sub-image.

That is for a given image, k, model, $\epsilon$ our model verifies our model gives same output for all images of above mentioned pertubutions.

## Usage

### Installation and Setup

α,β-CROWN is tested on Python 3.11 and PyTorch 2.2.1 (lower versions, including Python 3.7 and PyTorch 1.11, may also work). It can be installed easily into a conda environment. If you don't have conda, you can install [miniconda](https://docs.conda.io/en/latest/miniconda.html).

The same setup is enough for our repository also

clone it using

```
git clone https://github.com/Verified-Intelligence/alpha-beta-CROWN.git
```

Setup the conda environment

```
# Remove the old environment, if necessary.
conda deactivate; conda env remove --name alpha-beta-crown
# install all dependents into the alpha-beta-crown environment
conda env create -f complete_verifier/environment.yaml --name alpha-beta-crown
# activate the environment
conda activate alpha-beta-crown
```


### Running

You can use `modified_ab.py` as our main implementation. To run the code

```

conda activate alpha-beta-crown  # activate the conda environment

cd complete_verifier


python modified_ab.py --config <path-to-config> --k <value-of-k> --imagepath <path-to-image> --epsilon <eps> --device cpu


# path-to-config - This is where we specify the details of our model

# value-of-k - int, should be less than the input image size

# eps - the value of epsilon for perturbations (It could also be mentioned in the config file)

# --device cpu - you could remove this if you have GPU to run on

```

The format of the config file from an example  `mnist_cnn_a_adv.yaml` :

```

model:
  name: mnist_cnn_4layer
  path: models/sdp/mnist_cnn_a_adv.model
  data_min: 0.0
  data_max: 1.0

data:
  dataset: None

specification:
  epsilon: 0.7
  norm: .inf

attack:
  pgd_restarts: 50

solver:
  batch_size: 1024
  beta-crown:
    iteration: 20

bab:
  timeout: 10


```

In the above config file, for the model, data_min, and data_max, are the min and max possible values for each pixel of the input image eg: It can be 0.0, 255.0 etc... for rgb images depending on the model.

model:path - This should be the path to our model

Specification - epsilon: can be given in the config file or from the command line both

bab:timeout can be changed for faster running of our model at a particular step


The output of running `python modified_ab.py --config exp_configs/tutorial_examples/mnist_cnn_a_adv.yaml --k 5 --epsilon 0.3 --imagepath image.jpg --device cpu > out.txt` is present in `out.txt` for reference.
