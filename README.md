# Product Category inference

Here you'll find code for inferring a product catefory from **text** and **image data**.
<br>
<br>
The user should be able to tun `categorization.ipynb` and download the trained models.

# Chosen approach

The inference will be given by an ensemble of two separated inferences, one image inference and one text inference.
The final result will be give my a merge of those two inferences.

## Image inference

`- ImageCategorization.ipynb` -> [should run on google colab](https://colab.research.google.com/drive/1HpSTE3GQBLTo9X4NHl5on8K9Zn8EGvnF?usp=sharing)

- The approach chosen here was to find a pre-trained image classification model and **fine-tune it on a representative dataset**.
<br>
- The chosen dataset for image classification was the [fashion-product-images-small](https://www.kaggle.com/paramaggarwal/fashion-product-images-small), since it tries to solve a simmilar task - product category classification - with different data distribution.
<br>

## Text inference

`- TextCategorization.ipynb` -> [should run on google colab](https://colab.research.google.com/drive/1lZxNpzVrEgbuWKCUPp9_dwEUbtW_AjnL?usp=sharing)

- The approach chosen here was almost the same as the one for images, but here, we **unsupervisedly** try to learn the **words embeddings** on the test set first, we do that so that the model can have the word embedding priors of the targeted dataset. And second, we **supervisely** fit out model for category classification.
<br>
- The chosen dataset for text classification was also the [fashion-product-images-small](https://www.kaggle.com/paramaggarwal/fashion-product-images-small), since it tries to solve a simmilar task - product category classification - with different data distribution.
<br>
- The textual model is based on the **Continuous bag of words (CBOW)** model, in which it tries to infer a word from all the words in a surrounding window.

<br>

# Final inference

`- categorization.ipynb` -> should run with `jupyter notebook`

`- categorization.pdf` -> reproduction of `categorization.ipynb`

The inference will be given by:
- I) First averaging the probability distribution of both classifications (different classes than the target categories, since the models were trained on another dataset),
- II) And then comparing the resultant probability distribution to the **target categories projection** on the same categories/labels used during training.
- III) The selected final category will be that which holds the smallest the KL-divergence (distance between two probability distribution).

# Final Remarks

> - Why are you designing the solution in this way?

- No supervised data was given.

> - What are the aspects that you considered when designing?

- Answered on the above paragraphs

> - What are the cases your solution covers, how are they covered and why are they important?

- Ideally, it covers better the cases where examples follow the same distribution shown on the [train dataset](https://www.kaggle.com/paramaggarwal/fashion-product-images-small). And it is expected to lose performance when evaluated on another distribution, since all the models are susceptible to adversarial examples and overfit/bad fitting/

> - What are the cases your solution does not cover and what are the ways you can extend your current solution for them?

- As shown on the `categorization.ipynb`, the model performed poorly . And that is due to fact that models weren't properly evaluated and tuned on the target distribution, but only on the [train dataset](https://www.kaggle.com/paramaggarwal/fashion-product-images-small) dataset. With more time, it would be possible to try to better fit and evaluate both models individually, checking the error cases on the desired final dataset and also, find a more representative dataset for the first training phase.
