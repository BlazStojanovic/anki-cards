<!-- note_key: 1529397b2994 -->
BF: How does a bag-of-words model work?
BB: Given a vocabulary (all possible tokens that can be represented), represent an individual string as a sparse vector counting the number of occurences (i.e. a bag-of-words).

---

<!-- note_key: 6252ac6e0397 -->
BF: What is the key idea behind *word2vec*?
BB: That we should use dense embeddings to represent words. Train on predicting word similarity (e.g. measured by appearing *close* in text). 

The representation *word2vec* chooses, is a CBOW (continuous-bag-of-words), where the representation represent each word with a vector, such that it is possible to predict a word using the sum of the vectors of its neighbors.

---

<!-- note_key: 20ab8ff3759f -->
BF: Explain the logical sequence of developments to arrive at the transformer architecture!
BB: 1. Start with bag of words approaches, i.e. just word counting
2. Actually, we can use dense embeddings to encode semantic meaning of tokens and we can pre-train them (word2vec)
3. Now the problem becomes that embeddings are completely static (e.g. \*bank\* can mean financial or river bank but this is not reflected in the embeddings)
4. Key insight from 3. is that we should really be modelling sequences -> use \*sequence models.\* [[RNN]]s!
5. Another new idea RNNs are [[Auto-regressive models]] - I.e. to process $n$-th input it needs to first process the first $n-1$ inputs
6. Problem with RNNs is the context embedding, the entire sequence is represented as a \*single\* dense embedding
7. Solution -> \*\*[[attention]]\*\* focus the model on the parts of the input which are highly relevant to one another and amplify their signal. 
8. You can use RNN encoder \+ Attention mechanism (i.e. attention decoder), which is very powerful!
9. But, the sequential part is problematic from a computational perspective, it prevents \*parallelization\*
10. What if we get rid of sequence modelling --> "Attention is all you need!" and the Transformer. The transformer can be trained in parallel

---

<!-- note_key: f3807fe3b1d8 -->
BF: How many parameters to GPT1, GPT2, and GPT3 have?
BB: GPT1: 117M
GPT2: 1.5B
GPT3. 175B

---

<!-- note_key: e9b26bac4676 -->
BF: What does VRAM stand for?
BB: Video Random-Access Memory

---

<!-- note_key: 42e95ba0088f -->
BF: Why do LLMs need to use a tokenizer?
BB: LLMs need to work with numbers (IDs) not raw text. Tokenizers bridge the gap between text and numbers.

---

<!-- note_key: d57a2e909af5 -->
BF: Why don't we use words as tokens?
BB: 1. Vocabulary explodes in size, tokenizers allow us to hit the right balance between character-level (too granular) and word level (too sparse)
2. Out-of-vocabulary problem (new slang, misspelings, etc.)
3. Poor multi-lingual support
4. Loss of structure (happy, happiness, unhappy)

---

<!-- note_key: 38ef4d319773 -->
BF: What are common limitations associated with use of Tokenizers?
BB: 1. Character level tasks are hard (reversing, counting letters, spelling, detecting palindromes)
2. Trailing spaces and formatting quirks (" hello" can be different to "hello ")
3. Language bias (text in other languages needs more tokens to be represented!)
4. Arithmetic and symbolic reasoning (due to fragmentation any numeric reasoning is difficult)
5. Adversarial tokens exist (tokens with little representation in training may result in sporratic behavior)

---

<!-- note_key: 335bc208355d -->
BF: What are the most common tokenizers and core ideas behind them?
BB: 1) Byte Pair Encoding (BPE) --> **Core idea**: Start with individual characters, then iteratively merge the most frequent pairs. Used by GPT-2, GPT-3, GPT-4, LLaMA...

2) WordPiece --> **Core idea**: Similar to BPE, but chooses merges based on likelihood rather than raw frequency. Used by BERT, original Transformers...

3) Unigram Language Model --> **Core idea:** Start with a large vocabulary and iteratively remove tokens. Used by T5, some multilingual models...

---

<!-- note_key: 3a8df34fd684 -->
BF: What are the four notable tokenization reigmes?
BB: 1) Word tokens: Not really used by LLMs
2) Subword tokens: Most common representation within LLMs
3) Character tokens
4) Byte tokens: Can be competitive for multilingual scenarios. Also commonly used as a fallback for unrecognized tokens in subword scenarios.

---

<!-- note_key: 8ca8cce65312 -->
BF: How many transformer blocks did the original transformer have?
BB: 6

---

<!-- note_key: 7fef1a0595ad -->
BF: Describe the approximate computation which happens in an *attention* layer (generative transformer)!
BB: 1. Starting position: The attention layer (of a generative LLM) is processing attention for a single position
2. The inputs to the layer are:
 1. The vector representation of the current position or token
 2. The vector representations of the previous tokens
3. The goal is to produce a new representation of the current position that incorporates relevant information from the previous tokens
4. The training process produces three projection matrices that produce the components that interact in this calculation:
 1. The \*\*query projection matrix\*\*
 2. The \*\*key projection matrix\*\*
 3. The \*\*value projection matrix\*\*

Above is the starting position before the computation starts! Computation:
1. Multiply the inputs by the projection matrices to create three new matrices
 1. queries
 2. keys
 3. values
2. These projections help carry out the two steps of attention:
 1. Relevance scoring
 2. Combining information
3. \*\*Relevance scoring:\*\* Happens by \*multiplying the query vector of the current position with the keys matrix!\* This produces a score stating how relevant each previous token is (to this particular \*query\* token). Then pass through a [[Softmax]] layer so that this $q\\cdot K$ vector is normalized.
4. \*\*Combining information:\*\* We take these relevance scores, \*we multiply the value vector associated with each token by that tokens relevance score!\* Finally we sum up those \*weighted\* value vectors to arrive at a new vector!
5. This vector encodes contextual information from prior tokens into the embedding of the current token!

$$
\\operatorname{Attn}\\left(Q, K, V\\right) = \\operatorname{softmax}\\left(\\frac{Q\\cdot K^T}{\\sqrt{d_k}}\\right)\\cdot V
$$

---

<!-- note_key: c7beccef8cc3 -->
BF: Explain the difference between *multi-head, multi-query, and grouped-query* attention
BB: - **Multi-head** attention uses unique Q, K, V projections for each head
- **Multi-query** is an optimization where K, V are shared across all heads but Q is unique
- **Grouped-query** is a Multi-query approach but for several groups of heads (i.e. K, V are shared across each group but not all heads), Q is still unique for each head.

---

<!-- note_key: a474bb5ce050 -->
BF: Explain *flash attention*!
BB: Popular method and implementation of attention which provides significant speedups for both training and inference of transformer LLMs on GPUs. 

It speeds up the attention calculation by optimizing what values are loaded and moved between a GPUs shared memory (SRAM) and high bandwidth memory (HBM)

---

<!-- note_key: d0f705b1ad4d -->

---

<!-- note_key: 841dcafdb9e5 -->
BF: Explain how *contrained output* can be enforced for LLMs in some cases!
BB: You can just enforce sampling at the token stage (but can be tricky for some cases).

