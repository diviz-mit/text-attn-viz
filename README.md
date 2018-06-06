# Attention Visualizer

This is a tool to visualize the distribution of attention in a text-based sequence-to-sequence task such as summarization. As you hover your mouse over the decoded words, the tool shows a heatmap of attention over the source words. A demo of the original source code, from [See et al.](https://github.com/abisee/attn_vis) can be found [here](http://www.abigailsee.com/2017/04/16/taming-rnns-for-better-summarization.html) (scroll down to "Example Output" section).

Additionally, for pointer-generator networks such as the described in [this paper](https://arxiv.org/abs/1704.04368), the tool displays the _generation probability_ of each decoded word. This tool was designed to work with the [Tensorflow code](https://github.com/abisee/pointer-generator) for the paper.

## To run

To run the visualizer, run
```
python -m http.server
```
from this directory and open `http://localhost:8000/,` or the port used to initialize the interface. The visualizer will show some example data.

## To use your own data

To visualize your own data, you can add your images to the 'example\_images' folder, along with the proper JSON file in the 'example\_jsons,' or you can also use the <b>Upload</b> button provided in the interface. The JSON file, either produced by this [Tensorflow code](https://github.com/abisee/pointer-generator), or [our modified model](https://github.com/diviz-mit/pointer_gen). Each JSON file should contain the following fields:


* `article_lst`: the article (or source text) as a list of words.
* `decoded_lst`: the decoded (machine-generated) summary as a list of words.
* `abstract_str`: the reference summary as a single string.
* `attn_dists`: a list having the same length as the `decoded_lst`, containing lists of length "attention length", containing probabilities.  Note attention length must be less than or equal to the `article_lst`. The article may have 500 words, but if one feeds the first 200 words into the model, the attention will be of length 200.  In this case, the visualizer marks the truncation point in the article.
* `p_gens` a list containing generation probabilities for each word in the `decoded_lst`.
* `url`: the url, or file name, of the infographic being decoded.
* `positions`: A list of bounding boxes for each specific word in `decoded_lst`.  The list contains 8 values per word, for a total of 8 x `decoded_lst` values.  The 8 values correspond to the coordinates of each corner in the bounding box going clock-wise from the top-left of the image.  It is ordered as <b>top_left_x, top_left_y, top_right_x, top_right_y, bottom_right_x, bottom_right_y, bottom_left_x, bottom_left_y</b>, where <b>x</b> is the horizontal and <b>y</b> is the vertical distance from the top left corner of the infographic.

WARNING: Make sure that none of the strings in `article_lst`, `decoded_lst`, or `abstract_str` contain `<angled brackets>`. These will interfere with the HTML and can result in text not being displayed.
