---
created: 2023-08-07 05:17
aliases: 
- Transformer Architecture
tags:
- rn
cssclasses:
- 
publish:
dg-publish: false
---

<!-- 
tags: 
-->

<!--internal
parent:: [[20230703011428 Transformers]]
child:: [[]]
related:: [[]]
-->

<!--external
- [ ] []()
-->
# Transformer Architecture

![[notes/images/Screenshot 2023-07-03 at 1.28.32 AM.png|250]]

- The [[notes/20230703011428 Transformers|Transformer]] architecture is split into two distinct parts, the [[notes/202308090432 Encoder|Encoder]] and the [[notes/202308090433 Decoder|Decoder]]
 ![[notes/images/Screenshot 2023-08-07 at 5.24.45 AM.png|250]]
- These components work in conjunction with each other and share a number of similarities
- The architecture of a [[notes/20230703011428 Transformers|Transformer]] includes <mark style="background: #FF5582A6;">six</mark> encoders and decoders
- each encoder has 
	- 1 multi-head self-attention and 
	- 1 feed-forward neural network
- each decoder has 
	- 1 multi-head self-attention, 
	- 1 masked multi-head attention and 
	- 1 feed-forward neural network
- In between each sub-layer, there are Add and Normalized layer to [[notes/20230703034916 Normalization|Normalize]] the output out of the sub-layer
	- The normalization technique used here is called <mark style="background: #FF5582A6;">Layer Normalization</mark>
- The words in the text are tokenized
	- we can choose from multiple tokenization methods 
	- For example, token IDs can match two complete words, or represent parts of words
	- **It important is that once we've selected a tokenizer to train the model, we must use the same tokenizer to generate text.**
- The model processes each of the input tokens in parallel
- The tokens are [[notes/20230703031649 Word Embedding|Embedded]] 
	- Each token is represented as a vector and occupies a unique location within that high-dimensional space trainable vector space
	- Each token ID in the vocabulary is matched to a multi-dimensional vector ![[notes/images/Screenshot 2023-08-07 at 6.00.01 AM.png|500]]
	- **The intuition is that these vectors will learn to encode the meaning and context of individual tokens in the input sequence**
- The tokens also undergo [[notes/20230703031923 Positional Encoding|Positional Encoded]] ![[notes/images/Screenshot 2023-08-07 at 6.40.06 AM.png]]
- **Parallelization** is performed with feeding all words (tokenized) of the sentence through the <mark style="background: #FF5582A6;">multi-head self-attention layer</mark>
	- self-attention allows the model to attend to different parts of the input sequence to better capture the contextual dependencies between the words. 
	- The weights that are learned during training and stored in these layers reflect the importance of each word in that input sequence to all other words in the sequence. 
	- But it does not happen just once, the transformer architecture has multi-headed self-attention
	- This means that multiple sets of self-attention weights or heads are learned in parallel independent of each other.
	- The intuition here is that each self-attention head will learn a different aspect of language for example, one head might see the relationship between the people entities in our sentence while another head might focus on the activity of the sentence, yet another head might focus whether the words rhyme. 
	- It is important to note that we don't dictate ahead of time what aspects of language the attention heads will learn
	- The weights of each head are randomly initialized and given sufficient training data and time, each will learn different aspects of language. 
- After all of the attention weights have been applied to input data, the output is processed through a fully-connected feed-forward network.
- The output of FFF network is a vector of logits proportional to the probability score for each and every token in the tokenizer dictionary
- This vector is logit is passed to a final **softmax** layer, where these logits are normalized into a probability score for each word
- The output of the softmax includes a probability for every single word in the vocabulary, so there's likely to be thousands of scores here. 
- one single token will have a score higher than the rest which is likeliest predicted token. 
- However, there are a number of methods that you can use to vary the final selection from this vector of probabilities.
