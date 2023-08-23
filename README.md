# Visual_Question_Answer
Introduction
Ever envisioned possessing an AI akin to Iron Man's Jarvis—capable of answering queries about your surroundings or any image, just like in your dreams? Imagine swiftly locating pictures of your beloved pet amidst a sea of snapshots or uncovering that elusive photo buried deep within an avalanche of images.

Although this may seem like an ambitious dream, the reality isn't too distant. Tremendous strides have been made in AI, particularly in the realm of multimodal capabilities that fuse natural language and visual understanding. This comprehensive tutorial delves into cutting-edge techniques (BLIP, BLIP-2, and GIT) and demonstrates their practical implementation in Python.

Table of Contents:

1. Introduction
2. Visual Question Answering
3. Theoretical Underpinnings
4. BLIP
     Multimodal Mixture of Encoder-Decoder (MED)
     Captioning and Filtering (CapFilt)
7. BLIP-2
     Q-Former
9. GIT


Visual Question Answering
Venturing into the captivating frontiers of AI research, Visual Question Answering (VQA) involves addressing open-ended questions about images. This task assigns computers the role of delivering pertinent and coherent answers to text-based queries concerning specific images. Achieving this feat demands a profound grasp of visual and linguistic processing, coupled with a touch of common-sense knowledge to offer effective responses.

Beyond the scenarios mentioned earlier, VQA boasts a myriad of practical applications, including aiding the visually impaired, refining image retrieval systems, aiding medical diagnostics, enriching educational interactions, streamlining video searches, and the list continues.

Theoretical Underpinnings
BLIP
Proposed by Salesforce researchers, BLIP constitutes a Vision-Language Pre-training (VLP) framework that thrives on learning from noisy image-text pairs. VLP frameworks synergize vision and language, yielding a broader spectrum of downstream tasks compared to existing methods. Both BLIP and BLIP-2 build upon the Transformer architecture, a favored choice for various natural language processing tasks due to its ability to grapple with extensive dependencies and scalability.

BLIP's success hinges on two pivotal components: Multimodal Mixture of Encoder-Decoder (MED) and Captioning and Filtering (CapFilt).

Multimodal Mixture of Encoder-Decoder (MED)
The MED model is jointly pre-trained with three vision-language objectives: image-text contrastive learning, image-text matching, and image-conditioned language modeling. The architecture unfolds as follows:

![Screenshot 2023-08-23 181712](https://github.com/ZEBAAFROZ/Visual_Question_Answer/assets/93834320/9f1f32f7-f6ff-4ca5-a1b1-a3a61f8f481e)


Note the shared parameters among identically colored blocks. An image encoder captures the image's latent space representation, conditioning both the text encoder and decoder through cross-attention layers. This cross-attention enables models to discern contextual relationships between distinct data sets—the input text and image.

This framework operates diversely:

1. Unimodal encoder: The second model in the diagram achieves this, encoding only the text through image-text contrastive learning (ITC) loss.
2. Image-grounded text encoder: As the third model, it encodes text with injected visual data via cross-attention layers, driven by image-text matching (ITM) loss.
3. Image-grounded text decoder: The fourth model decodes text with visual information, learned via language modeling (LM) loss.

Captioning and Filtering (CapFilt)

[Diagram illustration]

Given the need for massive data and its associated high annotation costs, CapFilt introduces two modules derived from the same pre-trained objective. The Captioner generates captions for web images, acting as an image-grounded text decoder fine-tuned with LM objectives. The Filter removes noisy image-text pairs, functioning as an image-grounded text encoder fine-tuned with ITC and ITM objectives. Filtered pairs, alongside human-labeled captions, constitute the new dataset for further pre-training.

BLIP-2
BLIP-2, an advanced Visual Question Answering model, refines its predecessor—BLIP—through a range of enhancements.

BLIP-2 adopts a two-stream architecture, one handling images (akin to an image encoder) and the other processing questions (similar to a Large Language Model or LLM). These fixed streams integrate features from visual and textual inputs via an innovative fusion mechanism known as Q-Former.

Q-Former

[Diagram illustration]

The Q-Former comprises two submodules:

1. Image transformer: At the diagram's center, it interacts with the frozen image encoder for visual feature extraction. Learnable queries, inputted here, interact through self-attention layers and cross-attention with image features. These queries can also interact with text by combining learnable queries and text tokens.
2. Text transformer: Positioned to the right, it functions as a text decoder and encoder. Text input similarly interacts with learnable queries. Both submodules share self-attention layers.

Q-Former's training covers:

1. Image-text contrastive learning: Maximizes mutual information by contrasting image-text similarity between positive and negative pairs.
2. Image-grounded text generation: Learnable image queries extract visual features from frozen image encoder data, aiding text decoding.
3. Image-text matching: A binary classification task distinguishing positive and negative image-text pairs.

Attention masks include:

1. Bi-directional self-attention mask: Enables interactions between learnable query tokens and text tokens.
2. Multi-modal causal self-attention mask: Permits query token interactions and text tokens with prior predictions and queries.
3. Uni-modal self-attention mask: Facilitates interactions among query and text tokens but not inter-group interactions.

BLIP-2's generative pre-training stage involves the Q-Former linking image encoder to LLM. Output query embeddings are added to text embeddings, acting as visual prompts for LLM, mitigating vision-language alignment challenges.

GIT
Generative Image-to-text Transformer (GIT) unifies vision-language tasks—captions and question answering. Microsoft researchers introduced GIT, marked by its simplicity with a sole image encoder and text decoder, catering to a singular language modeling task.

Trained on an extensive dataset of 0.8 billion image-text pairs, GIT's substantial pre-training data and model size amplify performance. GIT excels on diverse benchmarks, even surpassing humans on TextCaps benchmark.

[Diagram illustration]

1. Image encoder: Contrastive pre-trained, takes raw image, yields compact feature map, flattened to features, and projected to D dimensions.
2. Text decoder: Randomly initialized transformer predicting text description. Comprises transformer blocks with self-attention and feed-forward layers. Tokenized text with D dimensions and positional encoding joins image features for prediction.

GIT's holistic training involves predicting next words in sentences using language modeling. Notably, the attention mask ensures text tokens rely on preceding tokens and all image tokens, enabling comprehensive inter-token interactions.

GIT's application extends to video-based VQA by processing frames through the image encoder and adding temporal embeddings for enriched temporal information.

[End of rephrased text.]
