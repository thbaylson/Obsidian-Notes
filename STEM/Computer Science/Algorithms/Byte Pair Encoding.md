
[[Algorithms]]

From [Wikipedia- Byte pair encoding](https://en.wikipedia.org/wiki/Byte_pair_encoding):
Byte pair encoding (BPE) is an algorithm for encoding strings of text into tabular form for use in downstream modeling. It's modification is notable as the large language model tokenizer with an ability to combine both tokens that encode single characters and those that encode whole words.

From [Geeks4Geeks- Byte-Pair Encoding (BPE) in NLP](https://www.geeksforgeeks.org/byte-pair-encoding-bpe-in-nlp/):
The basic idea of BPE is to iteratively merge the most frequent pair of consecutive bytes or characters in a text corpus until a predefined vocabulary size is reached. The resulting subword units can be used to represent the original text in a more compact and efficient way.

From [HuggingFace's NLP Course- Chapter 6: The Tokenizers Library](https://huggingface.co/learn/nlp-course/en/chapter6/5):
BPE training starts by computing the unique set of words used in the corpus (after the normalization and pre-tokenization steps are completed), then building the vocabulary by taking all the symbols used to write those words. For real-world cases, that base vocabulary will contain all the ASCII characters, at the very least, and probably some Unicode characters as well. If an example you are tokenizing uses a character that is not in the training corpus, that character will be converted to the unknown token. That’s one reason why lots of NLP models are very bad at analyzing content with emojis, for instance.

After getting this base vocabulary, we add new tokens until the desired vocabulary size is reached by learning merges, which are rules to merge two elements of the existing vocabulary together into a new one. So, at the beginning these merges will create tokens with two characters, and then, as training progresses, longer subwords.

At any step during the tokenizer training, the BPE algorithm will search for the most frequent pair of existing tokens (by “pair,” here we mean two consecutive tokens in a word). That most frequent pair is the one that will be merged, and we rinse and repeat for the next step.