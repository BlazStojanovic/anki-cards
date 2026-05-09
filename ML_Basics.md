# ML Basics
*48 cards · regenerated 2026-05-09 01:17 UTC from Anki via MCP · do not hand-edit*

## How does a bag-of-words model work?
*Basic · id 1758684368084*

Given a vocabulary (all possible tokens that can be represented), represent an individual string as a sparse vector counting the number of occurences (i.e. a bag-of-words).

---

## What is the key idea behind <i>word2vec</i>?
*Basic · id 1758686371283*

That we should use dense embeddings to represent words. Train on predicting word similarity (e.g. measured by appearing <i>close</i> in text). 

The representation <i>word2vec </i>chooses, is a CBOW (continuous-bag-of-words), where the representation represent each word with a vector, such that it is possible to predict a word using the sum of the vectors of its neighbors.

---

## Explain the logical sequence of developments to arrive at the transformer architecture!
*Basic · id 1759896314289*

1. Start with bag of words approaches, i.e. just word counting
2. Actually, we can use dense embeddings to encode semantic meaning of tokens and we can pre-train them (word2vec)
3. Now the problem becomes that embeddings are completely static (e.g. *bank* can mean financial or river bank but this is not reflected in the embeddings)
4. Key insight from 3. is that we should really be modelling sequences -> use *sequence models.* [[RNN]]s!
5. Another new idea RNNs are [[Auto-regressive models]] - I.e. to process $n$-th input it needs to first process the first $n-1$ inputs
6. Problem with RNNs is the context embedding, the entire sequence is represented as a *single* dense embedding
7. Solution -> **[[attention]]** focus the model on the parts of the input which are highly relevant to one another and amplify their signal. 
8. You can use RNN encoder + Attention mechanism (i.e. attention decoder), which is very powerful!
9. But, the sequential part is problematic from a computational perspective, it prevents *parallelization*
10. What if we get rid of sequence modelling --> "Attention is all you need!" and the Transformer. The transformer can be trained in parallel

---

## How many parameters to GPT1, GPT2, and GPT3 have?
*Basic · id 1759898633460*

GPT1: 117M
GPT2: 1.5B
GPT3. 175B

---

## What does VRAM stand for?
*Basic · id 1759899441491*

Video Random-Access Memory

---

## Why do LLMs need to use a tokenizer?
*Basic · id 1759902595534*

LLMs need to work with numbers (IDs) not raw text. Tokenizers bridge the gap between text and numbers.

---

## Why don't we use words as tokens?
*Basic · id 1759902716906*

1. Vocabulary explodes in size, tokenizers allow us to hit the right balance between character-level (too granular) and word level (too sparse)
2. Out-of-vocabulary problem (new slang, misspelings, etc.)
3. Poor multi-lingual support
4. Loss of structure (happy, happiness, unhappy)

---

## What are common limitations associated with use of Tokenizers?
*Basic · id 1759902917740*

1. Character level tasks are hard (reversing, counting letters, spelling, detecting palindromes)
2. Trailing spaces and formatting quirks (" hello" can be different to "hello ")
3. Language bias (text in other languages needs more tokens to be represented!)
4. Arithmetic and symbolic reasoning (due to fragmentation any numeric reasoning is difficult)
5. Adversarial tokens exist (tokens with little representation in training may result in sporratic behavior)

---

## What are the most common tokenizers and core ideas behind them?
*Basic · id 1759903085004*

1) Byte Pair Encoding (BPE) --> <b>Core idea</b>: Start with individual characters, then iteratively merge the most frequent pairs. Used by GPT-2, GPT-3, GPT-4, LLaMA...

2) WordPiece --> <b>Core idea</b>: Similar to BPE, but chooses merges based on likelihood rather than raw frequency. Used by BERT, original Transformers...

3) Unigram Language Model --> <b>Core idea: </b>Start with a large vocabulary and iteratively remove tokens. Used by T5, some multilingual models...

---

## What are the four notable tokenization reigmes?
*Basic · id 1759903713230*

1) Word tokens: Not really used by LLMs
2) Subword tokens: Most common representation within LLMs
3) Character tokens
4) Byte tokens: Can be competitive for multilingual scenarios. Also commonly used as a fallback for unrecognized tokens in subword scenarios.

---

## How many transformer blocks did the original transformer have?
*Basic · id 1759982016821*

6

---

## Describe the approximate computation which happens in an <i>attention</i> layer (generative transformer)!
*Basic · id 1759986130707*

1. Starting position: The attention layer (of a generative LLM) is processing attention for a single position
2. The inputs to the layer are:
    1. The vector representation of the current position or token
    2. The vector representations of the previous tokens
3. The goal is to produce a new representation of the current position that incorporates relevant information from the previous tokens
4. The training process produces three projection matrices that produce the components that interact in this calculation:
    1. The **query projection matrix**
    2. The **key projection matrix**
    3. The **value projection matrix**

Above is the starting position before the computation starts! Computation:
1. Multiply the inputs by the projection matrices to create three new matrices
    1. queries
    2. keys
    3. values
2. These projections help carry out the two steps of attention:
    1. Relevance scoring
    2. Combining information
3. **Relevance scoring:** Happens by *multiplying the query vector of the current position with the keys matrix!* This produces a score stating how relevant each previous token is (to this particular *query* token). Then pass through a [[Softmax]] layer so that this $q\cdot K$ vector is normalized.
4. **Combining information:** We take these relevance scores, *we multiply the value vector associated with each token by that tokens relevance score!* Finally we sum up those *weighted* value vectors to arrive at a new vector!
5. This vector encodes contextual information from prior tokens into the embedding of the current token!

$$
\operatorname{Attn}\left(Q, K, V\right) = \operatorname{softmax}\left(\frac{Q\cdot K^T}{\sqrt{d_k}}\right)\cdot V
$$

---

## Explain the difference between <i>multi-head, multi-query, and grouped-query </i>attention
*Basic · id 1759986957893*

<ul><li><b>Multi-head</b> attention uses unique Q, K, V projections for each head</li><li><b>Multi-query</b> is an optimization where K, V are shared across all heads but Q is unique</li><li><b>Grouped-query</b> is a Multi-query approach but for several groups of heads (i.e. K, V are shared across each group but not all heads), Q is still unique for each head.</li></ul>

---

## Explain <i>flash attention</i>!
*Basic · id 1759987295661*

Popular method and implementation of attention which provides significant speedups for both training and inference of transformer LLMs on GPUs. 

It speeds up the attention calculation by optimizing what values are loaded and moved between a GPUs shared memory (SRAM) and high bandwidth memory (HBM)

---

## (empty)
*Basic · id 1759987854466*

*(empty)*

---

## Explain how <i>contrained output</i> can be enforced for LLMs in some cases!
*Basic · id 1760068829635*

You can just enforce sampling at the token stage (but can be tricky for some cases).

