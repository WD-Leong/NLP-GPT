# NLP Language Model using Dilated Causal Convolutional Networks 
This repository includes the codes that I explored for language modelling using a 1D Dilated Causal Convolutional Networks. It is inspired from the [WaveNet Model](https://deepmind.com/blog/article/wavenet-generative-model-raw-audio), which I modified and applied to the Language Modelling task. By applying dilated causal convolutional networks (CNN), the model increases the receptive field exponentially to allow it to model long range dependencies and sequences. The first layer has a dilation rate of 1, but subsequent layers have a dilation rate of 2. 

This repository includes a code to train the data on the [Reddit Jokes](https://github.com/taivop/joke-dataset) dataset. To train the model, first process the Reddit data by running
```
python process_reddit_jokes.py
```
if word tokens are used, or
```
python process_reddit_jokes_subword.py
```
if sub-word tokens are used. After processing the data, run
```
python reddit_jokes_seq_cnn_train.py
```
to train the model. The CNN structure allows training to be optimized on a GPU. To perform inference, the model runs auto-regressively on the seed input, or the current output sequence. To perform inference, run the code
```
python reddit_jokes_seq_cnn_test.py
```
Instead of using temperature, `tf.random.categorical` function is applied on the logits directly to introduce diversity in the inferred joke. Depending on the output sequence length, the inference can take some time. In the processing, the score assigned to the joke is categorized into 3 classes - bad, ok and good - to study its effect on the quality of the jokes generated.

## Dilated Convolutional Networks
The dilated convolutional neural network applied in the [WaveNet](https://arxiv.org/pdf/1609.03499.pdf) paper allows it to cover thousands of timesteps, making it suitable to generate synthetic utterances. In this implementation, position embeddings, inspired from the [Convolutional Sequence to Sequence Learning](https://arxiv.org/pdf/1705.03122.pdf) paper, is applied to improve the model's performance. The positional embeddings are applied at two levels of granularity - at the input of each convolutional layer, as well as the input of each stack layer.

![WaveNet's Dilated 1D Convolutional Network](WaveNet_Dilated_Convolution.JPG)

Fig 1.: WaveNet's Dilated Convolutional Network (source: [WaveNet](https://arxiv.org/pdf/1609.03499.pdf))

## Outputs
After training a model with 256 filters, 4 layers and 2 stacks on Reddit jokes with 30 tokens or less for 20000 iterations, some outputs are provided in this section.
```
Input Phrase:
did you hear
Generated Phrase:
did you hear about the restaurant on the moon ? yeah , great food but no atmosphere . EOS

Input Phrase:
what is the difference
Generated Phrase:
what is the difference between a sharply dressed man on a unicycle and a poorly dressed man on a bicycle ?? attire EOS

Input Phrase:
why did
Generated Phrase:
why did the cat stop singing ? because it was out of tuna half a note . EOS
```
The hardware used is an Nvidia Quadro P1000 4GB Graphics Card.