At each autoregressive step, you have:
1. \*\*A partial JSON string\*\* (what's been generated so far)
2. \*\*A JSON grammar/schema\*\* (the rules for valid JSON)
3. \*\*The model's next-token probabilities\*\* (what it wants to generate)
The key is that you can \*\*parse the partial JSON incrementally\*\* to determine what tokens would keep it valid.

\*\*Step-by-step\*\*:
Let's say you've generated: `{"name": "Al`

\*\*Step 1: Parse the partial string\*\*
- A JSON parser can tell you the current state: "We're inside a string value for key 'name'"

\*\*Step 2: Determine valid next tokens\*\*
- In this state, valid tokens are:
 - Any printable characters (to continue the string)
 - `"` (to close the string)
 - But NOT: `{`, `}`, `:`, etc. (these would break the JSON)

\*\*Step 3: Mask the logits\*\*
- Take the model's probability distribution over all tokens
- Set probability to zero (or -infinity in log space) for invalid tokens
- Renormalize the remaining valid tokens
- Sample from this constrained distribution

\*\*Step 4: Update parser state and repeat\*\*
- Generate the chosen token
- Update the parser's state machine
- Repeat for the next token

---

<!-- note_key: dbd02bd6454f -->
BF: Rougly how does a vision transformer work?
BB: **Step 1: Split Image into Patches**

```
Input image: 224×224×3 (height × width × channels)
Patch size: 16×16
Number of patches: (224/16) × (224/16) = 14 × 14 = 196 patches
Each patch: 16×16×3 = 768 values
```

**Step 2: Flatten Each Patch** Each 16×16×3 patch becomes a flat vector of 768 numbers

**Step 3: Linear Projection (The Actual Embedding)** This is the core embedding step - a learned linear transformation

\# Flattened patch vector
x = [768 values]  \# shape: (768,)

\# Learned weight matrix
W = [768 × D]  \# D is the embedding dimension (e.g., 768)

\# Bias term
b = [D values]

\# Embedding
embedding = x @ W + b  \# shape: (D,)

## Why a Linear Projection?

This is essentially a **learned feature extraction** layer. Instead of hand-crafted features, the model learns what aspects of each patch are important.

Think of it as learning 768 different "filters" or "feature detectors" that look at the raw patch pixels.

## Alternative: Convolutional Embedding (More Common)

In practice, most ViT implementations use a **convolutional layer** instead, which is mathematically equivalent but more efficient:

python

```
\# Instead of flatten --> linear
\# Use a convolution with kernel_size = patch_size and stride = patch_size

self.patch_embed = nn.Conv2d(
    in_channels=3,
    out_channels=embed_dim,  \# 768
    kernel_size=patch_size,  \# 16
    stride=patch_size  \# 16 (non-overlapping patches)
)

\# For the full image
image = [B, 3, 224, 224]  \# batch, channels, height, width
embeddings = self.patch_embed(image)  \# [B, 768, 14, 14]
embeddings = embeddings.flatten(2)  \# [B, 768, 196]
embeddings = embeddings.transpose(1, 2)  \# [B, 196, 768]
```

## Adding Position Information

The patch embeddings alone don't encode position, so we add **positional embeddings**:

python

```
\# Learned positional embeddings for each of the 196 positions
self.pos_embed = nn.Parameter(torch.randn(1, 196, 768))

\# Add to patch embeddings
embeddings = patch_embeddings + self.pos_embed
```

These are also learned during training and help the model understand spatial relationships.

## The Complete Pipeline

python

```
class PatchEmbedding(nn.Module):
    def __init__(self, img_size=224, patch_size=16, embed_dim=768):
        super().__init__()
        self.num_patches = (img_size // patch_size) ** 2
  
        \# Convolutional patch embedding
        self.proj = nn.Conv2d(3, embed_dim,
                             kernel_size=patch_size,
                             stride=patch_size)
  
        \# Learnable positional embeddings
        self.pos_embed = nn.Parameter(
            torch.randn(1, self.num_patches + 1, embed_dim)
        )
  
        \# [CLS] token (for classification)
        self.cls_token = nn.Parameter(torch.randn(1, 1, embed_dim))
  
    def forward(self, x):
        B = x.shape[0]
  
        \# Patch embedding: [B, 3, 224, 224] --> [B, 768, 14, 14]
        x = self.proj(x)
  
        \# Reshape: [B, 768, 14, 14] --> [B, 196, 768]
        x = x.flatten(2).transpose(1, 2)
  
        \# Add [CLS] token: [B, 196, 768] --> [B, 197, 768]
        cls_tokens = self.cls_token.expand(B, -1, -1)
        x = torch.cat([cls_tokens, x], dim=1)
  
        \# Add positional embeddings
        x = x + self.pos_embed
  
        return x
```

## Key Insights

1. **It's just a learned linear layer** - nothing fancy, the magic is in what the model learns
2. **Each patch is embedded independently** - no interaction between patches at this stage (that happens in the transformer layers)
3. **The embedding is learned end-to-end** - initialized randomly, optimized during training
4. **Position is added separately** - unlike CNNs where position is implicit, ViTs add it explicitly

---

<!-- note_key: 9b1aa9772931 -->
BF: What are the embedding sizes of GPT1, GPT2, and GPT3 models?
BB: GPT1 -> 768
GPT2 -> 768
GPT3 -> 12_288

---

<!-- note_key: 4252672aa2ab -->
BF: What are positional embeddings?
BB: Add position/order information to input embeddings, crucial for architectures like Transformers that have **no inherent notion of sequence order**.

**Main approaches:** 

- Static absolute positional embeddings (encodings) - sinusoidal
- Learned absolute positional positional embeddings - lookup
- Relative positoinal encodings - RoPE, T5-style bias, ALiBI

**Key Takeaway:** Positional embeddings inject sequence order into Transformers by encoding position information, enabling them to distinguish token order despite parallel processing.

---

<!-- note_key: b1aa330a267f -->
BF: What is the reason behind the normalization in self-attention? Explain intuitively!
BB: **The Normalization:** In self-attention, we divide attention scores by √d_k before softmax:

python

```
scores = Q @ K.T / sqrt(d_k)  \# Scaling
attention_weights = softmax(scores)
output = attention_weights @ V
```

```
Intuitive Explanation:

Problem without normalization:

- Q and K are vectors of dimension d_k
- Dot product Q·K sums d_k terms
- As d_k increases, dot products grow larger in magnitude
- Large values --> softmax saturates into near one-hot distributions

Example:

python
```
\# d_k = 512
Q·K = sum of 512 terms ≈ large magnitude (e.g., 50)

softmax([50, 48, 45]) ≈ [0.95, 0.04, 0.01]  \# Very peaked!
```

Why this is bad:

- Extremely peaked softmax --> tiny gradients (gradient vanishing)
- Attention focuses too sharply on one token
- Model loses ability to attend to multiple relevant tokens
- Training becomes unstable

With √d_k scaling:

python
```
\# d_k = 512, sqrt(d_k) ≈ 22.6
(Q·K) / sqrt(d_k) ≈ 50 / 22.6 ≈ 2.2

softmax([2.2, 2.1, 2.0]) ≈ [0.37, 0.34, 0.29]  \# Smoother!
```

Statistical Intuition:

- If Q and K have unit variance, their dot product has variance ≈ d_k
- Standard deviation grows as √d_k
- Dividing by √d_k normalizes the variance back to ~1
- Keeps scores in a reasonable range regardless of dimension

Analogy: Think of it like averaging vs summing grades:

- Without scaling: Sum 512 test scores --> huge numbers (25,600/25,600)
- With scaling: Average 512 test scores --> reasonable scale (50/50)
- Averaging (dividing by √n) keeps values comparable across different test counts

Benefits:

1. Stable gradients: Softmax operates in its linear regime
2. Better attention distribution: Can attend to multiple tokens
3. Dimension-independent: Works consistently across different d_k values
4. Faster convergence: More stable training dynamics

Key Takeaway: Dividing by √d_k prevents dot products from growing too large with dimension, keeping softmax in a range with good gradients and allowing smooth attention distributions over multiple tokens.
```

---

<!-- note_key: fabe55ea6e88 -->
BF: What is causal (masked) attention, why/when is it used? (needs update)
BB: **Definition:** Causal attention prevents tokens from attending to future tokens in a sequence by masking out future positions. Each token can only attend to itself and previous tokens.

**How It Works:**

Apply a mask before softmax to set future positions to -∞:

```
\# Attention scores: [batch, heads, seq_len, seq_len]
scores = Q @ K.T / sqrt(d_k)

\# Causal mask (upper triangular with -inf)
mask = torch.triu(torch.ones(seq_len, seq_len) \* float('-inf'), diagonal=1)
\# [[0,   -inf, -inf, -inf],
\#  [0,    0,   -inf, -inf],
\#  [0,    0,    0,   -inf],
\#  [0,    0,    0,    0  ]]

scores = scores \+ mask  \# Future positions become -inf
attention_weights = softmax(scores, dim=-1)  \# -inf --> 0 after softmax
```

**Visual Example:**

```
Input: "The cat sat"

Without mask (bidirectional):
"The" can attend to: [The, cat, sat]
"cat" can attend to: [The, cat, sat]
"sat" can attend to: [The, cat, sat]

With mask (causal):
"The" can attend to: [The]
"cat" can attend to: [The, cat]
"sat" can attend to: [The, cat, sat]
```

**Why Use Causal Attention?**

**1. Autoregressive Generation:** Models must predict next token using only past context, not future information.

**2. Training-Inference Consistency:**

- During inference: model generates one token at a time (can't see future)
- During training: if allowed to see future, model learns unrealistic dependencies
- Causal masking ensures training matches inference conditions

**3. Prevents Information Leakage:** Without masking, model would "cheat" by looking ahead at answers during training.

**When It's Used:**

**Used in:**

- **GPT models** (decoder-only): All attention is causal for next-token prediction
- **Decoder in Transformer**: Masked self-attention in decoder blocks
- **Language modeling tasks**: Predicting next word/token

**NOT used in:**

- **BERT** (encoder-only): Uses bidirectional attention for understanding context
- **Encoder in Transformer**: Can see full input sequence
- **Classification tasks**: Need full context to classify

**Architecture Comparison:**

python

```
\# GPT (Decoder-only) - Causal everywhere
x = token_embedding \+ pos_embedding
for layer in layers:
    x = causal_self_attention(x)  \# Masked
    x = ffn(x)

\# BERT (Encoder-only) - Bidirectional
x = token_embedding \+ pos_embedding  
for layer in layers:
    x = self_attention(x)  \# NOT masked
    x = ffn(x)

\# Original Transformer - Both
\# Encoder: bidirectional attention
\# Decoder: causal self-attention \+ cross-attention to encoder
```

**Implementation Detail:**

python

```
\# Creating causal mask
def create_causal_mask(seq_len):
    mask = torch.triu(torch.ones(seq_len, seq_len), diagonal=1).bool()
    return mask.masked_fill(mask, float('-inf'))

\# Or using PyTorch built-in
mask = nn.Transformer.generate_square_subsequent_mask(seq_len)
```

**Key Difference from Padding Mask:**

- **Causal mask**: Structural - prevents looking ahead (same for all examples)
- **Padding mask**: Data-specific - ignores padding tokens (varies per example)
- Both can be combined!

**Key Takeaway:** Causal attention masks future tokens to ensure autoregressive models only use past context, maintaining consistency between training and generation. Essential for GPT-style models and language generation tasks.

---

<!-- note_key: f47ce3cd7c91 -->
BF: What is the softmax function, and why is it useful?
BB: **Definition:** Softmax converts a vector of real numbers (logits) into a probability distribution: all values between 0 and 1 that sum to 1.

**Formula:**

```
softmax(x_i) = exp(x_i) / Σ exp(x_j)
```

**Example:**

```
logits = [2.0, 1.0, 0.1]
exp_vals = [e^2.0, e^1.0, e^0.1] = [7.39, 2.72, 1.11]
sum_exp = 7.39 \+ 2.72 \+ 1.11 = 11.22

softmax = [7.39/11.22, 2.72/11.22, 1.11/11.22]
        = [0.659, 0.242, 0.099]  \# Sums to 1.0 ✓
```

**Key Properties:**

1. **Output is a probability distribution:**
   - All values in range (0, 1)
   - Sum equals exactly 1.0
   - Can interpret as class probabilities
2. **Preserves order:**
   - Larger input --> larger output
   - argmax(logits) = argmax(softmax(logits))
3. **Differentiable:**
   - Smooth function with well-defined gradients
   - Essential for backpropagation
4. **Emphasizes differences:**
   - Exponential amplifies gaps between values
   - Larger logits get disproportionately more probability

**Why It's Useful:**

**1. Multi-class Classification:**

python

```
logits = model(x)  \# [batch_size, num_classes]
probs = torch.softmax(logits, dim=-1)
predicted_class = torch.argmax(probs, dim=-1)
```

**2. Attention Mechanisms:** Converts attention scores to attention weights:

python

```
scores = Q @ K.T / sqrt(d_k)  \# Raw similarity scores
attention_weights = softmax(scores, dim=-1)  \# Normalized weights
output = attention_weights @ V  \# Weighted combination
```

**3. Probabilistic Interpretation:** Enables sampling, confidence estimation, and Bayesian approaches:

python

```
probs = softmax(logits)
sampled_token = torch.multinomial(probs, num_samples=1)
confidence = probs.max()  \# Confidence in prediction
```

**4. Works with Cross-Entropy Loss:** Natural pairing for classification:

python

```
\# Numerically stable combined operation
loss = CrossEntropyLoss()(logits, targets)
\# Internally computes: -log(softmax(logits)[target])
```

**Softmax vs Other Functions:**

```
| Function | Output Range | Sums to 1? | Use Case |
| --- | --- | --- | --- |
| Softmax | (0, 1) | Yes | Multi-class (mutually exclusive) |
| Sigmoid | (0, 1) | No | Binary or multi-label |
| ReLU | [0, ∞) | No | Hidden layer activations |
```

**Temperature Scaling:** Can adjust "sharpness" of distribution:

python

```
\# Temperature τ controls peakiness
softmax(x/τ, dim=-1)

\# τ < 1: Sharper (more confident)
softmax([2, 1, 0]/0.5) = [0.84, 0.14, 0.02]

\# τ > 1: Smoother (more uniform)  
softmax([2, 1, 0]/2.0) = [0.51, 0.31, 0.18]
```

**Numerical Stability:** Subtract max before exponentiating to prevent overflow:

python

```
\# Unstable
softmax(x) = exp(x) / sum(exp(x))

\# Stable (mathematically equivalent)
x_shifted = x - max(x)
softmax(x) = exp(x_shifted) / sum(exp(x_shifted))
```

**Key Takeaway:** Softmax converts raw scores into valid probability distributions, making it essential for classification, attention mechanisms, and any scenario requiring normalized, interpretable outputs that sum to 1.

---

<!-- note_key: c05a8a65a477 -->
BF: Explain dropout!
BB: **Definition:** Dropout is a regularization technique that randomly "drops" (sets to zero) a fraction of neurons during training to prevent overfitting.

**
**

**How It Works:**

```
\# During training (p=0.5 dropout rate)
x = [1.0, 2.0, 3.0, 4.0]
mask = [1, 0, 1, 0]  \# Random binary mask
output = x \* mask = [1.0, 0.0, 3.0, 0.0]

\# Scale remaining values to maintain expected sum
output = output / (1 - p) = [2.0, 0.0, 6.0, 0.0]
```

**PyTorch Implementation:**

python

```
dropout = nn.Dropout(p=0.5)  \# Drop 50% of neurons

\# Training mode
model.train()
x = torch.tensor([1.0, 2.0, 3.0, 4.0])
output = dropout(x)  \# Some neurons zeroed out randomly

\# Evaluation mode  
model.eval()
output = dropout(x)  \# No dropout applied (returns x unchanged)
```

**Why It Works:**

**1. Prevents Co-adaptation:**

- Forces neurons to not rely on specific other neurons
- Each neuron must learn robust features independently
- Network can't memorize training data through complex co-dependencies

**2. Ensemble Effect:**

- Each forward pass uses a different "sub-network"
- Training dropout = training exponentially many thinned networks
- At test time, using all neurons ≈ averaging ensemble predictions

**3. Noise Injection:**

- Acts as data augmentation at the neural level
- Makes model more robust to perturbations

**Inverted Dropout (Standard in PyTorch):**

**Problem:** If we drop 50% of neurons, output magnitude halves.

**Solution:** Scale activations during training by 1/(1-p):

python

```
\# Training: drop AND scale up
if training:
    mask = (torch.rand_like(x) > p).float()
    output = x \* mask / (1 - p)  \# Scale to maintain expectation
else:
    output = x  \# No change at test time
```

**Benefits:**

- Test-time behavior is simpler (no scaling needed)
- Expected value of activations stays constant

**Typical Usage Patterns:**

python

```
class MyModel(nn.Module):
    def __init__(self, dropout_rate=0.5):
        super().__init__()
        self.fc1 = nn.Linear(784, 256)
        self.dropout1 = nn.Dropout(dropout_rate)
        self.fc2 = nn.Linear(256, 128)
        self.dropout2 = nn.Dropout(dropout_rate)
        self.fc3 = nn.Linear(128, 10)
  
    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = self.dropout1(x)  \# Apply after activation
        x = F.relu(self.fc2(x))
        x = self.dropout2(x)
        x = self.fc3(x)  \# Usually no dropout on output layer
        return x
```

**Where to Apply Dropout:**

✅ **Use dropout on:**

- Fully connected layers (common: p=0.5)
- After activation functions
- Between transformer layers
- Recurrent connections in RNNs

❌ **Avoid dropout on:**

- Convolutional layers (less effective; use spatial dropout instead)
- Output layer
- Batch normalization layers (can conflict)

**Dropout Rates:**

- **Fully connected:** p = 0.5 (common default)
- **Input layer:** p = 0.2 (lower to preserve information)
- **Convolutional:** p = 0.1-0.3 (if used at all)
- **Large models:** Higher rates (up to 0.7-0.8)

**Training vs. Evaluation:**

python

```
\# Training
model.train()
for data, labels in train_loader:
    output = model(data)  \# Dropout active, random masking
    loss = criterion(output, labels)
  
\# Evaluation
model.eval()
with torch.no_grad():
    output = model(test_data)  \# Dropout disabled, all neurons used
```

**Variants:**

- **Spatial Dropout:** Drops entire feature maps in CNNs
- **DropConnect:** Drops connections (weights) instead of neurons
- **Variational Dropout:** Uses same mask across time steps in RNNs
- **DropBlock:** Drops contiguous regions in CNNs

**Effect on Training:**

```
Without dropout: Model overfits, memorizes training data
Training acc: 99% | Test acc: 75%

With dropout (p=0.5): Better generalization
Training acc: 92% | Test acc: 88%
```

**Key Takeaway:** Dropout randomly disables neurons during training to prevent overfitting by forcing the network to learn redundant, robust representations. It's turned off during evaluation, where all neurons contribute to predictions.

---

<!-- note_key: 236bb8d6001f -->
BF: Explain LayerNorm!
BB: **Definition:** Layer Normalization normalizes inputs across the feature dimension for each individual sample, rather than across the batch dimension.

**How it works:**

- For each sample independently, compute mean (μ) and variance (σ²) across all features
- Normalize: `x_norm = (x - μ) / √(σ² + ε)`
- Apply learnable affine transformation: `y = γ * x_norm + β`
   - γ (scale) and β (shift) are learned parameters

**Key characteristics:**

- **Independent of batch size** - computes statistics per sample
- **Same normalization at train & test time** - no moving averages needed
- **Works well with sequential data** and variable-length inputs

**Comparison to BatchNorm:**

- BatchNorm: normalizes across batch dimension (N samples, same feature)
- LayerNorm: normalizes across feature dimension (1 sample, all features)

**Common use cases:**

- Transformers and attention mechanisms
- RNNs and sequence models
- Any architecture where batch statistics are problematic (small batches, online learning)

**Formula:**

```
μ = (1/H) Σ x_i
σ² = (1/H) Σ (x_i - μ)²
y = γ ⊙ [(x - μ) / √(σ² + ε)] + β
```

where H is the feature dimension size

**
**

**PyTorch Implementation:**

```
import torch
import torch.nn as nn

class LayerNorm(nn.Module):
    def __init__(self, dim, eps=1e-5):
        super().__init__()
        self.eps = eps
        self.gamma = nn.Parameter(torch.ones(dim))
        self.beta = nn.Parameter(torch.zeros(dim))
  
    def forward(self, x):
        \# x shape: (batch, ..., dim)
        mean = x.mean(dim=-1, keepdim=True)
        var = x.var(dim=-1, keepdim=True, unbiased=False)
        x_norm = (x - mean) / torch.sqrt(var \+ self.eps)
        return self.gamma \* x_norm \+ self.beta

\# Usage
ln = LayerNorm(dim=512)
x = torch.randn(32, 10, 512)  \# (batch, seq_len, features)
out = ln(x)  \# normalized across last dimension
```

---

<!-- note_key: 4ababc38f49d -->
BF: What is Batch Normalization and how does it work?
BB: **Definition:** Batch Normalization normalizes inputs across the batch dimension, computing statistics over all samples in a batch for each feature independently.

**How it works:**

- For each feature, compute mean (μ) and variance (σ²) across the batch
- Normalize: `x_norm = (x - μ) / √(σ² + ε)`
- Apply learnable affine transformation: `y = γ * x_norm + β`
   - γ (scale) and β (shift) are learned per feature

**Key characteristics:**

- **Dependent on batch size** - needs reasonably large batches (typically >16)
- **Different behavior at train vs test** - uses moving averages at inference
- **Reduces internal covariate shift** and acts as regularization
- **Allows higher learning rates** and faster convergence

**Comparison to LayerNorm:**

- BatchNorm: normalizes across batch dimension (N samples, same feature)
- LayerNorm: normalizes across feature dimension (1 sample, all features)

**Common use cases:**

- CNNs (Convolutional Neural Networks)
- Feedforward networks with large batch sizes
- Computer vision tasks

**Training vs Inference:**

- **Training**: uses batch statistics (μ_batch, σ²_batch)
- **Inference**: uses running averages (μ_running, σ²_running) computed during training

**Formula:**

```
μ_B = (1/m) Σ x_i          # batch mean
σ²_B = (1/m) Σ (x_i - μ_B)²  # batch variance
x_norm = (x - μ_B) / √(σ²_B + ε)
y = γ * x_norm + β
```

where m is the batch size

**PyTorch Implementation:**

python

```
import torch
import torch.nn as nn

class BatchNorm1d(nn.Module):
    def __init__(self, num_features, eps=1e-5, momentum=0.1):
        super().__init__()
        self.eps = eps
        self.momentum = momentum
  
        \# Learnable parameters
        self.gamma = nn.Parameter(torch.ones(num_features))
        self.beta = nn.Parameter(torch.zeros(num_features))
  
        \# Running statistics (not learned)
        self.register_buffer('running_mean', torch.zeros(num_features))
        self.register_buffer('running_var', torch.ones(num_features))
  
    def forward(self, x):
        \# x shape: (batch, num_features) or (batch, num_features, length)
  
        if self.training:
            \# Compute batch statistics
            dims = [0] \+ list(range(2, x.ndim))  \# all except feature dim
            mean = x.mean(dim=dims)
            var = x.var(dim=dims, unbiased=False)
  
            \# Update running statistics
            self.running_mean = (1 - self.momentum) \* self.running_mean \+ \
                                self.momentum \* mean.detach()
            self.running_var = (1 - self.momentum) \* self.running_var \+ \
                               self.momentum \* var.detach()
        else:
            \# Use running statistics at inference
            mean = self.running_mean
            var = self.running_var
  
        \# Normalize and apply affine transform
        x_norm = (x - mean.view(1, -1, \*([1]\*(x.ndim-2)))) / \
                 torch.sqrt(var.view(1, -1, \*([1]\*(x.ndim-2))) \+ self.eps)
        return self.gamma.view(1, -1, \*([1]\*(x.ndim-2))) \* x_norm \+ \
               self.beta.view(1, -1, \*([1]\*(x.ndim-2)))

\# Usage
bn = BatchNorm1d(num_features=64)
x = torch.randn(32, 64, 28, 28)  \# (batch, channels, height, width)
out = bn(x)  \# normalized across batch dimension
```

---

<!-- note_key: 7e2ea5e82191 -->
BF: What is GELU and how does it differ from other activation functions?
BB: **Definition:** GELU (Gaussian Error Linear Unit) is a smooth, non-monotonic activation function that weights inputs by their value rather than gating them by their sign like ReLU.

**Mathematical Formula:**

```
GELU(x) = x · Φ(x)
```

where Φ(x) is the cumulative distribution function (CDF) of the standard Gaussian distribution.

**Alternative formulations:**

- **Exact**: `GELU(x) = x · Φ(x) = x · (1/2)[1 + erf(x/√2)]`
- **Approximation**: `GELU(x) ≈ 0.5x(1 + tanh[√(2/π)(x + 0.044715x³)])`
- **Sigmoid approximation**: `GELU(x) ≈ x · σ(1.702x)`

**Key characteristics:**

- **Smooth and differentiable** everywhere (unlike ReLU)
- **Non-monotonic** - has a small negative region for negative inputs
- **Stochastic regularizer interpretation** - probabilistically drops inputs based on their value
- **State-of-the-art in NLP** - used in BERT, GPT, and most modern transformers

**Comparison to other activations:**

- **ReLU**: `max(0, x)` - hard cutoff at zero
- **ELU**: smooth for negatives but exponential
- **GELU**: smoother than ReLU, weights inputs by their magnitude

**Why it works:**

- Combines properties of dropout, zoneout, and ReLU
- Allows small negative values (unlike ReLU's hard zero)
- Better gradient flow than ReLU for negative inputs

**Common use cases:**

- Transformer models (BERT, GPT, T5)
- Modern NLP architectures
- Vision transformers (ViT)

**PyTorch Implementation:**

python

```
import torch
import torch.nn as nn
import math

\# Method 1: Built-in PyTorch
gelu = nn.GELU()

\# Method 2: Exact implementation
class GELUExact(nn.Module):
    def forward(self, x):
        return 0.5 \* x \* (1 \+ torch.erf(x / math.sqrt(2)))

\# Method 3: Fast approximation (used in original BERT)
class GELUApprox(nn.Module):
    def forward(self, x):
        return 0.5 \* x \* (1 \+ torch.tanh(
            math.sqrt(2 / math.pi) \* (x \+ 0.044715 \* x\*\*3)
        ))

\# Method 4: Sigmoid approximation
class GELUSigmoid(nn.Module):
    def forward(self, x):
        return x \* torch.sigmoid(1.702 \* x)

\# Usage
x = torch.randn(32, 512)
out = gelu(x)

\# Comparison
import matplotlib.pyplot as plt
x_range = torch.linspace(-3, 3, 1000)
relu = torch.relu(x_range)
gelu_exact = 0.5 \* x_range \* (1 \+ torch.erf(x_range / math.sqrt(2)))

\# GELU is smooth and allows small negative values
```

**Visual intuition:**

- For large positive x: GELU(x) ≈ x (like ReLU)
- For large negative x: GELU(x) ≈ 0 (like ReLU)
- For x near 0: smooth transition, small negative values pass through
- Minimum around x ≈ -0.17 with value ≈ -0.17

---

<!-- note_key: 16fdde4ecfd9 -->
BF: Describe the detailed architecture of a GPT-2 transformer block, including components, flow, and approximate parameter counts.
BB: **Overall Architecture:** GPT-2 consists of stacked transformer decoder blocks with causal (autoregressive) masking. Each block follows a consistent pattern with residual connections.

**Single Transformer Block Flow:**

```
Input (d_model)
    ↓
LayerNorm
    ↓
Multi-Head Causal Self-Attention
    ↓
Residual Connection (+)
    ↓
LayerNorm
    ↓
Feed-Forward Network (MLP)
    ↓
Residual Connection (+)
    ↓
Output (d_model)
```

**Detailed Components:**

**1. Layer Normalization (pre-norm)**
- Applied before attention and FFN (not after, as in original Transformer)
- Parameters: 2 × d_model (γ and β)

**2. Multi-Head Causal Self-Attention**
- **Q, K, V projections**: 3 × (d_model × d_model)
- **Number of heads**: n_head (e.g., 12 for GPT-2 small)
- **Head dimension**: d_head = d_model / n_head
- **Causal mask**: prevents attending to future positions
- **Output projection**: d_model × d_model
- **Total params**: 4 × d_model²

**3. Feed-Forward Network (MLP)**
- **First linear layer**: d_model --> 4 × d_model
- **Activation**: GELU
- **Second linear layer**: 4 × d_model --> d_model
- **Total params**: 2 × (d_model × 4 × d_model) = 8 × d_model²

**4. Residual Connections**
- After attention and after FFN
- No additional parameters

**Parameter Count per Block:**
```
LayerNorm 1:        2 × d_model
Attention:          4 × d_model²
LayerNorm 2:        2 × d_model
FFN:                8 × d_model²
─────────────────────────────────
Total per block:    12 × d_model² + 4 × d_model
                    ≈ 12 × d_model² (dominant term)
```

**GPT-2 Model Variants:**

| Model | n_layers | d_model | n_heads | Params |
|-------|----------|---------|---------|---------|
| **GPT-2 Small** | 12 | 768 | 12 | 117M |
| **GPT-2 Medium** | 24 | 1024 | 16 | 345M |
| **GPT-2 Large** | 36 | 1280 | 20 | 762M |
| **GPT-2 XL** | 48 | 1600 | 25 | 1.5B |

**Complete Model Architecture:**
```
Token Embeddings (vocab_size × d_model)
    +
Position Embeddings (max_seq_len × d_model)
    ↓
[Transformer Block] × n_layers
    ↓
LayerNorm (final)
    ↓
Output Linear (d_model × vocab_size)
    ↓
Softmax
```

![figure](<media/4-15.png>)**
**

**
**

**Key Design Choices:**

- **Pre-norm** (LayerNorm before sublayers) instead of post-norm
- **Causal masking** for autoregressive generation
- **No encoder-decoder attention** (decoder-only)
- **GELU activation** instead of ReLU
- **Learned positional embeddings** (not sinusoidal)

```
\# GPT-2 Small
model = GPT2(vocab_size=50257, d_model=768, n_heads=12, n_layers=12)
print(f"Total parameters: {sum(p.numel() for p in model.parameters()):,}")
\# Output: ~117M parameters
```

**Memory Note:** For GPT-2 Small with context length 1024:

- Attention memory: O(n_heads × seq_len²) ≈ 12 × 1024² ≈ 12M elements
- Dominates memory at long sequences

---

<!-- note_key: e396c58ec9b3 -->
BF: Count the number of parameters in GPT2!
BB: ![figure](<media/4-15.png>)

TODO First time you practice

---

<!-- note_key: e87a6cc337f6 -->
BF: Why is working with logarithms of probabilities preferred over probabilities?
BB: **Key Reasons:**

**1. Numerical Stability (Primary Reason)**

- **Underflow prevention**: Probabilities can be extremely small (e.g., 10⁻³⁰⁰)
- Multiplying many small probabilities --> underflow --> rounds to 0
- Log space: `log(10⁻³⁰⁰) = -300` (perfectly representable)
- Example: P(sequence) = 0.1 × 0.1 × ... (1000 times) ≈ 10⁻¹⁰⁰⁰ --> underflow!
- Log space: `log P = -1 + -1 + ... = -1000` ✓

**2. Computational Efficiency**

- **Products --> Sums**: `log(a × b) = log(a) + log(b)`
- Addition is faster and more stable than multiplication
- Example: `P(x₁, x₂, ..., xₙ) = ∏ P(xᵢ)` becomes `log P = Σ log P(xᵢ)`

**3. Better Optimization Properties**

- **Convexity**: Negative log-likelihood is often convex (easier to optimize)
- **Gradients**: More numerically stable gradients
- Avoids vanishing gradients from very small probabilities

**4. Dynamic Range**

- Probabilities: [0, 1] (limited range)
- Log probabilities: (-∞, 0] (unlimited negative range)
- Better representation of very unlikely events

**5. Loss Function Formulation**

- Cross-entropy loss naturally uses log probabilities
- `Loss = -Σ yᵢ log(ŷᵢ)` (negative log-likelihood)
- Directly interpretable as "surprise" or information content

**Common Applications:**

**Language Models:**

```
P(sentence) = P(w₁) × P(w₂|w₁) × P(w₃|w₁,w₂) × ...
log P(sentence) = log P(w₁) + log P(w₂|w₁) + log P(w₃|w₁,w₂) + ...
```

**Hidden Markov Models (HMMs):**
```
Forward algorithm: α(t) = P(observations up to t)
In log space: log α(t) = log Σ exp(log α(t-1) + log transition + log emission)
```

**Softmax with Temperature:**
```
Standard: P(i) = exp(zᵢ) / Σ exp(zⱼ)
Log space: log P(i) = zᵢ - log Σ exp(zⱼ)  # LogSumExp trick
```

**Critical Numerical Example:**

python

```
\# Bad: Direct probability multiplication
p = 0.01
result = p \*\* 1000  \# Underflows to 0.0

\# Good: Log probability addition
log_p = math.log(0.01)  \# -4.605
log_result = 1000 \* log_p  \# -4605.17
\# Can convert back if needed: exp(-4605.17)
```

\*\*The LogSumExp Trick:\*\*
When you need to convert back: `log(Σ exp(xᵢ))` can also underflow/overflow.

\*\*Solution:\*\*
```
LSE(x) = max(x) \+ log(Σ exp(xᵢ - max(x)))
```

This keeps exponentials in a stable range!

**PyTorch Implementation Examples:**

python

```
import torch
import torch.nn.functional as F

\# Example 1: Cross-entropy with log probabilities
logits = torch.randn(32, 10)  \# (batch, classes)
targets = torch.randint(0, 10, (32,))

\# Method 1: Using log_softmax (numerically stable)
log_probs = F.log_softmax(logits, dim=1)
loss = F.nll_loss(log_probs, targets)

\# Method 2: Direct cross_entropy (does log_softmax internally)
loss = F.cross_entropy(logits, targets)

\# Example 2: Computing sequence probability
def sequence_log_prob(token_log_probs):
    """
    token_log_probs: (seq_len,) log probabilities
    Returns: log P(sequence)
    """
    return token_log_probs.sum()  \# Product becomes sum!

\# Example 3: LogSumExp trick
def log_sum_exp(x, dim=-1):
    """Numerically stable log(sum(exp(x)))"""
    max_x = x.max(dim=dim, keepdim=True)[0]
    return max_x \+ (x - max_x).exp().sum(dim=dim, keepdim=True).log()

\# Usage
log_probs = torch.tensor([-1000, -1001, -999])  \# Very negative
\# torch.logsumexp(log_probs)  \# PyTorch built-in
result = log_sum_exp(log_probs)  \# -998.59 (stable!)

\# Example 4: Language model perplexity
def compute_perplexity(log_probs):
    """
    log_probs: log probabilities for each token
    Perplexity = exp(-mean(log P))
    """
    avg_log_prob = log_probs.mean()
    perplexity = torch.exp(-avg_log_prob)
    return perplexity

\# Example 5: Comparing sequences
log_prob_seq1 = -45.2  \# log P(sequence 1)
log_prob_seq2 = -48.7  \# log P(sequence 2)

\# Sequence 1 is more likely (less negative)
if log_prob_seq1 > log_prob_seq2:
    print("Sequence 1 is more likely")

\# Example 6: Safe probability conversion
log_prob = -1000  \# Very small probability

\# Bad: exp(-1000) = 0.0 (underflow)
\# Good: Keep in log space or use for comparison only

\# If you MUST convert (e.g., for display):
if log_prob < -700:  \# threshold for float64
    print("Probability effectively zero")
else:
    prob = torch.exp(torch.tensor(log_prob))
```

**Interview Key Points:**

- **Numerical stability** is the \#1 reason
- Products become sums (computationally cheaper)
- Standard in ML: cross-entropy, NLL, language models
- Always mention **LogSumExp trick** for bonus points
- Know that `log_softmax` is more stable than `log(softmax)`

---

<!-- note_key: afa10aa33c96 -->
BF: What is perplexity and how is it used to evaluate language models?
BB: **Definition:** Perplexity (PPL) is a measurement of how well a probability model predicts a sample. For language models, it measures how "surprised" the model is by the test data. **Lower perplexity = better model.**

**Mathematical Formula:**

```
Perplexity = exp(-1/N Σ log P(wᵢ | context))
           = exp(H(P))  where H(P) is cross-entropy
           = 2^(H(P))   when using log₂
```

For a sequence of N tokens:
```
PPL = exp(-1/N log P(w₁, w₂, ..., wₙ))
    = [P(w₁, w₂, ..., wₙ)]^(-1/N)
    = ⁿ√(1/P(sequence))
```

**Intuitive Interpretation:**
- **"On average, the model is as confused as if it had to choose uniformly from PPL options"**
- PPL = 10 --> model is as uncertain as guessing from 10 equally likely words
- PPL = 50 --> model is as uncertain as guessing from 50 equally likely words
- PPL = 1 --> perfect prediction (model assigns probability 1 to correct tokens)

**Key Properties:**

1. **Inverse relationship with probability**
   - High probability --> Low perplexity (good)
   - Low probability --> High perplexity (bad)

2. **Normalized by sequence length**
   - Allows fair comparison across different text lengths
   - Geometric mean of inverse probabilities

3. **Exponential of cross-entropy**
   - Cross-entropy in nats --> `PPL = exp(H)`
   - Cross-entropy in bits --> `PPL = 2^H`

4. **Range**: [1, ∞)
   - Best possible: 1 (perfect prediction)
   - Random baseline: vocab_size (uniform distribution)

**Comparison Across Models:**

| Model | Perplexity | Interpretation |
|-------|------------|----------------|
| Perfect | 1.0 | Always predicts correctly |
| GPT-2 | ~20-30 | Good language model |
| Weak baseline | ~100-200 | Poor predictions |
| Random | ~50,000 | Uniform over vocabulary |

**Example Calculation:**

Given sequence: "the cat sat"
```
P(the) = 0.1
P(cat | the) = 0.2
P(sat | the cat) = 0.5

P(sequence) = 0.1 × 0.2 × 0.5 = 0.01
log P(sequence) = log(0.01) = -4.605
PPL = exp(-(-4.605)/3) = exp(1.535) = 4.64
```
**Interpretation**: The model is as confused as if choosing uniformly from ~4.6 words.

**Why Use Perplexity Instead of Likelihood?**
- **More interpretable**: "confused among N words" vs abstract probability
- **Length-normalized**: Fair comparison across sequences
- **Standard metric**: Industry benchmark for language models
- **Scale-invariant**: Works across different vocabulary sizes

**Relationship to Other Metrics:**
```
Cross-Entropy Loss = -1/N Σ log P(wᵢ)
Perplexity = exp(Cross-Entropy Loss)
Bits-per-character = log₂(Perplexity) / chars_per_token
```

**Limitations:**

- ⚠️ **Doesn't measure generation quality** directly
- ⚠️ **Vocabulary-dependent**: Can't compare models with different vocabularies
- ⚠️ **Doesn't capture semantic understanding**
- ⚠️ **Lower PPL =/= better for downstream tasks** (necessarily)

**Common Use Cases:**

- Language model evaluation (GPT, BERT MLM)
- Comparing different architectures
- Hyperparameter tuning
- Tracking training progress
- Speech recognition confidence

**PyTorch Implementation:**

python

```
import torch
import torch.nn.functional as F

\# Method 1: From log probabilities
def perplexity_from_log_probs(log_probs):
    """
    log_probs: (seq_len,) log probabilities of correct tokens
    """
    avg_neg_log_prob = -log_probs.mean()
    ppl = torch.exp(avg_neg_log_prob)
    return ppl

\# Method 2: From cross-entropy loss
def perplexity_from_loss(cross_entropy_loss):
    """
    cross_entropy_loss: scalar, average negative log likelihood
    """
    return torch.exp(cross_entropy_loss)

\# Method 3: Complete example with language model
def compute_perplexity(model, tokens, vocab_size):
    """
    model: language model
    tokens: (batch, seq_len) token IDs
    """
    model.eval()
    with torch.no_grad():
        \# Get logits
        logits = model(tokens[:, :-1])  \# (batch, seq_len-1, vocab)
        targets = tokens[:, 1:]  \# (batch, seq_len-1)
  
        \# Compute cross-entropy loss
        loss = F.cross_entropy(
            logits.reshape(-1, vocab_size),
            targets.reshape(-1),
            reduction='mean'
        )
  
        \# Perplexity is exp of loss
        ppl = torch.exp(loss)
  
    return ppl.item()

\# Example usage
tokens = torch.randint(0, 50000, (32, 128))  \# (batch, seq_len)
\# ppl = compute_perplexity(model, tokens, vocab_size=50000)
\# print(f"Perplexity: {ppl:.2f}")

\# Method 4: Manual calculation step-by-step
def detailed_perplexity(logits, targets):
    """
    logits: (seq_len, vocab_size) model predictions
    targets: (seq_len,) ground truth token IDs
    """
    \# Get log probabilities
    log_probs = F.log_softmax(logits, dim=-1)
  
    \# Extract log probs of correct tokens
    correct_log_probs = log_probs[
        torch.arange(len(targets)), targets
    ]
  
    \# Average negative log likelihood
    avg_nll = -correct_log_probs.mean()
  
    \# Perplexity
    ppl = torch.exp(avg_nll)
  
    print(f"Average NLL: {avg_nll:.4f}")
    print(f"Perplexity: {ppl:.4f}")
  
    return ppl

\# Method 5: Per-token perplexity (for analysis)
def per_token_perplexity(log_probs):
    """
    Returns perplexity for each token position
    log_probs: (seq_len,) log P(correct token)
    """
    return torch.exp(-log_probs)

\# Example: Track perplexity during training
class PerplexityTracker:
    def __init__(self):
        self.total_loss = 0.0
        self.total_tokens = 0
  
    def update(self, loss, num_tokens):
        """loss: batch loss, num_tokens: number of tokens in batch"""
        self.total_loss \+= loss.item() \* num_tokens
        self.total_tokens \+= num_tokens
  
    def compute(self):
        """Returns average perplexity over all batches"""
        avg_loss = self.total_loss / self.total_tokens
        return torch.exp(torch.tensor(avg_loss)).item()
  
    def reset(self):
        self.total_loss = 0.0
        self.total_tokens = 0

\# Usage in training loop
tracker = PerplexityTracker()

\# for batch in dataloader:
\#     logits = model(batch['input_ids'])
\#     loss = F.cross_entropy(logits, batch['targets'])
\#  
\#     num_tokens = batch['targets'].numel()
\#     tracker.update(loss, num_tokens)
\#  
\#     \# ... backward pass ...
\#
\# epoch_ppl = tracker.compute()
\# print(f"Epoch Perplexity: {epoch_ppl:.2f}")
\# tracker.reset()
```

**Real-World Examples:**

**GPT-2 Perplexities (WebText test set):**

- GPT-2 Small (117M): ~29.4
- GPT-2 Medium (345M): ~26.4
- GPT-2 Large (762M): ~24.1
- GPT-2 XL (1.5B): ~22.8

**Common Benchmark Values:**

- Penn Treebank: 20-60 (modern models)
- WikiText-103: 15-30 (modern models)
- LAMBADA: measured in accuracy, not PPL

**Interview Key Points:** 

✅ **Lower is better** (measures surprise/uncertainty)

✅ **Exponential of cross-entropy loss** 

✅ **"On average, confused among PPL choices"** 

✅ **Length-normalized** comparison

✅ **Limitations**: doesn't directly measure quality or understanding

✅ **Cannot compare** across different vocabularies

---

<!-- note_key: 22bc4c1879a6 -->
BF: What is cross-entropy loss and why is it used in classification tasks?
BB: **Definition:** Cross-entropy measures the dissimilarity between predicted probability distribution and true distribution. For classification, it's the negative log-probability of the correct class.

**
**

**Formulas:**

**Binary (2 classes):**

```
L = -[y log(ŷ) + (1-y) log(1-ŷ)]

Multi-class (categorical):
L = -log(ŷc)  where c is the correct class
  = -Σᵢ yᵢ log(ŷᵢ)  (equivalent with one-hot y)
```

```
Batch average:
```

```
Loss = (1/N) Σₙ -log(ŷₙ,cₙ)
```

**Key Properties:**

1. **Output range**: [0, ∞)
   - Perfect prediction (ŷ_c = 1): Loss = 0
   - Wrong prediction (ŷ_c --> 0): Loss --> ∞
2. **Gradient**: `∂L/∂logits = ŷ - y`
   - Linear in error, doesn't saturate
   - Much better than MSE for classification
3. **Probabilistic interpretation**: Maximizes likelihood of correct class
4. **Relationship to perplexity**: `Perplexity = exp(Cross-Entropy)`

**Why Cross-Entropy Over MSE?**

With sigmoid/softmax outputs:

- **CE gradient**: `ŷ - y` (clean, strong signal)
- **MSE gradient**: `2(ŷ - y)·ŷ·(1-ŷ)` (vanishes when very wrong!)

**PyTorch Implementation:**

python

```
import torch
import torch.nn.functional as F

\# ============================================
\# MULTI-CLASS CLASSIFICATION
\# ============================================

\# Input: raw logits (no softmax needed!)
logits = torch.randn(32, 10)  \# (batch_size, num_classes)
targets = torch.randint(0, 10, (32,))  \# class indices [0-9]

\# Standard cross-entropy (RECOMMENDED)
loss = F.cross_entropy(logits, targets)

\# With class weights (for imbalanced data)
weights = torch.tensor([1.0, 2.0, 1.0, ...])  \# higher weight = more important
loss = F.cross_entropy(logits, targets, weight=weights)

\# With label smoothing (prevents overconfidence)
loss = F.cross_entropy(logits, targets, label_smoothing=0.1)

\# Per-sample loss (no averaging)
losses = F.cross_entropy(logits, targets, reduction='none')  \# shape: (32,)

\# ============================================
\# BINARY CLASSIFICATION
\# ============================================

\# From logits (RECOMMENDED - more stable)
logits = torch.randn(32)  \# raw outputs
targets = torch.randint(0, 2, (32,)).float()  \# 0 or 1
loss = F.binary_cross_entropy_with_logits(logits, targets)

\# From probabilities (if you already have sigmoid output)
probs = torch.sigmoid(logits)  \# or from model with sigmoid
loss = F.binary_cross_entropy(probs, targets)

\# ============================================
\# MANUAL IMPLEMENTATION (for understanding)
\# ============================================

def cross_entropy_manual(logits, targets):
    """
    logits: (N, C) raw outputs
    targets: (N,) class indices
    """
    \# Stable log-softmax
    log_probs = F.log_softmax(logits, dim=1)
  
    \# Extract log prob of correct class for each sample
    N = logits.shape[0]
    correct_log_probs = log_probs[range(N), targets]
  
    \# Negative log likelihood
    return -correct_log_probs.mean()

\# ============================================
\# COMMON PATTERNS
\# ============================================

\# Pattern 1: Standard training loop
class Model(torch.nn.Module):
    def forward(self, x):
        return self.layers(x)  \# Returns LOGITS (no softmax!)

model = Model()
optimizer = torch.optim.Adam(model.parameters())

for x_batch, y_batch in dataloader:
    logits = model(x_batch)
    loss = F.cross_entropy(logits, y_batch)
  
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

\# Pattern 2: Get predictions at inference
model.eval()
with torch.no_grad():
    logits = model(x_test)
    probs = F.softmax(logits, dim=1)  \# Convert to probabilities
    preds = logits.argmax(dim=1)      \# Get class predictions

\# Pattern 3: Multi-label classification (multiple classes per sample)
logits = torch.randn(32, 10)  \# (batch, num_labels)
targets = torch.randint(0, 2, (32, 10)).float()  \# binary per label
loss = F.binary_cross_entropy_with_logits(logits, targets)
```

**Critical Rules:**

⚠️ **NEVER apply softmax before F.cross_entropy** (it does it internally)

⚠️ **ALWAYS use logits**, not probabilities with cross_entropy

⚠️ **Targets are class indices** (not one-hot) for cross_entropy

⚠️ **Use `*_with_logits` versions** for numerical stability

**
**

**Numerical Stability:**

python

```
\# BAD (can underflow/overflow):
probs = F.softmax(logits, dim=1)
loss = -torch.log(probs[range(N), targets]).mean()

\# GOOD (numerically stable):
loss = F.cross_entropy(logits, targets)
```

The stable version computes `log(softmax(x))` directly using the LogSumExp trick internally.

---

<!-- note_key: d788135b3461 -->
BF: What are the main decoding strategies for text generation in LLMs and how do they work?
BB: **Definition:** Decoding strategies determine how to select the next token from the model's probability distribution during autoregressive text generation.

**Main Strategies:**

**1. Greedy Decoding**

- Always pick the highest probability token
- Deterministic, fast, but repetitive and boring
- `next_token = argmax(probs)`

**2. Beam Search**

- Maintain top-k sequences (beams) at each step
- Explore multiple paths, keep best overall sequences
- Better quality than greedy, but still deterministic

**3. Temperature Sampling**

- Scale logits before softmax: `logits / T`
- T < 1: sharper (more confident), T > 1: flatter (more random)
- T --> 0: approaches greedy, T --> ∞: uniform random

**4. Top-k Sampling**

- Sample only from top-k highest probability tokens
- Filters out unlikely tokens
- Typically k = 40-50

**5. Top-p (Nucleus) Sampling**

- Sample from smallest set of tokens with cumulative probability ≥ p
- Adaptive vocabulary size based on confidence
- Typically p = 0.9-0.95

**6. Combined Strategies**

- Temperature \+ Top-p (most common in practice)
- Temperature \+ Top-k
- Provides controlled randomness

---

<!-- note_key: 643dcb69fc09 -->
BF: What is the KV cache in LLM inference and why is it important? (needs update)
BB: **Definition:** KV cache stores the computed Key and Value matrices from previous tokens during autoregressive generation, avoiding redundant computation of attention for already-processed tokens.

**
**

**The Problem Without KV Cache:**

In autoregressive generation, we generate one token at a time:

```
Step 1: "The" --> compute attention for position 0
Step 2: "The cat" --> recompute attention for positions 0,1
Step 3: "The cat sat" --> recompute attention for positions 0,1,2
...

**Redundant computation**: For each new token, we recompute Q, K, V for ALL previous tokens!

**The Solution - KV Cache:**

Cache the K and V matrices from previous forward passes:
```
Step 1: Compute Q₀, K₀, V₀ --> Cache K₀, V₀
Step 2: Compute Q₁ only --> Reuse K₀,V₀, append K₁,V₁ to cache
Step 3: Compute Q₂ only --> Reuse K₀,K₁,V₀,V₁, append K₂,V₂
```

**Why Only K and V?**

In self-attention: `Attention(Q, K, V) = softmax(QK^T / √d)V`

- **Q (Query)**: Only needed for the NEW token
- **K (Key)**: Needed for all previous tokens (cache it!)
- **V (Value)**: Needed for all previous tokens (cache it!)

**Benefits:**

1. **Speed**: O(n) --> O(1) attention computation per token
2. **Memory trade-off**: Store KV tensors to save computation
3. **Critical for long sequences**: Makes generation practical

**Memory Requirements:**

For a single layer:
```
KV cache size = 2 × seq_len × hidden_dim × num_bytes
```

For full model with L layers:
```
Total KV cache = 2 × L × seq_len × hidden_dim × precision
```

Example (LLaMA-7B, seq_len=2048, fp16):
```
2 × 32 layers × 2048 × 4096 × 2 bytes = ~1GB per sequence
```

**Challenges:**

1. **Memory-bound**: KV cache can be massive for long contexts
2. **Batch size limitation**: KV cache scales with batch × seq_len
3. **Multi-head attention**: Each head has separate KV cache

---

<!-- note_key: cf023630946c -->
BF: What is RMSNorm and how does it differ from LayerNorm?
BB: **Definition:** RMSNorm (Root Mean Square Normalization) simplifies LayerNorm by removing mean centering and only normalizing by RMS (root mean square).

**Equations:**

**LayerNorm (for comparison):**

```
μ = (1/d) Σᵢ xᵢ
σ² = (1/d) Σᵢ (xᵢ - μ)²
y = γ ⊙ [(x - μ) / √(σ² + ε)] + β
```

**RMSNorm:**
```
RMS(x) = √[(1/d) Σᵢ xᵢ²]

y = γ ⊙ [x / RMS(x)]
  = γ ⊙ [x / √((1/d) Σᵢ xᵢ² + ε)]
```

**Alternative form:**
```
RMS(x) = √[mean(x²) + ε]
y = (x / RMS(x)) * γ
```

**Key differences from LayerNorm:**

| | LayerNorm | RMSNorm |
|---|-----------|---------|
| Mean centering | ✓ (subtracts μ) | ✗ (no centering) |
| Variance | Uses σ² | Uses RMS |
| Shift parameter (β) | ✓ Has shift | ✗ No shift |
| Scale parameter (γ) | ✓ Has scale | ✓ Has scale |

**Computational formula:**
```
1. Compute x² (element-wise square)
2. Take mean: mean(x²) along feature dimension
3. Add epsilon: mean(x²) + ε
4. Take square root: √[mean(x²) + ε]
5. Normalize: x / RMS(x)
6. Scale: result * γ
```

**PyTorch pseudo-code structure:**

python

```
\# Hint: Steps for implementation
rms = torch.sqrt(x.pow(2).mean(dim=-1, keepdim=True) \+ eps)
x_norm = x / rms
output = x_norm \* gamma
```

**Benefits:**

- Simpler than LayerNorm (no mean, no shift)
- Faster computation (~15-20% speedup)
- Similar performance in practice
- Used in: LLaMA, Gopher, T5

**Used in modern LLMs:**

- LLaMA/LLaMA-2
- Mistral
- Mixtral
- T5 (called "RMS LayerNorm")

---

<!-- note_key: 4b58c802d013 -->
BF: Explain three observations which lead to the attention mechanism!
BB: Setting: We want to build a network to process sequences efficiently for downstream applications.

- **Observation 1:** The inputs can be very large - and using fully connected networks is unfeasible (e.g. thousands of words times thousands of dimension for each one)
- **Observation 2:** Each input is of a different length, the network should share parameters across all inputs!
- **Observation 3:** Language is ambiguous, and representations should reflect relative importance within the sequence.
Self-attention fulfills all of these requirements.

---

<!-- note_key: 74c365847da7 -->
BF: Explain the roles (and shapes) of *queries*, *keys*, and *values* in self-attention operation
BB: The input into the attention layer is a \(n\) length sequence of \(d\)-dimensional input token representations:
 \[X \sim (n, d)\]We then take this input and perform three linear projections with \(W_q\), \(W_k \sim (d, d_k)\) and \(W_v \sim (d, d_v)\)
This gives the core components of the dot-product attention layer:
\[Q = X \cdot W_q \sim (n, d_k)\]\[K = X \cdot W_k \sim (n, d_k)\]\[V = X \cdot W_v \sim (n, d_v)\]Since we usually wish to keep the representation unchanged we take \(d_v = d\). Borrowing from information theory parlor:

1. Queries -> "What am I looking for?"
2. Keys -> "What do I contain?"
3. Values -> "What should we aggregate/keep?"

---

<!-- note_key: 60b6722d6e9d -->
BF: Why do newer transformer architectures (>GPT2) ommit the bias term in the QKV projections?
BB: 1. LayerNorm can learn the bias/shifting operation more efficiently
2. The biases in the projections lead to inneficient learning, some terms are constant (get erased by softmax), others are position-independent and can be more efficiently learned in other layers.
3. Empirical reasons! It's just not worth having it there
4. Architectural simplicity - Q, K, V are all meant to be views of the same semantic space. Biases would introduce unnatural shifts in representation (where only relative comparison is meant to happen)

---

<!-- note_key: 8e72c62258d4 -->
BF: Where does the \(\sqrt d_k\) term in dot-product self-attention come from?
BB: To mitigate the fact that the dot products of queries and keys grow with increasing \(d_k\). If you assume \(q\) and \(k\) to be sampled with mean of 0 and variance of 1. Then their dot product \(q \cdot k = \sum q_i k_i\) has mean of \(0\) and variance \(d_k\). This results in vanishing gradients as the softmax saturates - we therefore divide the matrix product by the standard deviation of the dot product variance to normalize.

---

<!-- note_key: c223dc0dd76b -->
BF: Explain sinusoidal positional encoding
BB: Used in the original transformer for positional information. **Sinusoidal Positional Encoding** Fixed, deterministic patterns using sine/cosine functions:

```
PE(pos, 2i) = sin(pos / 10000^(2i/d_model))
PE(pos, 2i\+1) = cos(pos / 10000^(2i/d_model))
```

```
position -> position in sequence
```

```
i -> position in vector
```

- Can extrapolate to longer sequences than seen during training
- No additional parameters to learn
- Each dimension has different frequency

---

<!-- note_key: 201a192fd6b6 -->
BF: What are learned absolute positional embeddings
BB: Used to inject positional information into transformers:
\[\text{PE}(pos) = \operatorname{embedding table}(pos)\]where embedding table is of shape (context length, \(d_{model})\). Basically a learned lookup table for each position, performs well in practice GPT2/BERT uses.

The drawback of these embeddings is that they cannot extrapolate, you can still do longer sequences but those positional embeddings would be randomly initialized.

---

<!-- note_key: 1023e3ae7cef -->
BF: Explain T5 style relative attention bias
BB: T5 style relative attention bias is a **relative positional encodings** scheme in an attention layer, where the score between query \(i\) and key \(j\), is computed as:
\[\operatorname{score}(i, j) = \frac{q_i \cdot k_j^T}{\sqrt{d_k}} + \operatorname{b}(i, j)\]Where \(\operatorname{b}(i, j)\) is a **learned** positional bias function. There are a few approaches for the bias operator.
1. Naive bucketing: A lookup table indexed by distance
2. Intelligent bucketing: Group similar distances into buckets, where we're exact for close tokens but logarithmic for further away tokens

The operator is **shared across layers (all attention layers)**, but **each head gets its own bias** this way different heads learn different patterns.

Advantages:

- It's simpler and more parameter-efficient than learned absolute positions
- It generalizes better to sequences longer than those seen during training
- It captures the intuition that relative distances often matter more than absolute positions

---

<!-- note_key: 16b1ad695837 -->
BF: Explain Rotary Positional Embeddings (RoPE)!
BB: TODO - copy from notes when first solving

---

<!-- note_key: 7f61a5d7402e -->
BF: Explain how the ALiBI positional encoding scheme works!
BB: It's the simplest relative encoding scheme, we simply penalize the attention computation via linear distance
\[q^T_mk_n - \lambda ~|m - n|\]it's computationally efficient, and can extrapolate

---

<!-- note_key: 82c3596ff45c -->
BF: How can you efficiently implement the RoPE calculation?
BB: TODO: write out once practicing

![](<media/paste-f78e787d3e18263d103c7b435589e4f8a8a8dfa3.jpg>)

---

<!-- note_key: b72a2792312c -->
BF: What are some common context extension techniques for positional encoding schemes?
BB: - **Position Interpolation (PI)** - Scales down positions so longer sequences fit within the trained range (e.g., mapping [0, 4096] to [0, 2048] if trained on 2048 tokens).

- **NTK-aware scaling** - Increases RoPE's base frequency parameter proportionally to context extension, preserving high-frequency (local) information better than uniform interpolation.
- **NTK-by-parts** - Applies different NTK scaling factors to different frequency bands in RoPE. Low frequencies get more aggressive scaling for long-range, high frequencies get less scaling to preserve local detail.
- **Dynamic NTK** - Adaptively adjusts the NTK scaling factor based on the actual sequence length at inference time, using less aggressive scaling for shorter sequences and more for longer ones.
- **YaRN (Yet another RoPE extensioN)** - Combines NTK-by-parts with attention temperature scaling and other refinements for improved extrapolation across a wide range of sequence lengths.

---

<!-- note_key: 6dc270e2bccd -->
BF: Explain YaRN!
BB: **YaRN (Yet another RoPE extensioN)** is a comprehensive method for extending RoPE to much longer contexts. It combines several techniques:

**1. NTK-by-parts interpolation** Divides RoPE's frequency dimensions into three ranges:

- **Low frequencies** (long-range): Apply NTK scaling to extend their effective range
- **High frequencies** (short-range): Apply standard interpolation since they're already well-trained
- **Middle frequencies**: Smooth transition between the two approaches

This prevents over-compressing local information while still extending global context capability.

**
**

**2. Attention temperature scaling** Adds a learnable temperature parameter to attention scores that compensates for the "entropy loss" that occurs when extending context. Longer contexts spread attention more thinly, so temperature adjustment helps maintain appropriate attention sharpness.

**
**

**3. Modified attention scaling** Adjusts the scaling factor in attention computation to account for the extended context length, preventing attention scores from becoming too diffuse.

**Why it works well:**

- NTK-by-parts preserves the distinction between local and global positional information
- Temperature scaling addresses the practical issue that attention distributions change character at longer lengths
- The combination allows extending context 32× or more (e.g., 2K --> 64K) with minimal fine-tuning

---

<!-- note_key: ea1f9f3a3095 -->
BF: What is the KV-Cache size for MHA, MQA, GQA, and MLQA?
BB: MHA: \(2\cdot l\cdot d_n \cdot m\)
MQA: \(2\cdot l\cdot d_n\)
GQA: \(2\cdot l\cdot d_n \cdot g\)
MQLA: \(d_l \cdot l\)

---

<!-- note_key: 09e4ddc7e98a -->
BF: Explain Multi-head Latent attention
BB: Equations:
![](<media/paste-9a5d00137b27b99831518b45f4ff43a863699f1a.jpg>)

A way to save KV cache, which is now only \((d_c + d_h^R)\cdot l\) large. Need to separate out rope, otherwise project to common space from which the keys and values can be recovered.