At each autoregressive step, you have:
1. **A partial JSON string** (what's been generated so far)
2. **A JSON grammar/schema** (the rules for valid JSON)
3. **The model's next-token probabilities** (what it wants to generate)
The key is that you can **parse the partial JSON incrementally** to determine what tokens would keep it valid.

**Step-by-step**:
Let's say you've generated: `{"name": "Al`

**Step 1: Parse the partial string**
- A JSON parser can tell you the current state: "We're inside a string value for key 'name'"

**Step 2: Determine valid next tokens**
- In this state, valid tokens are:
    - Any printable characters (to continue the string)
    - `"` (to close the string)
    - But NOT: `{`, `}`, `:`, etc. (these would break the JSON)

**Step 3: Mask the logits**
- Take the model's probability distribution over all tokens
- Set probability to zero (or -infinity in log space) for invalid tokens
- Renormalize the remaining valid tokens
- Sample from this constrained distribution

**Step 4: Update parser state and repeat**
- Generate the chosen token
- Update the parser's state machine
- Repeat for the next token

---

## Rougly how does a vision transformer work?
*Basic · id 1760148507007*

<h2></h2><strong>Step 1: Split Image into Patches</strong>

<pre><code>Input image: 224×224×3 (height × width × channels)
Patch size: 16×16
Number of patches: (224/16) × (224/16) = 14 × 14 = 196 patches
Each patch: 16×16×3 = 768 values</code></pre>

<strong>Step 2: Flatten Each Patch</strong> Each 16×16×3 patch becomes a flat vector of 768 numbers
<strong>Step 3: Linear Projection (The Actual Embedding)</strong> This is the core embedding step - a learned linear transformation
<pre><code><span style="font-style: italic;">```</span></code></pre><pre><code><span style="font-style: italic;"># Flattened patch vector</span>
x = [768 values]  <span style="font-style: italic;"># shape: (768,)</span>

<span style="font-style: italic;"># Learned weight matrix</span>
W = [768 × D]  <span style="font-style: italic;"># D is the embedding dimension (e.g., 768)</span>

<span style="font-style: italic;"># Bias term</span>
b = [D values]

<span style="font-style: italic;"># Embedding</span>
embedding = x @ W + b  <span style="font-style: italic;"># shape: (D,)</span></code></pre><pre><code><span style="font-style: italic;">```</span></code></pre>

<h2>Why a Linear Projection?</h2>This is essentially a <strong>learned feature extraction</strong> layer. Instead of hand-crafted features, the model learns what aspects of each patch are important.
Think of it as learning 768 different "filters" or "feature detectors" that look at the raw patch pixels.
<h2>Alternative: Convolutional Embedding (More Common)</h2>In practice, most ViT implementations use a <strong>convolutional layer</strong> instead, which is mathematically equivalent but more efficient:

python
<pre><code><span style="font-style: italic;"># Instead of flatten → linear</span>
<span style="font-style: italic;"># Use a convolution with kernel_size = patch_size and stride = patch_size</span>

self.patch_embed = nn.Conv2d(
    in_channels=3,
    out_channels=embed_dim,  <span style="font-style: italic;"># 768</span>
    kernel_size=patch_size,  <span style="font-style: italic;"># 16</span>
    stride=patch_size  <span style="font-style: italic;"># 16 (non-overlapping patches)</span>
)

<span style="font-style: italic;"># For the full image</span>
image = [B, 3, 224, 224]  <span style="font-style: italic;"># batch, channels, height, width</span>
embeddings = self.patch_embed(image)  <span style="font-style: italic;"># [B, 768, 14, 14]</span>
embeddings = embeddings.flatten(2)  <span style="font-style: italic;"># [B, 768, 196]</span>
embeddings = embeddings.transpose(1, 2)  <span style="font-style: italic;"># [B, 196, 768]</span></code></pre>

<h2>Adding Position Information</h2>The patch embeddings alone don't encode position, so we add <strong>positional embeddings</strong>:

python
<pre><code><span style="font-style: italic;"># Learned positional embeddings for each of the 196 positions</span>
self.pos_embed = nn.Parameter(torch.randn(1, 196, 768))

<span style="font-style: italic;"># Add to patch embeddings</span>
embeddings = patch_embeddings + self.pos_embed</code></pre>

These are also learned during training and help the model understand spatial relationships.
<h2>The Complete Pipeline</h2>

python
<pre><code>class PatchEmbedding(nn.Module):
    def __init__(self, img_size=224, patch_size=16, embed_dim=768):
        super().__init__()
        self.num_patches = (img_size // patch_size) ** 2
        
        <span style="font-style: italic;"># Convolutional patch embedding</span>
        self.proj = nn.Conv2d(3, embed_dim, 
                             kernel_size=patch_size, 
                             stride=patch_size)
        
        <span style="font-style: italic;"># Learnable positional embeddings</span>
        self.pos_embed = nn.Parameter(
            torch.randn(1, self.num_patches + 1, embed_dim)
        )
        
        <span style="font-style: italic;"># [CLS] token (for classification)</span>
        self.cls_token = nn.Parameter(torch.randn(1, 1, embed_dim))
    
    def forward(self, x):
        B = x.shape[0]
        
        <span style="font-style: italic;"># Patch embedding: [B, 3, 224, 224] → [B, 768, 14, 14]</span>
        x = self.proj(x)
        
        <span style="font-style: italic;"># Reshape: [B, 768, 14, 14] → [B, 196, 768]</span>
        x = x.flatten(2).transpose(1, 2)
        
        <span style="font-style: italic;"># Add [CLS] token: [B, 196, 768] → [B, 197, 768]</span>
        cls_tokens = self.cls_token.expand(B, -1, -1)
        x = torch.cat([cls_tokens, x], dim=1)
        
        <span style="font-style: italic;"># Add positional embeddings</span>
        x = x + self.pos_embed
        
        return x</code></pre>

<h2>Key Insights</h2><ol><li><strong>It's just a learned linear layer</strong> - nothing fancy, the magic is in what the model learns</li><li><strong>Each patch is embedded independently</strong> - no interaction between patches at this stage (that happens in the transformer layers)</li><li><strong>The embedding is learned end-to-end</strong> - initialized randomly, optimized during training</li><li><strong>Position is added separately</strong> - unlike CNNs where position is implicit, ViTs add it explicitly</li></ol>

---

## What are the embedding sizes of GPT1, GPT2, and GPT3 models?
*Basic · id 1760295766461*

GPT1 -> 768
GPT2 -> 768
GPT3 -> 12_288

---

## What are positional embeddings?
*Basic · id 1760311014510*

Add position/order information to input embeddings, crucial for architectures like Transformers that have <strong>no inherent notion of sequence order</strong>.

<b>Main approaches: </b>

<ul><li>Static absolute positional embeddings (encodings) - sinusoidal</li><li>Learned absolute positional positional embeddings - lookup</li><li>Relative positoinal encodings - RoPE, T5-style bias, ALiBI</li></ul>

<strong>Key Takeaway:</strong> Positional embeddings inject sequence order into Transformers by encoding position information, enabling them to distinguish token order despite parallel processing.

---

## What is the reason behind the normalization in self-attention? Explain intuitively!
*Basic · id 1760395945234*

<strong>The Normalization:</strong> In self-attention, we divide attention scores by √d_k before softmax:

python
<pre><code>scores <span style="color: rgb(64, 120, 242);">=</span> Q @ K<span style="color: rgb(56, 58, 66);">.</span>T <span style="color: rgb(64, 120, 242);">/</span> sqrt<span style="color: rgb(56, 58, 66);">(</span>d_k<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Scaling</span>
attention_weights <span style="color: rgb(64, 120, 242);">=</span> softmax<span style="color: rgb(56, 58, 66);">(</span>scores<span style="color: rgb(56, 58, 66);">)</span>
output <span style="color: rgb(64, 120, 242);">=</span> attention_weights @ V</code></pre><pre><code><strong>Intuitive Explanation:</strong>
<strong>Problem without normalization:</strong>
<ul><li>Q and K are vectors of dimension d_k</li><li>Dot product Q·K sums d_k terms</li><li>As d_k increases, dot products grow larger in magnitude</li><li>Large values → softmax saturates into near one-hot distributions</li></ul><strong>Example:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># d_k = 512</span>
Q·K <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(80, 161, 79);">sum</span> of <span style="color: rgb(183, 107, 1);">512</span> terms ≈ large magnitude <span style="color: rgb(56, 58, 66);">(</span>e<span style="color: rgb(56, 58, 66);">.</span>g<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">50</span><span style="color: rgb(56, 58, 66);">)</span>

softmax<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">50</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">48</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">45</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span> ≈ <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.95</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.04</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.01</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Very peaked!</span></code></pre>

<strong>Why this is bad:</strong>
<ul><li>Extremely peaked softmax → tiny gradients (gradient vanishing)</li><li>Attention focuses too sharply on one token</li><li>Model loses ability to attend to multiple relevant tokens</li><li>Training becomes unstable</li></ul><strong>With √d_k scaling:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># d_k = 512, sqrt(d_k) ≈ 22.6</span>
<span style="color: rgb(56, 58, 66);">(</span>Q·K<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> sqrt<span style="color: rgb(56, 58, 66);">(</span>d_k<span style="color: rgb(56, 58, 66);">)</span> ≈ <span style="color: rgb(183, 107, 1);">50</span> <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(183, 107, 1);">22.6</span> ≈ <span style="color: rgb(183, 107, 1);">2.2</span>

softmax<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">2.2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span> ≈ <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.37</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.34</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.29</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Smoother!</span></code></pre>

<strong>Statistical Intuition:</strong>
<ul><li>If Q and K have unit variance, their dot product has variance ≈ d_k</li><li>Standard deviation grows as √d_k</li><li>Dividing by √d_k normalizes the variance back to ~1</li><li>Keeps scores in a reasonable range regardless of dimension</li></ul><strong>Analogy:</strong> Think of it like averaging vs summing grades:
<ul><li><strong>Without scaling</strong>: Sum 512 test scores → huge numbers (25,600/25,600)</li><li><strong>With scaling</strong>: Average 512 test scores → reasonable scale (50/50)</li><li>Averaging (dividing by √n) keeps values comparable across different test counts</li></ul><strong>Benefits:</strong>
<ol><li><strong>Stable gradients</strong>: Softmax operates in its linear regime</li><li><strong>Better attention distribution</strong>: Can attend to multiple tokens</li><li><strong>Dimension-independent</strong>: Works consistently across different d_k values</li><li><strong>Faster convergence</strong>: More stable training dynamics</li></ol><strong>Key Takeaway:</strong> Dividing by √d_k prevents dot products from growing too large with dimension, keeping softmax in a range with good gradients and allowing smooth attention distributions over multiple tokens.
</code></pre>

---

## What is causal (masked) attention, why/when is it used? (needs update)
*Basic · id 1760400794922*

<strong>Definition:</strong> Causal attention prevents tokens from attending to future tokens in a sequence by masking out future positions. Each token can only attend to itself and previous tokens.
<strong>How It Works:</strong>
Apply a mask before softmax to set future positions to -∞:
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Attention scores: [batch, heads, seq_len, seq_len]</span>
scores <span style="color: rgb(64, 120, 242);">=</span> Q @ K<span style="color: rgb(56, 58, 66);">.</span>T <span style="color: rgb(64, 120, 242);">/</span> sqrt<span style="color: rgb(56, 58, 66);">(</span>d_k<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Causal mask (upper triangular with -inf)</span>
mask <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>triu<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>seq_len<span style="color: rgb(56, 58, 66);">,</span> seq_len<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'-inf'</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> diagonal<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># [[0,   -inf, -inf, -inf],</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#  [0,    0,   -inf, -inf],</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#  [0,    0,    0,   -inf],</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#  [0,    0,    0,    0  ]]</span>

scores <span style="color: rgb(64, 120, 242);">=</span> scores <span style="color: rgb(64, 120, 242);">+</span> mask  <span style="color: rgb(160, 161, 167); font-style: italic;"># Future positions become -inf</span>
attention_weights <span style="color: rgb(64, 120, 242);">=</span> softmax<span style="color: rgb(56, 58, 66);">(</span>scores<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># -inf → 0 after softmax</span></code></pre>

<strong>Visual Example:</strong>

<pre><code>Input: "The cat sat"

Without mask (bidirectional):
"The" can attend to: [The, cat, sat]
"cat" can attend to: [The, cat, sat]
"sat" can attend to: [The, cat, sat]

With mask (causal):
"The" can attend to: [The]
"cat" can attend to: [The, cat]
"sat" can attend to: [The, cat, sat]</code></pre>

<strong>Why Use Causal Attention?</strong>
<strong>1. Autoregressive Generation:</strong> Models must predict next token using only past context, not future information.
<strong>2. Training-Inference Consistency:</strong>
<ul><li>During inference: model generates one token at a time (can't see future)</li><li>During training: if allowed to see future, model learns unrealistic dependencies</li><li>Causal masking ensures training matches inference conditions</li></ul><strong>3. Prevents Information Leakage:</strong> Without masking, model would "cheat" by looking ahead at answers during training.
<strong>When It's Used:</strong>
<strong>Used in:</strong>
<ul><li><strong>GPT models</strong> (decoder-only): All attention is causal for next-token prediction</li><li><strong>Decoder in Transformer</strong>: Masked self-attention in decoder blocks</li><li><strong>Language modeling tasks</strong>: Predicting next word/token</li></ul><strong>NOT used in:</strong>
<ul><li><strong>BERT</strong> (encoder-only): Uses bidirectional attention for understanding context</li><li><strong>Encoder in Transformer</strong>: Can see full input sequence</li><li><strong>Classification tasks</strong>: Need full context to classify</li></ul><strong>Architecture Comparison:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># GPT (Decoder-only) - Causal everywhere</span>
x <span style="color: rgb(64, 120, 242);">=</span> token_embedding <span style="color: rgb(64, 120, 242);">+</span> pos_embedding
<span style="color: rgb(166, 38, 164);">for</span> layer <span style="color: rgb(166, 38, 164);">in</span> layers<span style="color: rgb(56, 58, 66);">:</span>
    x <span style="color: rgb(64, 120, 242);">=</span> causal_self_attention<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Masked</span>
    x <span style="color: rgb(64, 120, 242);">=</span> ffn<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># BERT (Encoder-only) - Bidirectional</span>
x <span style="color: rgb(64, 120, 242);">=</span> token_embedding <span style="color: rgb(64, 120, 242);">+</span> pos_embedding  
<span style="color: rgb(166, 38, 164);">for</span> layer <span style="color: rgb(166, 38, 164);">in</span> layers<span style="color: rgb(56, 58, 66);">:</span>
    x <span style="color: rgb(64, 120, 242);">=</span> self_attention<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># NOT masked</span>
    x <span style="color: rgb(64, 120, 242);">=</span> ffn<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Original Transformer - Both</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Encoder: bidirectional attention</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Decoder: causal self-attention + cross-attention to encoder</span></code></pre>

<strong>Implementation Detail:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Creating causal mask</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">create_causal_mask</span><span style="color: rgb(56, 58, 66);">(</span>seq_len<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    mask <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>triu<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>seq_len<span style="color: rgb(56, 58, 66);">,</span> seq_len<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> diagonal<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">bool</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    <span style="color: rgb(166, 38, 164);">return</span> mask<span style="color: rgb(56, 58, 66);">.</span>masked_fill<span style="color: rgb(56, 58, 66);">(</span>mask<span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'-inf'</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Or using PyTorch built-in</span>
mask <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Transformer<span style="color: rgb(56, 58, 66);">.</span>generate_square_subsequent_mask<span style="color: rgb(56, 58, 66);">(</span>seq_len<span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>Key Difference from Padding Mask:</strong>
<ul><li><strong>Causal mask</strong>: Structural - prevents looking ahead (same for all examples)</li><li><strong>Padding mask</strong>: Data-specific - ignores padding tokens (varies per example)</li><li>Both can be combined!</li></ul><strong>Key Takeaway:</strong> Causal attention masks future tokens to ensure autoregressive models only use past context, maintaining consistency between training and generation. Essential for GPT-style models and language generation tasks.

---

## What is the softmax function, and why is it useful?
*Basic · id 1760403548257*

<strong>Definition:</strong> Softmax converts a vector of real numbers (logits) into a probability distribution: all values between 0 and 1 that sum to 1.
<strong>Formula:</strong>

<pre><code>softmax(x_i) = exp(x_i) / Σ exp(x_j)</code></pre>

<strong>Example:</strong>
<pre><code>logits <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.1</span><span style="color: rgb(56, 58, 66);">]</span>
exp_vals <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span>e<span style="color: rgb(64, 120, 242);">^</span><span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> e<span style="color: rgb(64, 120, 242);">^</span><span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> e<span style="color: rgb(64, 120, 242);">^</span><span style="color: rgb(183, 107, 1);">0.1</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">7.39</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.72</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1.11</span><span style="color: rgb(56, 58, 66);">]</span>
sum_exp <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">7.39</span> <span style="color: rgb(64, 120, 242);">+</span> <span style="color: rgb(183, 107, 1);">2.72</span> <span style="color: rgb(64, 120, 242);">+</span> <span style="color: rgb(183, 107, 1);">1.11</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">11.22</span>

softmax <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">7.39</span><span style="color: rgb(64, 120, 242);">/</span><span style="color: rgb(183, 107, 1);">11.22</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.72</span><span style="color: rgb(64, 120, 242);">/</span><span style="color: rgb(183, 107, 1);">11.22</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1.11</span><span style="color: rgb(64, 120, 242);">/</span><span style="color: rgb(183, 107, 1);">11.22</span><span style="color: rgb(56, 58, 66);">]</span>
        <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.659</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.242</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.099</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Sums to 1.0 ✓</span></code></pre>

<strong>Key Properties:</strong>
<ol><li><strong>Output is a probability distribution:</strong><ul><li>All values in range (0, 1)</li><li>Sum equals exactly 1.0</li><li>Can interpret as class probabilities</li></ul></li><li><strong>Preserves order:</strong><ul><li>Larger input → larger output</li><li>argmax(logits) = argmax(softmax(logits))</li></ul></li><li><strong>Differentiable:</strong><ul><li>Smooth function with well-defined gradients</li><li>Essential for backpropagation</li></ul></li><li><strong>Emphasizes differences:</strong><ul><li>Exponential amplifies gaps between values</li><li>Larger logits get disproportionately more probability</li></ul></li></ol><strong>Why It's Useful:</strong>
<strong>1. Multi-class Classification:</strong>

python
<pre><code>logits <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># [batch_size, num_classes]</span>
probs <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
predicted_class <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>argmax<span style="color: rgb(56, 58, 66);">(</span>probs<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>2. Attention Mechanisms:</strong> Converts attention scores to attention weights:

python
<pre><code>scores <span style="color: rgb(64, 120, 242);">=</span> Q @ K<span style="color: rgb(56, 58, 66);">.</span>T <span style="color: rgb(64, 120, 242);">/</span> sqrt<span style="color: rgb(56, 58, 66);">(</span>d_k<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Raw similarity scores</span>
attention_weights <span style="color: rgb(64, 120, 242);">=</span> softmax<span style="color: rgb(56, 58, 66);">(</span>scores<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Normalized weights</span>
output <span style="color: rgb(64, 120, 242);">=</span> attention_weights @ V  <span style="color: rgb(160, 161, 167); font-style: italic;"># Weighted combination</span></code></pre>

<strong>3. Probabilistic Interpretation:</strong> Enables sampling, confidence estimation, and Bayesian approaches:

python
<pre><code>probs <span style="color: rgb(64, 120, 242);">=</span> softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">)</span>
sampled_token <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>multinomial<span style="color: rgb(56, 58, 66);">(</span>probs<span style="color: rgb(56, 58, 66);">,</span> num_samples<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
confidence <span style="color: rgb(64, 120, 242);">=</span> probs<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">max</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Confidence in prediction</span></code></pre>

<strong>4. Works with Cross-Entropy Loss:</strong> Natural pairing for classification:

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Numerically stable combined operation</span>
loss <span style="color: rgb(64, 120, 242);">=</span> CrossEntropyLoss<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Internally computes: -log(softmax(logits)[target])</span></code></pre>

<strong>Softmax vs Other Functions:</strong>
<pre><table><tbody><tr><th>Function</th><th>Output Range</th><th>Sums to 1?</th><th>Use Case</th></tr><tr><td>Softmax</td><td>(0, 1)</td><td>Yes</td><td>Multi-class (mutually exclusive)</td></tr><tr><td>Sigmoid</td><td>(0, 1)</td><td>No</td><td>Binary or multi-label</td></tr><tr><td>ReLU</td><td>[0, ∞)</td><td>No</td><td>Hidden layer activations</td></tr></tbody></table></pre><strong>Temperature Scaling:</strong> Can adjust "sharpness" of distribution:

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Temperature τ controls peakiness</span>
softmax<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(64, 120, 242);">/</span>τ<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># τ < 1: Sharper (more confident)</span>
softmax<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">/</span><span style="color: rgb(183, 107, 1);">0.5</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.84</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.14</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.02</span><span style="color: rgb(56, 58, 66);">]</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># τ > 1: Smoother (more uniform)  </span>
softmax<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">/</span><span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.51</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.31</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.18</span><span style="color: rgb(56, 58, 66);">]</span></code></pre>

<strong>Numerical Stability:</strong> Subtract max before exponentiating to prevent overflow:

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Unstable</span>
softmax<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> exp<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(80, 161, 79);">sum</span><span style="color: rgb(56, 58, 66);">(</span>exp<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Stable (mathematically equivalent)</span>
x_shifted <span style="color: rgb(64, 120, 242);">=</span> x <span style="color: rgb(64, 120, 242);">-</span> <span style="color: rgb(80, 161, 79);">max</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>
softmax<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> exp<span style="color: rgb(56, 58, 66);">(</span>x_shifted<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(80, 161, 79);">sum</span><span style="color: rgb(56, 58, 66);">(</span>exp<span style="color: rgb(56, 58, 66);">(</span>x_shifted<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>Key Takeaway:</strong> Softmax converts raw scores into valid probability distributions, making it essential for classification, attention mechanisms, and any scenario requiring normalized, interpretable outputs that sum to 1.

---

## Explain dropout!
*Basic · id 1760403658759*

<strong>Definition:</strong> Dropout is a regularization technique that randomly "drops" (sets to zero) a fraction of neurons during training to prevent overfitting.
<strong>
</strong>
<strong>How It Works:</strong>
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># During training (p=0.5 dropout rate)</span>
x <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">3.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">4.0</span><span style="color: rgb(56, 58, 66);">]</span>
mask <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Random binary mask</span>
output <span style="color: rgb(64, 120, 242);">=</span> x <span style="color: rgb(64, 120, 242);">*</span> mask <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">3.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.0</span><span style="color: rgb(56, 58, 66);">]</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Scale remaining values to maintain expected sum</span>
output <span style="color: rgb(64, 120, 242);">=</span> output <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">-</span> p<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">6.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.0</span><span style="color: rgb(56, 58, 66);">]</span></code></pre>

<strong>PyTorch Implementation:</strong>

python
<pre><code>dropout <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Dropout<span style="color: rgb(56, 58, 66);">(</span>p<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">0.5</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Drop 50% of neurons</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Training mode</span>
model<span style="color: rgb(56, 58, 66);">.</span>train<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
x <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">3.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">4.0</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span>
output <span style="color: rgb(64, 120, 242);">=</span> dropout<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Some neurons zeroed out randomly</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Evaluation mode  </span>
model<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">eval</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
output <span style="color: rgb(64, 120, 242);">=</span> dropout<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># No dropout applied (returns x unchanged)</span></code></pre>

<strong>Why It Works:</strong>
<strong>1. Prevents Co-adaptation:</strong>
<ul><li>Forces neurons to not rely on specific other neurons</li><li>Each neuron must learn robust features independently</li><li>Network can't memorize training data through complex co-dependencies</li></ul><strong>2. Ensemble Effect:</strong>
<ul><li>Each forward pass uses a different "sub-network"</li><li>Training dropout = training exponentially many thinned networks</li><li>At test time, using all neurons ≈ averaging ensemble predictions</li></ul><strong>3. Noise Injection:</strong>
<ul><li>Acts as data augmentation at the neural level</li><li>Makes model more robust to perturbations</li></ul><strong>Inverted Dropout (Standard in PyTorch):</strong>
<strong>Problem:</strong> If we drop 50% of neurons, output magnitude halves.
<strong>Solution:</strong> Scale activations during training by 1/(1-p):

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Training: drop AND scale up</span>
<span style="color: rgb(166, 38, 164);">if</span> training<span style="color: rgb(56, 58, 66);">:</span>
    mask <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>rand_like<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">></span> p<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    output <span style="color: rgb(64, 120, 242);">=</span> x <span style="color: rgb(64, 120, 242);">*</span> mask <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">-</span> p<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Scale to maintain expectation</span>
<span style="color: rgb(166, 38, 164);">else</span><span style="color: rgb(56, 58, 66);">:</span>
    output <span style="color: rgb(64, 120, 242);">=</span> x  <span style="color: rgb(160, 161, 167); font-style: italic;"># No change at test time</span></code></pre>

<strong>Benefits:</strong>
<ul><li>Test-time behavior is simpler (no scaling needed)</li><li>Expected value of activations stays constant</li></ul><strong>Typical Usage Patterns:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">MyModel</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> dropout_rate<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">0.5</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>fc1 <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Linear<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">784</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">256</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>dropout1 <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Dropout<span style="color: rgb(56, 58, 66);">(</span>dropout_rate<span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>fc2 <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Linear<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">256</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">128</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>dropout2 <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Dropout<span style="color: rgb(56, 58, 66);">(</span>dropout_rate<span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>fc3 <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Linear<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">128</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        x <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>relu<span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">.</span>fc1<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        x <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>dropout1<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Apply after activation</span>
        x <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>relu<span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">.</span>fc2<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        x <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>dropout2<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>
        x <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>fc3<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Usually no dropout on output layer</span>
        <span style="color: rgb(166, 38, 164);">return</span> x</code></pre>

<strong>Where to Apply Dropout:</strong>
✅ <strong>Use dropout on:</strong>
<ul><li>Fully connected layers (common: p=0.5)</li><li>After activation functions</li><li>Between transformer layers</li><li>Recurrent connections in RNNs</li></ul>❌ <strong>Avoid dropout on:</strong>
<ul><li>Convolutional layers (less effective; use spatial dropout instead)</li><li>Output layer</li><li>Batch normalization layers (can conflict)</li></ul><strong>Dropout Rates:</strong>
<ul><li><strong>Fully connected:</strong> p = 0.5 (common default)</li><li><strong>Input layer:</strong> p = 0.2 (lower to preserve information)</li><li><strong>Convolutional:</strong> p = 0.1-0.3 (if used at all)</li><li><strong>Large models:</strong> Higher rates (up to 0.7-0.8)</li></ul><strong>Training vs. Evaluation:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Training</span>
model<span style="color: rgb(56, 58, 66);">.</span>train<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(166, 38, 164);">for</span> data<span style="color: rgb(56, 58, 66);">,</span> labels <span style="color: rgb(166, 38, 164);">in</span> train_loader<span style="color: rgb(56, 58, 66);">:</span>
    output <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>data<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Dropout active, random masking</span>
    loss <span style="color: rgb(64, 120, 242);">=</span> criterion<span style="color: rgb(56, 58, 66);">(</span>output<span style="color: rgb(56, 58, 66);">,</span> labels<span style="color: rgb(56, 58, 66);">)</span>
    
<span style="color: rgb(160, 161, 167); font-style: italic;"># Evaluation</span>
model<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">eval</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(166, 38, 164);">with</span> torch<span style="color: rgb(56, 58, 66);">.</span>no_grad<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    output <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>test_data<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Dropout disabled, all neurons used</span></code></pre>

<strong>Variants:</strong>
<ul><li><strong>Spatial Dropout:</strong> Drops entire feature maps in CNNs</li><li><strong>DropConnect:</strong> Drops connections (weights) instead of neurons</li><li><strong>Variational Dropout:</strong> Uses same mask across time steps in RNNs</li><li><strong>DropBlock:</strong> Drops contiguous regions in CNNs</li></ul><strong>Effect on Training:</strong>

<pre><code>Without dropout: Model overfits, memorizes training data
Training acc: 99% | Test acc: 75%

With dropout (p=0.5): Better generalization
Training acc: 92% | Test acc: 88%</code></pre>

<strong>Key Takeaway:</strong> Dropout randomly disables neurons during training to prevent overfitting by forcing the network to learn redundant, robust representations. It's turned off during evaluation, where all neurons contribute to predictions.

---

## Explain LayerNorm!
*Basic · id 1760491218537*

<strong>Definition:</strong> Layer Normalization normalizes inputs across the feature dimension for each individual sample, rather than across the batch dimension.
<strong>How it works:</strong>
<ul><li>For each sample independently, compute mean (μ) and variance (σ²) across all features</li><li>Normalize: <code>x_norm = (x - μ) / √(σ² + ε)</code></li><li>Apply learnable affine transformation: <code>y = γ * x_norm + β</code><ul><li>γ (scale) and β (shift) are learned parameters</li></ul></li></ul><strong>Key characteristics:</strong>
<ul><li><strong>Independent of batch size</strong> - computes statistics per sample</li><li><strong>Same normalization at train & test time</strong> - no moving averages needed</li><li><strong>Works well with sequential data</strong> and variable-length inputs</li></ul><strong>Comparison to BatchNorm:</strong>
<ul><li>BatchNorm: normalizes across batch dimension (N samples, same feature)</li><li>LayerNorm: normalizes across feature dimension (1 sample, all features)</li></ul><strong>Common use cases:</strong>
<ul><li>Transformers and attention mechanisms</li><li>RNNs and sequence models</li><li>Any architecture where batch statistics are problematic (small batches, online learning)</li></ul><strong>Formula:</strong>

<pre><code>μ = (1/H) Σ x_i
σ² = (1/H) Σ (x_i - μ)²
y = γ ⊙ [(x - μ) / √(σ² + ε)] + β</code></pre>

where H is the feature dimension size
<strong>
</strong>
<strong>PyTorch Implementation:</strong>
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn <span style="color: rgb(166, 38, 164);">as</span> nn

<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">LayerNorm</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(56, 58, 66);">,</span> eps<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1e-5</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>eps <span style="color: rgb(64, 120, 242);">=</span> eps
        self<span style="color: rgb(56, 58, 66);">.</span>gamma <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>beta <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># x shape: (batch, ..., dim)</span>
        mean <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> keepdim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">)</span>
        var <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span>var<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> keepdim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">,</span> unbiased<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">False</span><span style="color: rgb(56, 58, 66);">)</span>
        x_norm <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">-</span> mean<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> torch<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span>var <span style="color: rgb(64, 120, 242);">+</span> self<span style="color: rgb(56, 58, 66);">.</span>eps<span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(166, 38, 164);">return</span> self<span style="color: rgb(56, 58, 66);">.</span>gamma <span style="color: rgb(64, 120, 242);">*</span> x_norm <span style="color: rgb(64, 120, 242);">+</span> self<span style="color: rgb(56, 58, 66);">.</span>beta

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage</span>
ln <span style="color: rgb(64, 120, 242);">=</span> LayerNorm<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">512</span><span style="color: rgb(56, 58, 66);">)</span>
x <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">512</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, seq_len, features)</span>
out <span style="color: rgb(64, 120, 242);">=</span> ln<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># normalized across last dimension</span></code></pre>

---

## What is Batch Normalization and how does it work?
*Basic · id 1760491286566*

<strong>Definition:</strong> Batch Normalization normalizes inputs across the batch dimension, computing statistics over all samples in a batch for each feature independently.

<strong>How it works:</strong>
<ul><li>For each feature, compute mean (μ) and variance (σ²) across the batch</li><li>Normalize: <code>x_norm = (x - μ) / √(σ² + ε)</code></li><li>Apply learnable affine transformation: <code>y = γ * x_norm + β</code><ul><li>γ (scale) and β (shift) are learned per feature</li></ul></li></ul><strong>Key characteristics:</strong>
<ul><li><strong>Dependent on batch size</strong> - needs reasonably large batches (typically >16)</li><li><strong>Different behavior at train vs test</strong> - uses moving averages at inference</li><li><strong>Reduces internal covariate shift</strong> and acts as regularization</li><li><strong>Allows higher learning rates</strong> and faster convergence</li></ul><strong>Comparison to LayerNorm:</strong>
<ul><li>BatchNorm: normalizes across batch dimension (N samples, same feature)</li><li>LayerNorm: normalizes across feature dimension (1 sample, all features)</li></ul><strong>Common use cases:</strong>
<ul><li>CNNs (Convolutional Neural Networks)</li><li>Feedforward networks with large batch sizes</li><li>Computer vision tasks</li></ul><strong>Training vs Inference:</strong>
<ul><li><strong>Training</strong>: uses batch statistics (μ_batch, σ²_batch)</li><li><strong>Inference</strong>: uses running averages (μ_running, σ²_running) computed during training</li></ul><strong>Formula:</strong>

<pre><code>μ_B = (1/m) Σ x_i          # batch mean
σ²_B = (1/m) Σ (x_i - μ_B)²  # batch variance
x_norm = (x - μ_B) / √(σ²_B + ε)
y = γ * x_norm + β</code></pre>

where m is the batch size
<strong>PyTorch Implementation:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn <span style="color: rgb(166, 38, 164);">as</span> nn

<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">BatchNorm1d</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> num_features<span style="color: rgb(56, 58, 66);">,</span> eps<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1e-5</span><span style="color: rgb(56, 58, 66);">,</span> momentum<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">0.1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>eps <span style="color: rgb(64, 120, 242);">=</span> eps
        self<span style="color: rgb(56, 58, 66);">.</span>momentum <span style="color: rgb(64, 120, 242);">=</span> momentum
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Learnable parameters</span>
        self<span style="color: rgb(56, 58, 66);">.</span>gamma <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>beta <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Running statistics (not learned)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_mean'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_var'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># x shape: (batch, num_features) or (batch, num_features, length)</span>
        
        <span style="color: rgb(166, 38, 164);">if</span> self<span style="color: rgb(56, 58, 66);">.</span>training<span style="color: rgb(56, 58, 66);">:</span>
            <span style="color: rgb(160, 161, 167); font-style: italic;"># Compute batch statistics</span>
            dims <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span> <span style="color: rgb(64, 120, 242);">+</span> <span style="color: rgb(80, 161, 79);">list</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">.</span>ndim<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># all except feature dim</span>
            mean <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span>dims<span style="color: rgb(56, 58, 66);">)</span>
            var <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span>var<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span>dims<span style="color: rgb(56, 58, 66);">,</span> unbiased<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">False</span><span style="color: rgb(56, 58, 66);">)</span>
            
            <span style="color: rgb(160, 161, 167); font-style: italic;"># Update running statistics</span>
            self<span style="color: rgb(56, 58, 66);">.</span>running_mean <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">-</span> self<span style="color: rgb(56, 58, 66);">.</span>momentum<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> self<span style="color: rgb(56, 58, 66);">.</span>running_mean <span style="color: rgb(64, 120, 242);">+</span> \
                                self<span style="color: rgb(56, 58, 66);">.</span>momentum <span style="color: rgb(64, 120, 242);">*</span> mean<span style="color: rgb(56, 58, 66);">.</span>detach<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
            self<span style="color: rgb(56, 58, 66);">.</span>running_var <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">-</span> self<span style="color: rgb(56, 58, 66);">.</span>momentum<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> self<span style="color: rgb(56, 58, 66);">.</span>running_var <span style="color: rgb(64, 120, 242);">+</span> \
                               self<span style="color: rgb(56, 58, 66);">.</span>momentum <span style="color: rgb(64, 120, 242);">*</span> var<span style="color: rgb(56, 58, 66);">.</span>detach<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(166, 38, 164);">else</span><span style="color: rgb(56, 58, 66);">:</span>
            <span style="color: rgb(160, 161, 167); font-style: italic;"># Use running statistics at inference</span>
            mean <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>running_mean
            var <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>running_var
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Normalize and apply affine transform</span>
        x_norm <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">-</span> mean<span style="color: rgb(56, 58, 66);">.</span>view<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">.</span>ndim<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> \
                 torch<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span>var<span style="color: rgb(56, 58, 66);">.</span>view<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">.</span>ndim<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> self<span style="color: rgb(56, 58, 66);">.</span>eps<span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(166, 38, 164);">return</span> self<span style="color: rgb(56, 58, 66);">.</span>gamma<span style="color: rgb(56, 58, 66);">.</span>view<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">.</span>ndim<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> x_norm <span style="color: rgb(64, 120, 242);">+</span> \
               self<span style="color: rgb(56, 58, 66);">.</span>beta<span style="color: rgb(56, 58, 66);">.</span>view<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(64, 120, 242);">*</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">.</span>ndim<span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage</span>
bn <span style="color: rgb(64, 120, 242);">=</span> BatchNorm1d<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">64</span><span style="color: rgb(56, 58, 66);">)</span>
x <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">64</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">28</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">28</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, channels, height, width)</span>
out <span style="color: rgb(64, 120, 242);">=</span> bn<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># normalized across batch dimension</span></code></pre>

---

## What is GELU and how does it differ from other activation functions?
*Basic · id 1760493127235*

<strong>Definition:</strong> GELU (Gaussian Error Linear Unit) is a smooth, non-monotonic activation function that weights inputs by their value rather than gating them by their sign like ReLU.
<strong>Mathematical Formula:</strong>

<pre><code>GELU(x) = x · Φ(x)</code></pre>

where Φ(x) is the cumulative distribution function (CDF) of the standard Gaussian distribution.
<strong>Alternative formulations:</strong>
<ul><li><strong>Exact</strong>: <code>GELU(x) = x · Φ(x) = x · (1/2)[1 + erf(x/√2)]</code></li><li><strong>Approximation</strong>: <code>GELU(x) ≈ 0.5x(1 + tanh[√(2/π)(x + 0.044715x³)])</code></li><li><strong>Sigmoid approximation</strong>: <code>GELU(x) ≈ x · σ(1.702x)</code></li></ul><strong>Key characteristics:</strong>
<ul><li><strong>Smooth and differentiable</strong> everywhere (unlike ReLU)</li><li><strong>Non-monotonic</strong> - has a small negative region for negative inputs</li><li><strong>Stochastic regularizer interpretation</strong> - probabilistically drops inputs based on their value</li><li><strong>State-of-the-art in NLP</strong> - used in BERT, GPT, and most modern transformers</li></ul><strong>Comparison to other activations:</strong>
<ul><li><strong>ReLU</strong>: <code>max(0, x)</code> - hard cutoff at zero</li><li><strong>ELU</strong>: smooth for negatives but exponential</li><li><strong>GELU</strong>: smoother than ReLU, weights inputs by their magnitude</li></ul><strong>Why it works:</strong>
<ul><li>Combines properties of dropout, zoneout, and ReLU</li><li>Allows small negative values (unlike ReLU's hard zero)</li><li>Better gradient flow than ReLU for negative inputs</li></ul><strong>Common use cases:</strong>
<ul><li>Transformer models (BERT, GPT, T5)</li><li>Modern NLP architectures</li><li>Vision transformers (ViT)</li></ul><strong>PyTorch Implementation:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn <span style="color: rgb(166, 38, 164);">as</span> nn
<span style="color: rgb(166, 38, 164);">import</span> math

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 1: Built-in PyTorch</span>
gelu <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>GELU<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 2: Exact implementation</span>
<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">GELUExact</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(183, 107, 1);">0.5</span> <span style="color: rgb(64, 120, 242);">*</span> x <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">+</span> torch<span style="color: rgb(56, 58, 66);">.</span>erf<span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">/</span> math<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 3: Fast approximation (used in original BERT)</span>
<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">GELUApprox</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(183, 107, 1);">0.5</span> <span style="color: rgb(64, 120, 242);">*</span> x <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">+</span> torch<span style="color: rgb(56, 58, 66);">.</span>tanh<span style="color: rgb(56, 58, 66);">(</span>
            math<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span> <span style="color: rgb(64, 120, 242);">/</span> math<span style="color: rgb(56, 58, 66);">.</span>pi<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">+</span> <span style="color: rgb(183, 107, 1);">0.044715</span> <span style="color: rgb(64, 120, 242);">*</span> x<span style="color: rgb(64, 120, 242);">**</span><span style="color: rgb(183, 107, 1);">3</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 4: Sigmoid approximation</span>
<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">GELUSigmoid</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(166, 38, 164);">return</span> x <span style="color: rgb(64, 120, 242);">*</span> torch<span style="color: rgb(56, 58, 66);">.</span>sigmoid<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1.702</span> <span style="color: rgb(64, 120, 242);">*</span> x<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage</span>
x <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">512</span><span style="color: rgb(56, 58, 66);">)</span>
out <span style="color: rgb(64, 120, 242);">=</span> gelu<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Comparison</span>
<span style="color: rgb(166, 38, 164);">import</span> matplotlib<span style="color: rgb(56, 58, 66);">.</span>pyplot <span style="color: rgb(166, 38, 164);">as</span> plt
x_range <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>linspace<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">3</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">3</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1000</span><span style="color: rgb(56, 58, 66);">)</span>
relu <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>relu<span style="color: rgb(56, 58, 66);">(</span>x_range<span style="color: rgb(56, 58, 66);">)</span>
gelu_exact <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0.5</span> <span style="color: rgb(64, 120, 242);">*</span> x_range <span style="color: rgb(64, 120, 242);">*</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span> <span style="color: rgb(64, 120, 242);">+</span> torch<span style="color: rgb(56, 58, 66);">.</span>erf<span style="color: rgb(56, 58, 66);">(</span>x_range <span style="color: rgb(64, 120, 242);">/</span> math<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># GELU is smooth and allows small negative values</span></code></pre>

<strong>Visual intuition:</strong>
<ul><li>For large positive x: GELU(x) ≈ x (like ReLU)</li><li>For large negative x: GELU(x) ≈ 0 (like ReLU)</li><li>For x near 0: smooth transition, small negative values pass through</li><li>Minimum around x ≈ -0.17 with value ≈ -0.17</li></ul>

---

## Describe the detailed architecture of a GPT-2 transformer block, including components, flow, and approximate parameter counts.
*Basic · id 1760493680606*

<strong>Overall Architecture:</strong> GPT-2 consists of stacked transformer decoder blocks with causal (autoregressive) masking. Each block follows a consistent pattern with residual connections.

<strong>Single Transformer Block Flow:</strong>

<pre><code>Input (d_model)
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
- **First linear layer**: d_model → 4 × d_model
- **Activation**: GELU
- **Second linear layer**: 4 × d_model → d_model
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
Softmax</code></pre>

<img alt="figure" src="4-15.png"><strong>
</strong>
<strong>
</strong>
<strong>Key Design Choices:</strong>
<ul><li><strong>Pre-norm</strong> (LayerNorm before sublayers) instead of post-norm</li><li><strong>Causal masking</strong> for autoregressive generation</li><li><strong>No encoder-decoder attention</strong> (decoder-only)</li><li><strong>GELU activation</strong> instead of ReLU</li><li><strong>Learned positional embeddings</strong> (not sinusoidal)</li></ul><pre><code>
<span style="color: rgb(160, 161, 167); font-style: italic;"># GPT-2 Small</span>
model <span style="color: rgb(64, 120, 242);">=</span> GPT2<span style="color: rgb(56, 58, 66);">(</span>vocab_size<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">50257</span><span style="color: rgb(56, 58, 66);">,</span> d_model<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">768</span><span style="color: rgb(56, 58, 66);">,</span> n_heads<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">12</span><span style="color: rgb(56, 58, 66);">,</span> n_layers<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">12</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(166, 38, 164);">print</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">f"Total parameters: </span><span style="color: rgb(56, 58, 66);">{</span><span style="color: rgb(80, 161, 79);">sum</span><span style="color: rgb(56, 58, 66);">(</span>p<span style="color: rgb(56, 58, 66);">.</span>numel<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(166, 38, 164);">for</span> p <span style="color: rgb(166, 38, 164);">in</span> model<span style="color: rgb(56, 58, 66);">.</span>parameters<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>,<span style="color: rgb(56, 58, 66);">}</span><span style="color: rgb(80, 161, 79);">"</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Output: ~117M parameters</span></code></pre>

<strong>Memory Note:</strong> For GPT-2 Small with context length 1024:
<ul><li>Attention memory: O(n_heads × seq_len²) ≈ 12 × 1024² ≈ 12M elements</li><li>Dominates memory at long sequences</li></ul>

---

## Count the number of parameters in GPT2!
*Basic · id 1760494981141*

<img alt="figure" src="4-15.png">

TODO First time you practice

---

## Why is working with logarithms of probabilities preferred over probabilities?
*Basic · id 1760497371936*

<strong>Key Reasons:</strong>
<strong>1. Numerical Stability (Primary Reason)</strong>
<ul><li><strong>Underflow prevention</strong>: Probabilities can be extremely small (e.g., 10⁻³⁰⁰)</li><li>Multiplying many small probabilities → underflow → rounds to 0</li><li>Log space: <code>log(10⁻³⁰⁰) = -300</code> (perfectly representable)</li><li>Example: P(sequence) = 0.1 × 0.1 × ... (1000 times) ≈ 10⁻¹⁰⁰⁰ → underflow!</li><li>Log space: <code>log P = -1 + -1 + ... = -1000</code> ✓</li></ul><strong>2. Computational Efficiency</strong>
<ul><li><strong>Products → Sums</strong>: <code>log(a × b) = log(a) + log(b)</code></li><li>Addition is faster and more stable than multiplication</li><li>Example: <code>P(x₁, x₂, ..., xₙ) = ∏ P(xᵢ)</code> becomes <code>log P = Σ log P(xᵢ)</code></li></ul><strong>3. Better Optimization Properties</strong>
<ul><li><strong>Convexity</strong>: Negative log-likelihood is often convex (easier to optimize)</li><li><strong>Gradients</strong>: More numerically stable gradients</li><li>Avoids vanishing gradients from very small probabilities</li></ul><strong>4. Dynamic Range</strong>
<ul><li>Probabilities: [0, 1] (limited range)</li><li>Log probabilities: (-∞, 0] (unlimited negative range)</li><li>Better representation of very unlikely events</li></ul><strong>5. Loss Function Formulation</strong>
<ul><li>Cross-entropy loss naturally uses log probabilities</li><li><code>Loss = -Σ yᵢ log(ŷᵢ)</code> (negative log-likelihood)</li><li>Directly interpretable as "surprise" or information content</li></ul><strong>Common Applications:</strong>
<strong>Language Models:</strong>

<pre><code>P(sentence) = P(w₁) × P(w₂|w₁) × P(w₃|w₁,w₂) × ...
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
Log space: log P(i) = zᵢ - log Σ exp(zⱼ)  # LogSumExp trick</code></pre>

<strong>Critical Numerical Example:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Bad: Direct probability multiplication</span>
p <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0.01</span>
result <span style="color: rgb(64, 120, 242);">=</span> p <span style="color: rgb(64, 120, 242);">**</span> <span style="color: rgb(183, 107, 1);">1000</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Underflows to 0.0</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Good: Log probability addition</span>
log_p <span style="color: rgb(64, 120, 242);">=</span> math<span style="color: rgb(56, 58, 66);">.</span>log<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0.01</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># -4.605</span>
log_result <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">1000</span> <span style="color: rgb(64, 120, 242);">*</span> log_p  <span style="color: rgb(160, 161, 167); font-style: italic;"># -4605.17</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Can convert back if needed: exp(-4605.17)</span>
```

<span style="color: rgb(64, 120, 242);">**</span>The LogSumExp Trick<span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(64, 120, 242);">**</span>
When you need to convert back<span style="color: rgb(56, 58, 66);">:</span> `log<span style="color: rgb(56, 58, 66);">(</span>Σ exp<span style="color: rgb(56, 58, 66);">(</span>xᵢ<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>` can also underflow<span style="color: rgb(64, 120, 242);">/</span>overflow<span style="color: rgb(56, 58, 66);">.</span>

<span style="color: rgb(64, 120, 242);">**</span>Solution<span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(64, 120, 242);">**</span>
```
LSE<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(80, 161, 79);">max</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> log<span style="color: rgb(56, 58, 66);">(</span>Σ exp<span style="color: rgb(56, 58, 66);">(</span>xᵢ <span style="color: rgb(64, 120, 242);">-</span> <span style="color: rgb(80, 161, 79);">max</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

This keeps exponentials in a stable range!
<strong>PyTorch Implementation Examples:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn<span style="color: rgb(56, 58, 66);">.</span>functional <span style="color: rgb(166, 38, 164);">as</span> F

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 1: Cross-entropy with log probabilities</span>
logits <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, classes)</span>
targets <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randint<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 1: Using log_softmax (numerically stable)</span>
log_probs <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>log_softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>nll_loss<span style="color: rgb(56, 58, 66);">(</span>log_probs<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 2: Direct cross_entropy (does log_softmax internally)</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 2: Computing sequence probability</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">sequence_log_prob</span><span style="color: rgb(56, 58, 66);">(</span>token_log_probs<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    token_log_probs: (seq_len,) log probabilities
    Returns: log P(sequence)
<span style="color: rgb(80, 161, 79);">    """</span>
    <span style="color: rgb(166, 38, 164);">return</span> token_log_probs<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">sum</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Product becomes sum!</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 3: LogSumExp trick</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">log_sum_exp</span><span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""Numerically stable log(sum(exp(x)))"""</span>
    max_x <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">max</span><span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span>dim<span style="color: rgb(56, 58, 66);">,</span> keepdim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span>
    <span style="color: rgb(166, 38, 164);">return</span> max_x <span style="color: rgb(64, 120, 242);">+</span> <span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">-</span> max_x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">sum</span><span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span>dim<span style="color: rgb(56, 58, 66);">,</span> keepdim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>log<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage</span>
log_probs <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1000</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1001</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">999</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Very negative</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># torch.logsumexp(log_probs)  # PyTorch built-in</span>
result <span style="color: rgb(64, 120, 242);">=</span> log_sum_exp<span style="color: rgb(56, 58, 66);">(</span>log_probs<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># -998.59 (stable!)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 4: Language model perplexity</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">compute_perplexity</span><span style="color: rgb(56, 58, 66);">(</span>log_probs<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    log_probs: log probabilities for each token
    Perplexity = exp(-mean(log P))
<span style="color: rgb(80, 161, 79);">    """</span>
    avg_log_prob <span style="color: rgb(64, 120, 242);">=</span> log_probs<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    perplexity <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(64, 120, 242);">-</span>avg_log_prob<span style="color: rgb(56, 58, 66);">)</span>
    <span style="color: rgb(166, 38, 164);">return</span> perplexity

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 5: Comparing sequences</span>
log_prob_seq1 <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">45.2</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># log P(sequence 1)</span>
log_prob_seq2 <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">48.7</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># log P(sequence 2)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Sequence 1 is more likely (less negative)</span>
<span style="color: rgb(166, 38, 164);">if</span> log_prob_seq1 <span style="color: rgb(64, 120, 242);">></span> log_prob_seq2<span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">print</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">"Sequence 1 is more likely"</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example 6: Safe probability conversion</span>
log_prob <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1000</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Very small probability</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Bad: exp(-1000) = 0.0 (underflow)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Good: Keep in log space or use for comparison only</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># If you MUST convert (e.g., for display):</span>
<span style="color: rgb(166, 38, 164);">if</span> log_prob <span style="color: rgb(64, 120, 242);"><</span> <span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">700</span><span style="color: rgb(56, 58, 66);">:</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># threshold for float64</span>
    <span style="color: rgb(166, 38, 164);">print</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">"Probability effectively zero"</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(166, 38, 164);">else</span><span style="color: rgb(56, 58, 66);">:</span>
    prob <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span>log_prob<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>Interview Key Points:</strong>
<ul><li><strong>Numerical stability</strong> is the #1 reason</li><li>Products become sums (computationally cheaper)</li><li>Standard in ML: cross-entropy, NLL, language models</li><li>Always mention <strong>LogSumExp trick</strong> for bonus points</li><li>Know that <code>log_softmax</code> is more stable than <code>log(softmax)</code></li></ul>

---

## What is perplexity and how is it used to evaluate language models?
*Basic · id 1760498473681*

<strong>Definition:</strong> Perplexity (PPL) is a measurement of how well a probability model predicts a sample. For language models, it measures how "surprised" the model is by the test data. <strong>Lower perplexity = better model.</strong>
<strong>Mathematical Formula:</strong>

<pre><code>Perplexity = exp(-1/N Σ log P(wᵢ | context))
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
- PPL = 10 → model is as uncertain as guessing from 10 equally likely words
- PPL = 50 → model is as uncertain as guessing from 50 equally likely words
- PPL = 1 → perfect prediction (model assigns probability 1 to correct tokens)

**Key Properties:**

1. **Inverse relationship with probability**
   - High probability → Low perplexity (good)
   - Low probability → High perplexity (bad)

2. **Normalized by sequence length**
   - Allows fair comparison across different text lengths
   - Geometric mean of inverse probabilities

3. **Exponential of cross-entropy**
   - Cross-entropy in nats → `PPL = exp(H)`
   - Cross-entropy in bits → `PPL = 2^H`

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
Bits-per-character = log₂(Perplexity) / chars_per_token</code></pre>

<strong>Limitations:</strong>
<ul><li>⚠️ <strong>Doesn't measure generation quality</strong> directly</li><li>⚠️ <strong>Vocabulary-dependent</strong>: Can't compare models with different vocabularies</li><li>⚠️ <strong>Doesn't capture semantic understanding</strong></li><li>⚠️ <strong>Lower PPL ≠ better for downstream tasks</strong> (necessarily)</li></ul><strong>Common Use Cases:</strong>
<ul><li>Language model evaluation (GPT, BERT MLM)</li><li>Comparing different architectures</li><li>Hyperparameter tuning</li><li>Tracking training progress</li><li>Speech recognition confidence</li></ul><strong>PyTorch Implementation:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn<span style="color: rgb(56, 58, 66);">.</span>functional <span style="color: rgb(166, 38, 164);">as</span> F

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 1: From log probabilities</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">perplexity_from_log_probs</span><span style="color: rgb(56, 58, 66);">(</span>log_probs<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    log_probs: (seq_len,) log probabilities of correct tokens
<span style="color: rgb(80, 161, 79);">    """</span>
    avg_neg_log_prob <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span>log_probs<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    ppl <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>avg_neg_log_prob<span style="color: rgb(56, 58, 66);">)</span>
    <span style="color: rgb(166, 38, 164);">return</span> ppl

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 2: From cross-entropy loss</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">perplexity_from_loss</span><span style="color: rgb(56, 58, 66);">(</span>cross_entropy_loss<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    cross_entropy_loss: scalar, average negative log likelihood
<span style="color: rgb(80, 161, 79);">    """</span>
    <span style="color: rgb(166, 38, 164);">return</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>cross_entropy_loss<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 3: Complete example with language model</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">compute_perplexity</span><span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">,</span> tokens<span style="color: rgb(56, 58, 66);">,</span> vocab_size<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    model: language model
    tokens: (batch, seq_len) token IDs
<span style="color: rgb(80, 161, 79);">    """</span>
    model<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">eval</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    <span style="color: rgb(166, 38, 164);">with</span> torch<span style="color: rgb(56, 58, 66);">.</span>no_grad<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Get logits</span>
        logits <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>tokens<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, seq_len-1, vocab)</span>
        targets <span style="color: rgb(64, 120, 242);">=</span> tokens<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">:</span><span style="color: rgb(56, 58, 66);">]</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, seq_len-1)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Compute cross-entropy loss</span>
        loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>
            logits<span style="color: rgb(56, 58, 66);">.</span>reshape<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> vocab_size<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span>
            targets<span style="color: rgb(56, 58, 66);">.</span>reshape<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span>
            reduction<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(80, 161, 79);">'mean'</span>
        <span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Perplexity is exp of loss</span>
        ppl <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>loss<span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">return</span> ppl<span style="color: rgb(56, 58, 66);">.</span>item<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example usage</span>
tokens <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randint<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">50000</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">128</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, seq_len)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># ppl = compute_perplexity(model, tokens, vocab_size=50000)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># print(f"Perplexity: {ppl:.2f}")</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 4: Manual calculation step-by-step</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">detailed_perplexity</span><span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    logits: (seq_len, vocab_size) model predictions
    targets: (seq_len,) ground truth token IDs
<span style="color: rgb(80, 161, 79);">    """</span>
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Get log probabilities</span>
    log_probs <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>log_softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Extract log probs of correct tokens</span>
    correct_log_probs <span style="color: rgb(64, 120, 242);">=</span> log_probs<span style="color: rgb(56, 58, 66);">[</span>
        torch<span style="color: rgb(56, 58, 66);">.</span>arange<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">len</span><span style="color: rgb(56, 58, 66);">(</span>targets<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> targets
    <span style="color: rgb(56, 58, 66);">]</span>
    
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Average negative log likelihood</span>
    avg_nll <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span>correct_log_probs<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Perplexity</span>
    ppl <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>avg_nll<span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">print</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">f"Average NLL: </span><span style="color: rgb(56, 58, 66);">{</span>avg_nll<span style="color: rgb(56, 58, 66);">:</span>.4f<span style="color: rgb(56, 58, 66);">}</span><span style="color: rgb(80, 161, 79);">"</span><span style="color: rgb(56, 58, 66);">)</span>
    <span style="color: rgb(166, 38, 164);">print</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">f"Perplexity: </span><span style="color: rgb(56, 58, 66);">{</span>ppl<span style="color: rgb(56, 58, 66);">:</span>.4f<span style="color: rgb(56, 58, 66);">}</span><span style="color: rgb(80, 161, 79);">"</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">return</span> ppl

<span style="color: rgb(160, 161, 167); font-style: italic;"># Method 5: Per-token perplexity (for analysis)</span>
<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">per_token_perplexity</span><span style="color: rgb(56, 58, 66);">(</span>log_probs<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    Returns perplexity for each token position
    log_probs: (seq_len,) log P(correct token)
<span style="color: rgb(80, 161, 79);">    """</span>
    <span style="color: rgb(166, 38, 164);">return</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(64, 120, 242);">-</span>log_probs<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Example: Track perplexity during training</span>
<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">PerplexityTracker</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        self<span style="color: rgb(56, 58, 66);">.</span>total_loss <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0.0</span>
        self<span style="color: rgb(56, 58, 66);">.</span>total_tokens <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">update</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> loss<span style="color: rgb(56, 58, 66);">,</span> num_tokens<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">"""loss: batch loss, num_tokens: number of tokens in batch"""</span>
        self<span style="color: rgb(56, 58, 66);">.</span>total_loss <span style="color: rgb(64, 120, 242);">+=</span> loss<span style="color: rgb(56, 58, 66);">.</span>item<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">*</span> num_tokens
        self<span style="color: rgb(56, 58, 66);">.</span>total_tokens <span style="color: rgb(64, 120, 242);">+=</span> num_tokens
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">compute</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">"""Returns average perplexity over all batches"""</span>
        avg_loss <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>total_loss <span style="color: rgb(64, 120, 242);">/</span> self<span style="color: rgb(56, 58, 66);">.</span>total_tokens
        <span style="color: rgb(166, 38, 164);">return</span> torch<span style="color: rgb(56, 58, 66);">.</span>exp<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span>avg_loss<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>item<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">reset</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        self<span style="color: rgb(56, 58, 66);">.</span>total_loss <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0.0</span>
        self<span style="color: rgb(56, 58, 66);">.</span>total_tokens <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">0</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage in training loop</span>
tracker <span style="color: rgb(64, 120, 242);">=</span> PerplexityTracker<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># for batch in dataloader:</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     logits = model(batch['input_ids'])</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     loss = F.cross_entropy(logits, batch['targets'])</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     </span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     num_tokens = batch['targets'].numel()</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     tracker.update(loss, num_tokens)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     </span>
<span style="color: rgb(160, 161, 167); font-style: italic;">#     # ... backward pass ...</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># </span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># epoch_ppl = tracker.compute()</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># print(f"Epoch Perplexity: {epoch_ppl:.2f}")</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># tracker.reset()</span></code></pre>

<strong>Real-World Examples:</strong>
<strong>GPT-2 Perplexities (WebText test set):</strong>
<ul><li>GPT-2 Small (117M): ~29.4</li><li>GPT-2 Medium (345M): ~26.4</li><li>GPT-2 Large (762M): ~24.1</li><li>GPT-2 XL (1.5B): ~22.8</li></ul><strong>Common Benchmark Values:</strong>
<ul><li>Penn Treebank: 20-60 (modern models)</li><li>WikiText-103: 15-30 (modern models)</li><li>LAMBADA: measured in accuracy, not PPL</li></ul><strong>Interview Key Points:</strong> 
✅ <strong>Lower is better</strong> (measures surprise/uncertainty) 
✅ <strong>Exponential of cross-entropy loss</strong> 
✅ <strong>"On average, confused among PPL choices"</strong> 
✅ <strong>Length-normalized</strong> comparison 
✅ <strong>Limitations</strong>: doesn't directly measure quality or understanding 
✅ <strong>Cannot compare</strong> across different vocabularies

---

## What is cross-entropy loss and why is it used in classification tasks?
*Basic · id 1760505309985*

<strong>Definition:</strong> Cross-entropy measures the dissimilarity between predicted probability distribution and true distribution. For classification, it's the negative log-probability of the correct class.
<strong>
</strong>
<strong>Formulas:</strong>
<strong>Binary (2 classes):</strong>

<pre><code>L = -[y log(ŷ) + (1-y) log(1-ŷ)]

<b>Multi-class (categorical):</b>
L = -log(ŷc)  where c is the correct class
  = -Σᵢ yᵢ log(ŷᵢ)  (equivalent with one-hot y)

</code></pre><pre><code><b>Batch average:</b></code></pre><pre><code>Loss = (1/N) Σₙ -log(ŷₙ,cₙ)</code></pre>

<strong>Key Properties:</strong>
<ol><li><strong>Output range</strong>: [0, ∞)<ul><li>Perfect prediction (ŷ_c = 1): Loss = 0</li><li>Wrong prediction (ŷ_c → 0): Loss → ∞</li></ul></li><li><strong>Gradient</strong>: <code>∂L/∂logits = ŷ - y</code><ul><li>Linear in error, doesn't saturate</li><li>Much better than MSE for classification</li></ul></li><li><strong>Probabilistic interpretation</strong>: Maximizes likelihood of correct class</li><li><strong>Relationship to perplexity</strong>: <code>Perplexity = exp(Cross-Entropy)</code></li></ol><strong>Why Cross-Entropy Over MSE?</strong>
With sigmoid/softmax outputs:
<ul><li><strong>CE gradient</strong>: <code>ŷ - y</code> (clean, strong signal)</li><li><strong>MSE gradient</strong>: <code>2(ŷ - y)·ŷ·(1-ŷ)</code> (vanishes when very wrong!)</li></ul><strong>PyTorch Implementation:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">import</span> torch
<span style="color: rgb(166, 38, 164);">import</span> torch<span style="color: rgb(56, 58, 66);">.</span>nn<span style="color: rgb(56, 58, 66);">.</span>functional <span style="color: rgb(166, 38, 164);">as</span> F

<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># MULTI-CLASS CLASSIFICATION</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Input: raw logits (no softmax needed!)</span>
logits <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch_size, num_classes)</span>
targets <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randint<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># class indices [0-9]</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Standard cross-entropy (RECOMMENDED)</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># With class weights (for imbalanced data)</span>
weights <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">1.0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># higher weight = more important</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">,</span> weight<span style="color: rgb(64, 120, 242);">=</span>weights<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># With label smoothing (prevents overconfidence)</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">,</span> label_smoothing<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">0.1</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Per-sample loss (no averaging)</span>
losses <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">,</span> reduction<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(80, 161, 79);">'none'</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># shape: (32,)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># BINARY CLASSIFICATION</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># From logits (RECOMMENDED - more stable)</span>
logits <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># raw outputs</span>
targets <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randint<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># 0 or 1</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>binary_cross_entropy_with_logits<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># From probabilities (if you already have sigmoid output)</span>
probs <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>sigmoid<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># or from model with sigmoid</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>binary_cross_entropy<span style="color: rgb(56, 58, 66);">(</span>probs<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># MANUAL IMPLEMENTATION (for understanding)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>

<span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">cross_entropy_manual</span><span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(80, 161, 79);">"""
</span>    logits: (N, C) raw outputs
    targets: (N,) class indices
<span style="color: rgb(80, 161, 79);">    """</span>
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Stable log-softmax</span>
    log_probs <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>log_softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Extract log prob of correct class for each sample</span>
    N <span style="color: rgb(64, 120, 242);">=</span> logits<span style="color: rgb(56, 58, 66);">.</span>shape<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">]</span>
    correct_log_probs <span style="color: rgb(64, 120, 242);">=</span> log_probs<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span>N<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">]</span>
    
    <span style="color: rgb(160, 161, 167); font-style: italic;"># Negative log likelihood</span>
    <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(64, 120, 242);">-</span>correct_log_probs<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># COMMON PATTERNS</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># ============================================</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Pattern 1: Standard training loop</span>
<span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">Model</span><span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(166, 38, 164);">return</span> self<span style="color: rgb(56, 58, 66);">.</span>layers<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Returns LOGITS (no softmax!)</span>

model <span style="color: rgb(64, 120, 242);">=</span> Model<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
optimizer <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>optim<span style="color: rgb(56, 58, 66);">.</span>Adam<span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">.</span>parameters<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(166, 38, 164);">for</span> x_batch<span style="color: rgb(56, 58, 66);">,</span> y_batch <span style="color: rgb(166, 38, 164);">in</span> dataloader<span style="color: rgb(56, 58, 66);">:</span>
    logits <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>x_batch<span style="color: rgb(56, 58, 66);">)</span>
    loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> y_batch<span style="color: rgb(56, 58, 66);">)</span>
    
    optimizer<span style="color: rgb(56, 58, 66);">.</span>zero_grad<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    loss<span style="color: rgb(56, 58, 66);">.</span>backward<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
    optimizer<span style="color: rgb(56, 58, 66);">.</span>step<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Pattern 2: Get predictions at inference</span>
model<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">eval</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(166, 38, 164);">with</span> torch<span style="color: rgb(56, 58, 66);">.</span>no_grad<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    logits <span style="color: rgb(64, 120, 242);">=</span> model<span style="color: rgb(56, 58, 66);">(</span>x_test<span style="color: rgb(56, 58, 66);">)</span>
    probs <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Convert to probabilities</span>
    preds <span style="color: rgb(64, 120, 242);">=</span> logits<span style="color: rgb(56, 58, 66);">.</span>argmax<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>      <span style="color: rgb(160, 161, 167); font-style: italic;"># Get class predictions</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Pattern 3: Multi-label classification (multiple classes per sample)</span>
logits <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># (batch, num_labels)</span>
targets <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randint<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">32</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># binary per label</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>binary_cross_entropy_with_logits<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>Critical Rules:</strong>
⚠️ <strong>NEVER apply softmax before F.cross_entropy</strong> (it does it internally) 
⚠️ <strong>ALWAYS use logits</strong>, not probabilities with cross_entropy 
⚠️ <strong>Targets are class indices</strong> (not one-hot) for cross_entropy 
⚠️ <strong>Use <code>*_with_logits</code> versions</strong> for numerical stability
<strong>
</strong>
<strong>Numerical Stability:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># BAD (can underflow/overflow):</span>
probs <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>softmax<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span>
loss <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(64, 120, 242);">-</span>torch<span style="color: rgb(56, 58, 66);">.</span>log<span style="color: rgb(56, 58, 66);">(</span>probs<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(80, 161, 79);">range</span><span style="color: rgb(56, 58, 66);">(</span>N<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># GOOD (numerically stable):</span>
loss <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>cross_entropy<span style="color: rgb(56, 58, 66);">(</span>logits<span style="color: rgb(56, 58, 66);">,</span> targets<span style="color: rgb(56, 58, 66);">)</span></code></pre>

The stable version computes <code>log(softmax(x))</code> directly using the LogSumExp trick internally.

---

## What are the main decoding strategies for text generation in LLMs and how do they work?
*Basic · id 1760505544897*

<strong>Definition:</strong> Decoding strategies determine how to select the next token from the model's probability distribution during autoregressive text generation.
<strong>Main Strategies:</strong>
<strong>1. Greedy Decoding</strong>
<ul><li>Always pick the highest probability token</li><li>Deterministic, fast, but repetitive and boring</li><li><code>next_token = argmax(probs)</code></li></ul><strong>2. Beam Search</strong>
<ul><li>Maintain top-k sequences (beams) at each step</li><li>Explore multiple paths, keep best overall sequences</li><li>Better quality than greedy, but still deterministic</li></ul><strong>3. Temperature Sampling</strong>
<ul><li>Scale logits before softmax: <code>logits / T</code></li><li>T < 1: sharper (more confident), T > 1: flatter (more random)</li><li>T → 0: approaches greedy, T → ∞: uniform random</li></ul><strong>4. Top-k Sampling</strong>
<ul><li>Sample only from top-k highest probability tokens</li><li>Filters out unlikely tokens</li><li>Typically k = 40-50</li></ul><strong>5. Top-p (Nucleus) Sampling</strong>
<ul><li>Sample from smallest set of tokens with cumulative probability ≥ p</li><li>Adaptive vocabulary size based on confidence</li><li>Typically p = 0.9-0.95</li></ul><strong>6. Combined Strategies</strong>
<ul><li>Temperature + Top-p (most common in practice)</li><li>Temperature + Top-k</li><li>Provides controlled randomness</li></ul>

---

## What is the KV cache in LLM inference and why is it important? (needs update)
*Basic · id 1760588602711*

<strong>Definition:</strong> KV cache stores the computed Key and Value matrices from previous tokens during autoregressive generation, avoiding redundant computation of attention for already-processed tokens.
<strong>
</strong>
<strong>The Problem Without KV Cache:</strong>
In autoregressive generation, we generate one token at a time:

<pre><code>Step 1: "The" → compute attention for position 0
Step 2: "The cat" → recompute attention for positions 0,1
Step 3: "The cat sat" → recompute attention for positions 0,1,2
...

**Redundant computation**: For each new token, we recompute Q, K, V for ALL previous tokens!

**The Solution - KV Cache:**

Cache the K and V matrices from previous forward passes:
```
Step 1: Compute Q₀, K₀, V₀ → Cache K₀, V₀
Step 2: Compute Q₁ only → Reuse K₀,V₀, append K₁,V₁ to cache
Step 3: Compute Q₂ only → Reuse K₀,K₁,V₀,V₁, append K₂,V₂
```

**Why Only K and V?**

In self-attention: `Attention(Q, K, V) = softmax(QK^T / √d)V`

- **Q (Query)**: Only needed for the NEW token
- **K (Key)**: Needed for all previous tokens (cache it!)
- **V (Value)**: Needed for all previous tokens (cache it!)

**Benefits:**

1. **Speed**: O(n) → O(1) attention computation per token
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
2 × 32 layers × 2048 × 4096 × 2 bytes = ~1GB per sequence</code></pre>

<strong>Challenges:</strong>
<ol><li><strong>Memory-bound</strong>: KV cache can be massive for long contexts</li><li><strong>Batch size limitation</strong>: KV cache scales with batch × seq_len</li><li><strong>Multi-head attention</strong>: Each head has separate KV cache</li></ol>

---

## What is RMSNorm and how does it differ from LayerNorm?
*Basic · id 1760643908066*

<strong>Definition:</strong> RMSNorm (Root Mean Square Normalization) simplifies LayerNorm by removing mean centering and only normalizing by RMS (root mean square).
<strong>Equations:</strong>
<strong>LayerNorm (for comparison):</strong>

<pre><code>μ = (1/d) Σᵢ xᵢ
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
6. Scale: result * γ</code></pre>

<strong>PyTorch pseudo-code structure:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Hint: Steps for implementation</span>
rms <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>sqrt<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">pow</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">2</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">(</span>dim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(64, 120, 242);">-</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">,</span> keepdim<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">+</span> eps<span style="color: rgb(56, 58, 66);">)</span>
x_norm <span style="color: rgb(64, 120, 242);">=</span> x <span style="color: rgb(64, 120, 242);">/</span> rms
output <span style="color: rgb(64, 120, 242);">=</span> x_norm <span style="color: rgb(64, 120, 242);">*</span> gamma</code></pre>

<strong>Benefits:</strong>
<ul><li>Simpler than LayerNorm (no mean, no shift)</li><li>Faster computation (~15-20% speedup)</li><li>Similar performance in practice</li><li>Used in: LLaMA, Gopher, T5</li></ul><strong>Used in modern LLMs:</strong>
<ul><li>LLaMA/LLaMA-2</li><li>Mistral</li><li>Mixtral</li><li>T5 (called "RMS LayerNorm")</li></ul>

---

## Explain three observations which lead to the attention mechanism!
*Basic · id 1765698293967*

Setting: We want to build a network to process sequences efficiently for downstream applications.
<ul><li><b>Observation 1:</b> The inputs can be very large - and using fully connected networks is unfeasible (e.g. thousands of words times thousands of dimension for each one)</li><li><b>Observation 2:</b> Each input is of a different length, the network should share parameters across all inputs!</li><li><b>Observation 3:</b> Language is ambiguous, and representations should reflect relative importance within the sequence.</li></ul>Self-attention fulfills all of these requirements.

---

## Explain the roles (and shapes) of <i>queries</i>, <i>keys</i>, and <i>values </i>in self-attention operation
*Basic · id 1765776774461*

The input into the attention layer is a \(n\) length sequence of \(d\)-dimensional input token representations:
 \[X \sim (n, d)\]We then take this input and perform three linear projections with \(W_q\), \(W_k \sim (d, d_k)\) and \(W_v \sim (d, d_v)\)
This gives the core components of the dot-product attention layer:
\[Q = X \cdot W_q \sim (n, d_k)\]\[K = X \cdot W_k \sim (n, d_k)\]\[V = X \cdot W_v \sim (n, d_v)\]Since we usually wish to keep the representation unchanged we take \(d_v = d\). Borrowing from information theory parlor:
<ol><li>Queries -> "What am I looking for?"</li><li>Keys -> "What do I contain?"</li><li>Values -> "What should we aggregate/keep?"</li></ol>

---

## Why do newer transformer architectures (>GPT2) ommit the bias term in the QKV projections?
*Basic · id 1765777681792*

<ol><li>LayerNorm can learn the bias/shifting operation more efficiently</li><li>The biases in the projections lead to inneficient learning, some terms are constant (get erased by softmax), others are position-independent and can be more efficiently learned in other layers.</li><li>Empirical reasons! It's just not worth having it there</li><li>Architectural simplicity - Q, K, V are all meant to be views of the same semantic space. Biases would introduce unnatural shifts in representation (where only relative comparison is meant to happen)</li></ol>

---

## Where does the \(\sqrt d_k\) term in dot-product self-attention come from?
*Basic · id 1765778474836*

To mitigate the fact that the dot products of queries and keys grow with increasing \(d_k\). If you assume \(q\) and \(k\) to be sampled with mean of 0 and variance of 1. Then their dot product \(q \cdot k = \sum q_i k_i\) has mean of \(0\) and variance \(d_k\). This results in vanishing gradients as the softmax saturates - we therefore divide the matrix product by the standard deviation of the dot product variance to normalize.

---

## Explain sinusoidal positional encoding
*Basic · id 1765781139751*

Used in the original transformer for positional information. <strong>Sinusoidal Positional Encoding</strong> Fixed, deterministic patterns using sine/cosine functions:<pre><code>PE<span style="color: rgb(56, 58, 66);">(</span>pos<span style="color: rgb(56, 58, 66);">,</span> 2i<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> sin<span style="color: rgb(56, 58, 66);">(</span>pos <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(183, 107, 1);">10000</span><span style="color: rgb(64, 120, 242);">^</span><span style="color: rgb(56, 58, 66);">(</span>2i<span style="color: rgb(64, 120, 242);">/</span>d_model<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
PE<span style="color: rgb(56, 58, 66);">(</span>pos<span style="color: rgb(56, 58, 66);">,</span> 2i<span style="color: rgb(64, 120, 242);">+</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">=</span> cos<span style="color: rgb(56, 58, 66);">(</span>pos <span style="color: rgb(64, 120, 242);">/</span> <span style="color: rgb(183, 107, 1);">10000</span><span style="color: rgb(64, 120, 242);">^</span><span style="color: rgb(56, 58, 66);">(</span>2i<span style="color: rgb(64, 120, 242);">/</span>d_model<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre><pre><code><span style="color: rgb(56, 58, 66);">position -> position in sequence</span></code></pre><pre><code><span style="color: rgb(56, 58, 66);">i -> position in vector</span></code></pre>

<ul><li>Can extrapolate to longer sequences than seen during training</li><li>No additional parameters to learn</li><li>Each dimension has different frequency</li></ul>

---

## What are learned absolute positional embeddings
*Basic · id 1765781268866*

Used to inject positional information into transformers:
\[\text{PE}(pos) = \operatorname{embedding table}(pos)\]where embedding table is of shape (context length, \(d_{model})\). Basically a learned lookup table for each position, performs well in practice GPT2/BERT uses.

The drawback of these embeddings is that they cannot extrapolate, you can still do longer sequences but those positional embeddings would be randomly initialized.

---

## Explain T5 style relative attention bias
*Basic · id 1766745559103*

T5 style relative attention bias is a <b>relative positional encodings </b>scheme in an attention layer, where the score between query \(i\) and key \(j\), is computed as:
\[\operatorname{score}(i, j) = \frac{q_i \cdot k_j^T}{\sqrt{d_k}} + \operatorname{b}(i, j)\]Where \(\operatorname{b}(i, j)\) is a <b>learned </b>positional bias function. There are a few approaches for the bias operator.
1. Naive bucketing: A lookup table indexed by distance
2. Intelligent bucketing: Group similar distances into buckets, where we're exact for close tokens but logarithmic for further away tokens

The operator is <b>shared across layers (all attention layers)</b>, but <b>each head gets its own bias </b>this way different heads learn different patterns.

Advantages:
<strong></strong>
<ul><li>It's simpler and more parameter-efficient than learned absolute positions</li><li>It generalizes better to sequences longer than those seen during training</li><li>It captures the intuition that relative distances often matter more than absolute positions</li></ul>

---

## Explain Rotary Positional Embeddings (RoPE)!
*Basic · id 1766939740425*

TODO - copy from notes when first solving

---

## Explain how the ALiBI positional encoding scheme works!
*Basic · id 1766941257487*

It's the simplest relative encoding scheme, we simply penalize the attention computation via linear distance
\[q^T_mk_n - \lambda ~|m - n|\]it's computationally efficient, and can extrapolate

---

## How can you efficiently implement the RoPE calculation?
*Basic · id 1766941938214*

TODO: write out once practicing

<img src="paste-f78e787d3e18263d103c7b435589e4f8a8a8dfa3.jpg">

---

## What are some common context extension techniques for positional encoding schemes?
*Basic · id 1766944356348*

<li><strong>Position Interpolation (PI)</strong> - Scales down positions so longer sequences fit within the trained range (e.g., mapping [0, 4096] to [0, 2048] if trained on 2048 tokens).
</li><li><strong>NTK-aware scaling</strong> - Increases RoPE's base frequency parameter proportionally to context extension, preserving high-frequency (local) information better than uniform interpolation.</li><li><strong>NTK-by-parts</strong> - Applies different NTK scaling factors to different frequency bands in RoPE. Low frequencies get more aggressive scaling for long-range, high frequencies get less scaling to preserve local detail.</li><li><strong>Dynamic NTK</strong> - Adaptively adjusts the NTK scaling factor based on the actual sequence length at inference time, using less aggressive scaling for shorter sequences and more for longer ones.</li><li><strong>YaRN (Yet another RoPE extensioN)</strong> - Combines NTK-by-parts with attention temperature scaling and other refinements for improved extrapolation across a wide range of sequence lengths.</li>

---

## Explain YaRN!
*Basic · id 1766946730383*

<strong>YaRN (Yet another RoPE extensioN)</strong> is a comprehensive method for extending RoPE to much longer contexts. It combines several techniques:
<strong>1. NTK-by-parts interpolation</strong> Divides RoPE's frequency dimensions into three ranges:
<ul><li><strong>Low frequencies</strong> (long-range): Apply NTK scaling to extend their effective range</li><li><strong>High frequencies</strong> (short-range): Apply standard interpolation since they're already well-trained</li><li><strong>Middle frequencies</strong>: Smooth transition between the two approaches</li></ul>This prevents over-compressing local information while still extending global context capability.
<strong>
</strong>
<strong>2. Attention temperature scaling</strong> Adds a learnable temperature parameter to attention scores that compensates for the "entropy loss" that occurs when extending context. Longer contexts spread attention more thinly, so temperature adjustment helps maintain appropriate attention sharpness.
<strong>
</strong>
<strong>3. Modified attention scaling</strong> Adjusts the scaling factor in attention computation to account for the extended context length, preventing attention scores from becoming too diffuse.

<strong>Why it works well:</strong>
<ul><li>NTK-by-parts preserves the distinction between local and global positional information</li><li>Temperature scaling addresses the practical issue that attention distributions change character at longer lengths</li><li>The combination allows extending context 32× or more (e.g., 2K → 64K) with minimal fine-tuning</li></ul>

---

## What is the KV-Cache size for MHA, MQA, GQA, and MLQA?
*Basic · id 1768621733916*

MHA: \(2\cdot l\cdot d_n \cdot m\)
MQA: \(2\cdot l\cdot d_n\)
GQA: \(2\cdot l\cdot d_n \cdot g\)
MQLA: \(d_l \cdot l\)

---

## Explain Multi-head Latent attention
*Basic · id 1768635157075*

Equations:
<img src="paste-9a5d00137b27b99831518b45f4ff43a863699f1a.jpg">

A way to save KV cache, which is now only \((d_c + d_h^R)\cdot l\) large. Need to separate out rope, otherwise project to common space from which the keys and values can be recovered.
