<!-- note_key: 3a2a9bf23def -->
BF: What are the three core components of the PyTorch library?
BB: 1. The `Tensor` Library -> A library for array operations, optimized for efficiency across different device types
2. The automatic-differentiation engine -> Enables automatic computation of gradients for tensor operations
3. The Deep learning library -> Building blocks, for ML. Including pre-trained models, loss functions, and optimizers

---

<!-- note_key: 0c6eb4cd58c1 -->
BF: What is the difference between `.reshape` and `.view` in `PyTorch`?
BB: Both methods are used to reshape the `tensor` object. `.view` is more commonly used. 

The difference is subtle, `.view` requires the original data to be contiguous in memory and will fail if it isn't, `.reshape()` will work regardless, copying the data if necessary.

**Rule of thumb:
**Use `.view()` when you need to ensure no-copy occurs (performance-critical code)
Use `.reshape()` when unsure.

---

<!-- note_key: 6879c7a2a338 -->
BF: Why do some `PyTorch` operations on tensors like `.transpose()` or `.permute()` result in non-contiguous tensors in memory?
BB: Tensors are stored as 1D arrays in memory!

A contiguous array is just an array stored in an unbroken block of memory: to access the next value in the array, we just move to the next memory address.

Transposing and permuting is performed internally by changing the "stride" property of the array/tensor, because this is an O(1) operation, instead of copying! 

Contiguity breaks because you can no longer access next element at the next index in memory!

`.contiguous` makes a copy of the data!

---

<!-- note_key: e2feb79e57a9 -->
BF: How does `.view` work in `PyTorch`?
BB: `.view()` creates a **new tensor object** that shares the same underlying memory as the original tensor, but interprets it with different dimensions.

**Core components of the tensor:**

1. **Data pointer**: Points to the actual memory buffer
2. **Shape**: The dimensions, e.g., `(3, 4)`
3. **Strides**: How many elements to skip in memory to move along each dimension
4. **Storage offset**: Where in the memory buffer this tensor starts

**What `.view` does?**

```
x = torch.randn(3, 4)  \# Shape (3,4)
y = x.view(12)         \# Shape (12,)
```

**Steps**:

1. **Check compatibility**: Verify the total number of elements is the same (3×4 = 12 ✓)
2. **Check contiguity**: Verify the memory layout allows the new shape to be represented with valid strides
3. **Create new tensor metadata**:
   - Same data pointer (shares memory!)
   - New shape: `(12,)`
   - New strides: `(1,)`
   - Same storage offset

It is a **metadata only** operation! If incompatible strides are required for the operation then it throws an error

---

<!-- note_key: 2ec802126aea -->
BF: How does automatic differentiation work in popular ML libraries?
BB: **Core Mechanism:** Automatic differentiation (autodiff) computes derivatives by applying the chain rule systematically to elementary operations. Modern ML libraries use **reverse-mode autodiff** (backpropagation).

**Key Components:**

1. **Computational Graph**: Operations are represented as nodes, with data flowing along edges. Each operation stores:
   - Input values
   - Output values
   - Local gradient functions
2. **Forward Pass**:
   - Executes operations and computes outputs
   - Records operations in a dynamic computation graph (tape)
   - Example: PyTorch builds graph on-the-fly; TensorFlow can use static or dynamic graphs
3. **Backward Pass**:
   - Starts from loss (scalar output)
   - Applies chain rule in reverse topological order
   - Each node computes: `∂Loss/∂input = ∂Loss/∂output × ∂output/∂input`
   - Accumulates gradients for parameters

**Implementation Details:**

- **PyTorch**: Uses `autograd` with dynamic computation graphs. Tensors track operations when `requires_grad=True`
- **TensorFlow**: Uses `GradientTape` to record operations for differentiation
- **JAX**: Uses function transformations (`grad`, `jit`) for pure functional autodiff

**Advantages over alternatives:**

- More accurate than numerical differentiation
- More efficient than symbolic differentiation for high-dimensional problems
- Handles complex control flow in dynamic graphs

---

<!-- note_key: 2e34bfc27b8f -->
BF: How to get the number of elements of a tensor?
BB: `tensor.numel()`

---

<!-- note_key: e644f878ded0 -->
BF: Why would you want to use torch.no_grad()?
BB: # **Purpose:** Disables gradient computation and tracking of operations in the computational graph.

**Key Use Cases:**

1. **Inference/Evaluation**:
   - No need to compute gradients during model evaluation
   - Significantly reduces memory consumption
   - Speeds up forward pass

```
with torch.no_grad():
outputs = model(test_data)
```

1. **Manual Parameter Updates**:
   - When updating weights manually (not through optimizer)
   - Prevents these operations from being tracked

```
with torch.no_grad():
param -= learning_rate * param.grad
```

1. **Data Preprocessing/Augmentation**:
   - Operations on input data that shouldn't be part of computational graph
   - Saves memory by not storing intermediate activations

**Benefits:**

- **Memory Efficiency**: Doesn't store intermediate activations needed for backprop (major savings for large models)
- **Speed**: Faster execution without gradient bookkeeping overhead
- **Prevents Errors**: Avoids accidentally computing gradients where they're not needed

**Alternative:**

- `@torch.inference_mode()`: Similar but more aggressive optimization, cannot be used if you might enable gradients later
- `.detach()`: For specific tensors rather than context blocks

**Common Pattern:**

```
model.eval()  \# Sets batch norm, dropout to eval mode
with torch.no_grad():  \# Disables gradient computation
    predictions = model(data)
```

---

<!-- note_key: 8c14381536e1 -->
BF: What are logits, and why do usually models in `PyTorch` skip the last non-linearity?
BB: **What are Logits?** Logits are the raw, unnormalized output scores from a model before applying the final activation function (e.g., softmax or sigmoid). They represent the model's "confidence" for each class in log-odds space.

**Why Skip the Final Non-linearity?**

1. **Numerical Stability**:
   - Combined operations (softmax \+ log) are more numerically stable when fused
   - Example: `CrossEntropyLoss` internally applies log-softmax
   - Avoids issues like log(0) or overflow in exp()

```
\# Unstable
probs = softmax(logits)
loss = -log(probs[target])
  
\# Stable (what PyTorch does internally)
loss = log_softmax(logits)[target]
```

1. **Computational Efficiency**:
   - Fused operations are faster and more memory-efficient
   - `log(softmax(x))` simplifies mathematically: `x - log(sum(exp(x)))`
   - Avoids computing intermediate softmax probabilities
2. **Gradient Flow**:
   - Combined implementation has better gradient properties
   - Prevents vanishing gradients from separate softmax \+ log operations

**Common PyTorch Pattern:**

python

```
\# Model outputs logits
logits = model(x)  \# Shape: [batch, num_classes]

\# Loss function handles softmax internally
loss = nn.CrossEntropyLoss()(logits, targets)

\# For predictions, apply softmax manually
probs = torch.softmax(logits, dim=1)
```

**Key Takeaway:** Models output logits; loss functions apply the activation. This separation improves stability, efficiency, and training dynamics.

---

<!-- note_key: 9a7a30f44d9b -->
BF: What are the `Dataset` and `DataLoader` classes for?
BB: # **Purpose:** PyTorch classes that provide a standardized interface for loading and iterating over data during training/evaluation.

**Dataset Class:** Represents the data itself and defines how to access individual samples.

**Key Methods:**

- `__len__()`: Returns total number of samples
- `__getitem__(idx)`: Returns a single sample (features, label) at index `idx`

python

```
class CustomDataset(Dataset):
    def __init__(self, data, labels):
        self.data = data
        self.labels = labels
  
    def __len__(self):
        return len(self.data)
  
    def __getitem__(self, idx):
        return self.data[idx], self.labels[idx]
```

**DataLoader Class:** Wraps a Dataset and provides batching, shuffling, parallel loading, and more.

**Key Features:**

- **Batching**: Groups samples into batches automatically
- **Shuffling**: Randomizes data order each epoch
- **Parallel Loading**: Uses multiple workers to load data asynchronously
- **Automatic Collation**: Combines samples into batches (with `collate_fn`)

python

```
dataloader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True,
    num_workers=4,
    pin_memory=True  \# Faster GPU transfer
)

for batch_features, batch_labels in dataloader:
    \# batch_features: [32, ...], batch_labels: [32]
    outputs = model(batch_features)
```

**Benefits:**

- **Separation of Concerns**: Dataset handles data access; DataLoader handles iteration
- **Memory Efficiency**: Loads data on-demand, not all at once
- **Performance**: Multi-process loading prevents I/O bottlenecks
- **Flexibility**: Easy to swap datasets or change loading parameters

**Common Use:** Dataset = "what the data is", DataLoader = "how to iterate over it"

---

<!-- note_key: dfa0437d211c -->
BF: Why are `model.train()` and `model.eval()` necessary?
BB: **Purpose:** These methods switch the model between training and evaluation modes, changing the behavior of certain layers.

**Layers Affected:**

1. **Dropout**:
   - `train()`: Randomly drops neurons (e.g., with p=0.5)
   - `eval()`: Disables dropout, uses all neurons
   - Why: Dropout is a regularization technique only needed during training
2. **Batch Normalization**:
   - `train()`: Uses batch statistics (mean/std of current batch) and updates running statistics
   - `eval()`: Uses running statistics accumulated during training (fixed)
   - Why: Test batches may be small or single samples; batch stats would be unreliable
3. **Other Layers**: Layer Normalization variants, DropPath, Stochastic Depth, etc.

**Example Usage:**

python

```
\# Training
model.train()
for data, labels in train_loader:
    optimizer.zero_grad()
    outputs = model(data)  \# Dropout active, BN uses batch stats
    loss = criterion(outputs, labels)
    loss.backward()
    optimizer.step()

\# Evaluation
model.eval()
with torch.no_grad():
    for data, labels in test_loader:
        outputs = model(data)  \# Dropout off, BN uses running stats
        \# compute metrics
```

**Important:**

- `model.eval()` does NOT disable gradient computation (use `torch.no_grad()` for that)
- Forgetting `model.eval()` during inference causes incorrect predictions due to dropout/BN behavior
- Always pair: `model.eval() + torch.no_grad()` for inference

**Key Takeaway:** Controls layer behavior to ensure proper training (with regularization) and deterministic, accurate inference (without randomness).

---

<!-- note_key: cf4b0a21045b -->
BF: Why is `optimizer.zero_grad()` necessary?
BB: **Purpose:** Clears (zeros out) the gradients of all parameters tracked by the optimizer before computing new gradients.

**Why It's Necessary:**

**PyTorch accumulates gradients by default** - calling `.backward()` *adds* gradients to existing `.grad` attributes rather than replacing them.

python

```
\# Without zero_grad()
loss1.backward()  \# param.grad = grad1
loss2.backward()  \# param.grad = grad1 \+ grad2 ❌

\# With zero_grad()
loss1.backward()       \# param.grad = grad1
optimizer.zero_grad()  \# param.grad = 0
loss2.backward()       \# param.grad = grad2 ✓
```

**Standard Training Loop:**

python

```
for data, labels in dataloader:
    optimizer.zero_grad()        \# 1. Clear old gradients
  
    outputs = model(data)        \# 2. Forward pass
    loss = criterion(outputs, labels)
  
    loss.backward()              \# 3. Compute gradients
    optimizer.step()             \# 4. Update parameters
```

**When Gradient Accumulation IS Desired:**

python

```
\# Accumulate gradients over multiple batches (for large effective batch size)
for i, (data, labels) in enumerate(dataloader):
    outputs = model(data)
    loss = criterion(outputs, labels) / accumulation_steps
    loss.backward()  \# Accumulate gradients
  
    if (i + 1) % accumulation_steps == 0:
        optimizer.step()
        optimizer.zero_grad()  \# Clear after update
```

**Common Mistake:** Forgetting `zero_grad()` leads to:

- Gradients from multiple batches mixing incorrectly
- Exploding gradients
- Model failing to converge

**Key Takeaway:** Call `optimizer.zero_grad()` before each backward pass to ensure clean gradient computation, unless intentionally accumulating gradients.

---

<!-- note_key: 534aa1e2b783 -->
BF: How do you store and load `PyTorch` models?
BB: **1. Save/Load State Dictionary (RECOMMENDED)** Saves only the model parameters (weights and biases), not the model architecture.

python

```
\# Saving
torch.save(model.state_dict(), 'model_weights.pth')

\# Loading
model = MyModel()  \# Must instantiate architecture first
model.load_state_dict(torch.load('model_weights.pth'))
model.eval()  \# Set to evaluation mode
```

**2. Save/Load Entire Model** Saves both architecture and parameters (less flexible, not recommended).

python

```
\# Saving
torch.save(model, 'entire_model.pth')

\# Loading
model = torch.load('entire_model.pth')
model.eval()
```

**Best Practice - Save Checkpoint with Extra Info:**

python

```
\# Saving
torch.save({
    'epoch': epoch,
    'model_state_dict': model.state_dict(),
    'optimizer_state_dict': optimizer.state_dict(),
    'loss': loss,
    'hyperparameters': config
}, 'checkpoint.pth')

\# Loading
checkpoint = torch.load('checkpoint.pth')
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']
```

**Important Considerations:**

- **Device Mapping**: When loading on different device:

python

```
\# Load model trained on GPU to CPU
model.load_state_dict(torch.load('model.pth', map_location='cpu'))
  
\# Load to specific GPU
model.load_state_dict(torch.load('model.pth', map_location='cuda:0'))
```

- **File Extension**: `.pth` or `.pt` are conventional
- **Why state_dict is preferred**:
   - More portable and flexible
   - Survives code refactoring
   - Smaller file size
   - Compatible across PyTorch versions

**Key Takeaway:** Use `state_dict()` for production; save optimizer state for resuming training; use `map_location` for device compatibility.

---

<!-- note_key: 8414a0e4f776 -->
BF: How does using `PyTorch` work on a GPU? How can you train on multiple GPUs?
BB: **Single GPU Usage:**

Move model and data to GPU using `.to()` or `.cuda()`:

python

```
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

\# Move model to GPU
model = MyModel().to(device)

\# Move data to GPU in training loop
for data, labels in dataloader:
    data = data.to(device)
    labels = labels.to(device)
  
    outputs = model(data)  \# Computation happens on GPU
    loss = criterion(outputs, labels)

Multi-GPU Training Approaches:

1. DataParallel (Simple but Limited)

- Splits batch across GPUs, replicates model on each
- Single-process, limited scalability

python
```
model = nn.DataParallel(model)
model.to(device)
\# Use normally - PyTorch handles the rest
```

2. DistributedDataParallel (RECOMMENDED)

- Multi-process, one GPU per process
- Better performance and scaling
- More efficient gradient synchronization

python
```
\# Basic setup
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

\# Initialize process group
dist.init_process_group(backend='nccl')
local_rank = int(os.environ['LOCAL_RANK'])

\# Move model to specific GPU
model = MyModel().to(local_rank)
model = DDP(model, device_ids=[local_rank])

\# Use DistributedSampler for data
sampler = DistributedSampler(dataset)
dataloader = DataLoader(dataset, sampler=sampler)
```

Launch DDP with:

bash
```
torchrun --nproc_per_node=4 train.py
\# or
python -m torch.distributed.launch --nproc_per_node=4 train.py
```

3. Other Strategies:

- Model Parallelism: Split model across GPUs (for models too large for one GPU)
- Pipeline Parallelism: Different layers on different GPUs
- FSDP (Fully Sharded Data Parallel): For very large models, shards parameters across GPUs

Key Differences:
```
| Feature | DataParallel | DistributedDataParallel |
| --- | --- | --- |
| Processes | Single | Multiple |
| Performance | Good | Better |
| Scalability | Limited | Excellent |
| Multi-node | No | Yes |

```

Memory Considerations:

- GPU memory freed with del tensor or torch.cuda.empty_cache()
- Monitor with torch.cuda.memory_allocated()

Key Takeaway: Single GPU: use .to(device). Multiple GPUs: prefer DDP over DataParallel for better performance and scalability.
```

---

<!-- note_key: ec4aabc13751 -->
BF: What is the best practice line for making `PyTorch` code resilient on device it runs on?
BB: device = torch.device("cude" if torch.cuda.is_available() else "cpu")

\# .to(device)

---

<!-- note_key: 760378cf5dc8 -->
BF: What is the intuition behind a `torch.nn.Embedding` layer? How does it relate to a one-hot encoding?
BB: An embedding layer maps discrete tokens (words, categories, IDs) to dense, continuous vector representations. It's a **learnable lookup table** where each token has an associated vector.

Embedding is mathematically equivalent to:

1. One-hot encoding the input
2. Multiplying by a weight matrix

```
\# Manual process (inefficient)
vocab_size = 10000
one_hot = F.one_hot(token_id, num_classes=vocab_size)  \# [vocab_size]
weight_matrix = embedding.weight  \# [vocab_size, embedding_dim]
result = one_hot @ weight_matrix  \# [embedding_dim]

\# Embedding layer (efficient)
result = embedding(token_id)  \# Direct lookup, same result!

1. Efficiency:
   - One-hot: stores sparse vector of size vocab_size (e.g., 10,000 dimensions)
   - Embedding: direct lookup, returns dense vector (e.g., 300 dimensions)
   - No matrix multiplication needed
2. Learnable Representations:
   - Embedding vectors are learned during training
   - Similar tokens learn similar vectors (semantic meaning captured)
   - One-hot has no notion of similarity (all tokens equally distant)
3. Memory:
   - Stores only vocab_size × embedding_dim parameters
   - vs one-hot \+ linear layer: same parameters but slower computation
Key Takeaway: Embeddings are efficient, learnable lookup tables that convert discrete tokens to dense vectors. They're equivalent to one-hot encoding + linear layer but much faster via direct indexing.
```

---

<!-- note_key: 7f68f1cb7414 -->
BF: Explain `register_buffer()` in `PyTorch`!
BB: **Definition:** `register_buffer()` registers a tensor as part of the model's state that should be saved/loaded but **not** trained (no gradients computed).

**Basic Usage:**

python

```
class MyModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc = nn.Linear(10, 5)  \# Trainable parameter
  
        \# Register buffer - not trainable
        self.register_buffer('running_mean', torch.zeros(5))
  
    def forward(self, x):
        \# Use buffer like any tensor
        x = self.fc(x)
        return x - self.running_mean
```

**Key Characteristics:**

**1. Not a Parameter (No Gradients):**

python

```
\# Parameters (trainable)
list(model.parameters())  \# Contains fc.weight, fc.bias

\# Buffers (not trainable)
list(model.buffers())  \# Contains running_mean

\# All tensors
model.state_dict()  \# Contains both parameters AND buffers
```

**2. Saved/Loaded with Model:**

python

```
\# Saving
torch.save(model.state_dict(), 'model.pth')
\# Saves both parameters AND buffers

\# Loading
model.load_state_dict(torch.load('model.pth'))
\# Restores both parameters AND buffers
```

**3. Moved with Model:**

python

```
model.to('cuda')  \# Moves parameters AND buffers to GPU
model.to('cpu')   \# Moves everything back to CPU
```

**When to Use Buffers:**

**1. Running Statistics (BatchNorm):**

python

```
class BatchNorm(nn.Module):
    def __init__(self, num_features):
        super().__init__()
        \# Trainable parameters
        self.weight = nn.Parameter(torch.ones(num_features))
        self.bias = nn.Parameter(torch.zeros(num_features))
  
        \# Buffers for running statistics
        self.register_buffer('running_mean', torch.zeros(num_features))
        self.register_buffer('running_var', torch.ones(num_features))
        self.register_buffer('num_batches_tracked', torch.tensor(0))
```

**2. Fixed Masks/Constants:**

python

```
class CausalAttention(nn.Module):
    def __init__(self, max_len):
        super().__init__()
        \# Causal mask doesn't need gradients
        mask = torch.triu(torch.ones(max_len, max_len), diagonal=1).bool()
        self.register_buffer('causal_mask', mask)
  
    def forward(self, x):
        \# Use pre-computed mask
        scores = scores.masked_fill(self.causal_mask, float('-inf'))
```

**3. Positional Encodings:**

python

```
class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_len):
        super().__init__()
        \# Pre-compute sinusoidal encodings
        pe = self._create_positional_encoding(d_model, max_len)
        self.register_buffer('pe', pe)
  
    def forward(self, x):
        return x \+ self.pe[:x.size(1)]
```

**Buffer vs Parameter vs Regular Tensor:**

python

```
class ComparisonModel(nn.Module):
    def __init__(self):
        super().__init__()
  
        \# 1. Parameter - trainable, saved, moved with model
        self.weight = nn.Parameter(torch.randn(10, 10))
  
        \# 2. Buffer - NOT trainable, saved, moved with model
        self.register_buffer('mask', torch.ones(10))
  
        \# 3. Regular attribute - NOT trainable, NOT saved, NOT moved
        self.temp = torch.zeros(10)

model = ComparisonModel()

\# Moving to GPU
model.to('cuda')
\# weight: ✓ moved to cuda
\# mask: ✓ moved to cuda
\# temp: ✗ still on cpu!

\# Saving
torch.save(model.state_dict(), 'model.pth')
\# weight: ✓ saved
\# mask: ✓ saved
\# temp: ✗ NOT saved

\# Gradients
model.weight.requires_grad  \# True
model.mask.requires_grad    \# False
model.temp.requires_grad    \# False (if even still exists)
```

**Persistent vs Non-Persistent Buffers:**

python

```
\# Persistent buffer (default) - saved in state_dict
self.register_buffer('running_mean', torch.zeros(5), persistent=True)

\# Non-persistent buffer - NOT saved in state_dict, but still moved with model
self.register_buffer('temp_cache', torch.zeros(5), persistent=False)
```

**Common Use Cases Summary:**

```
| Use Case | Example | Why Buffer? |
| --- | --- | --- |
| Running stats | BatchNorm mean/var | Need to save, not train |
| Fixed masks | Causal attention mask | Computed once, reused |
| Positional encodings | Sinusoidal PE | Pre-computed constants |
| Normalization constants | Dataset mean/std | Fixed preprocessing values |
```

**Practical Example - Normalization Layer:**

python

```
class NormalizeLayer(nn.Module):
    def __init__(self, mean, std):
        super().__init__()
        \# Register as buffers so they move with model
        self.register_buffer('mean', torch.tensor(mean))
        self.register_buffer('std', torch.tensor(std))
  
    def forward(self, x):
        \# Normalize using stored statistics
        return (x - self.mean) / self.std

\# Usage
model = NormalizeLayer(mean=[0.485, 0.456, 0.406],
                       std=[0.229, 0.224, 0.225])
model.to('cuda')  \# mean and std automatically moved to GPU
```

**Key Takeaway:** `register_buffer()` creates tensors that are part of the model's state (saved/loaded and moved with the model) but are **not trainable parameters**. Essential for running statistics, fixed masks, and pre-computed constants that need to persist with the model.

---

<!-- note_key: 59b37e1bff41 -->
BF: How does the @ operator work on `PyTorch`? How does it work for `len(shape) > 2`​?
BB: **Definition:** The `@` operator performs matrix multiplication in PyTorch (equivalent to `torch.matmul()`).

**Basic Cases:**

**1. 2D × 2D (Standard Matrix Multiplication):**

python

```
A = torch.randn(3, 4)  \# [3, 4]
B = torch.randn(4, 5)  \# [4, 5]
C = A @ B              \# [3, 5]

\# Rule: (m, n) @ (n, p) = (m, p)
\# Inner dimensions must match!
```

**2. 1D × 1D (Dot Product):**

python

```
a = torch.randn(5)  \# [5]
b = torch.randn(5)  \# [5]
c = a @ b           \# scalar

\# Equivalent to: sum(a \* b)
```

**3. 2D × 1D (Matrix-Vector Product):**

python

```
A = torch.randn(3, 4)  \# [3, 4]
b = torch.randn(4)     \# [4]
c = A @ b              \# [3]

\# Treats b as column vector
```

**Higher-Dimensional Behavior (len(shape) > 2):**

**Key Rule:** The `@` operator treats the last two dimensions as matrices and performs **batched matrix multiplication** on them, while broadcasting over leading dimensions.

**4. 3D × 3D (Batched Matrix Multiplication):**

python

```
A = torch.randn(10, 3, 4)  \# [batch, 3, 4]
B = torch.randn(10, 4, 5)  \# [batch, 4, 5]
C = A @ B                  \# [batch, 3, 5]

\# Performs 10 separate matrix multiplications:
\# C[i] = A[i] @ B[i] for i in range(10)
```

**5. Broadcasting with Batch Dimensions:**

python

```
\# Different batch sizes - broadcasts like other PyTorch ops
A = torch.randn(2, 1, 3, 4)   \# [2, 1, 3, 4]
B = torch.randn(1, 5, 4, 6)   \# [1, 5, 4, 6]
C = A @ B                     \# [2, 5, 3, 6]

\# Broadcasting rules:
\# - Last 2 dims: matrix multiply (3,4) @ (4,6) = (3,6)
\# - Leading dims: broadcast (2,1) and (1,5) --> (2,5)
```

**Real-World Example - Transformer Attention:**

python

```
\# Q, K, V in multi-head attention
batch_size = 32
num_heads = 8
seq_len = 100
head_dim = 64

Q = torch.randn(batch_size, num_heads, seq_len, head_dim)  \# [32, 8, 100, 64]
K = torch.randn(batch_size, num_heads, seq_len, head_dim)  \# [32, 8, 100, 64]
V = torch.randn(batch_size, num_heads, seq_len, head_dim)  \# [32, 8, 100, 64]

\# Compute attention scores: Q @ K^T
scores = Q @ K.transpose(-2, -1)  \# [32, 8, 100, 100]
\# For each of 32 batches and 8 heads:
\#   (100, 64) @ (64, 100) = (100, 100)

\# Apply attention to values
attention_weights = torch.softmax(scores, dim=-1)  \# [32, 8, 100, 100]
output = attention_weights @ V  \# [32, 8, 100, 64]
\# For each batch and head:
\#   (100, 100) @ (100, 64) = (100, 64)
```

**Dimension Rules Summary:**

python

```
\# Last 2 dimensions: matrix multiplication
\# Earlier dimensions: batch dimensions (broadcasted)

Shape compatibility:
(..., m, n) @ (..., n, p) = (..., m, p)
     ↑____↑        ↑____↑
     must match

Examples:
[10, 3, 4] @ [10, 4, 5] = [10, 3, 5]        ✓
[2, 3, 4] @ [4, 5] = [2, 3, 5]              ✓ (broadcasts)
[5, 1, 3, 4] @ [1, 7, 4, 6] = [5, 7, 3, 6]  ✓ (broadcasts)
[3, 4] @ [5, 6] = ERROR                      ✗ (4 =/= 5)
```

**Common Patterns:**

**1. Batch of Matrix-Vector Products:**

python

```
A = torch.randn(100, 3, 4)  \# 100 matrices of size [3, 4]
x = torch.randn(100, 4, 1)  \# 100 vectors of size [4]
y = A @ x                   \# [100, 3, 1]
```

**2. Single Matrix with Batch of Vectors:**

python

```
A = torch.randn(3, 4)       \# Single matrix [3, 4]
X = torch.randn(100, 4, 1)  \# 100 vectors [100, 4, 1]
Y = A @ X                   \# [100, 3, 1] - broadcasts A
```

**3. Einstein Notation Alternative:**

python

```
\# @ operator
C = A @ B

\# Equivalent with einsum (more explicit)
C = torch.einsum('...ij,...jk->...ik', A, B)
```

**Comparison with Other Operations:**

python

```
A = torch.randn(2, 3, 4)
B = torch.randn(2, 4, 5)

\# Matrix multiplication
A @ B              \# [2, 3, 5] - last 2 dims multiply

\# Element-wise multiplication
A * B              \# Broadcasting error (shapes don't match)

\# Batch matrix multiply (explicit)
torch.bmm(A, B)    \# [2, 3, 5] - requires 3D exactly

\# General matmul (most flexible)
torch.matmul(A, B) \# [2, 3, 5] - same as @
```

**Key Differences:**

```
| Operation | Dimensions | Broadcasting | Use Case |
| --- | --- | --- | --- |
| @ | Any (≥1D) | Yes | General purpose |
| torch.mm | Exactly 2D | No | Simple matrices only |
| torch.bmm | Exactly 3D | No | Batched, no broadcasting |
| torch.matmul | Any | Yes | Same as @ |
```

**Key Takeaway:** The `@` operator performs matrix multiplication on the **last two dimensions** while treating all earlier dimensions as batch dimensions that follow broadcasting rules. Essential for batched operations in neural networks, especially in attention mechanisms.

---

<!-- note_key: 654195592018 -->
BF: How does `.view` transform tensors in `PyTorch`? Specifically how does it work for `len(shape) > 2`
BB: **Definition:** `.view()` reshapes a tensor without copying data. It returns a new view with different dimensions but sharing the same underlying memory.

**Basic Principle:** Elements are stored in **row-major (C-contiguous) order** in memory. `.view()` reinterprets this flat sequence with new dimensions.

**Simple Examples:**

python

```
\# 1D to 2D
x = torch.tensor([1, 2, 3, 4, 5, 6])  \# [6]
y = x.view(2, 3)  \# [2, 3]
\# [[1, 2, 3],
\#  [4, 5, 6]]

\# 2D to 1D (flatten)
z = y.view(-1)  \# [6] - same as x
\# [1, 2, 3, 4, 5, 6]

\# Using -1 for automatic dimension inference
x = torch.randn(24)
y = x.view(4, -1)  \# [4, 6] - PyTorch infers 6
```

**How It Works (Memory Layout):**

python

```
\# Memory is always a flat 1D array
x = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])  \# Shape: [2, 3]

\# In memory: [1, 2, 3, 4, 5, 6]
\#             ↑row 0↑  ↑row 1↑

\# Reshape to [3, 2]
y = x.view(3, 2)
\# [[1, 2],    ← elements 0-1
\#  [3, 4],    ← elements 2-3
\#  [5, 6]]    ← elements 4-5

\# Same memory, different interpretation!
```

**Higher Dimensions (len(shape) > 2):**

**Key Insight:** The **rightmost dimensions vary fastest** in memory (row-major order).

python

```
\# 3D tensor
x = torch.arange(24).view(2, 3, 4)  \# [2, 3, 4]
\# Shape: [batch=2, rows=3, cols=4]

\# Memory layout:
\# [0,1,2,3,  4,5,6,7,  8,9,10,11,  12,13,14,15,  16,17,18,19,  20,21,22,23]
\#  ↑batch 0, row 0↑  ↑b0,r1↑  ↑b0,r2↑  ↑batch 1, row 0↑  ↑b1,r1↑  ↑b1,r2↑

print(x)
\# [[[0,  1,  2,  3],
\#   [4,  5,  6,  7],
\#   [8,  9,  10, 11]],
\#
\#  [[12, 13, 14, 15],
\#   [16, 17, 18, 19],
\#   [20, 21, 22, 23]]]
```

**Practical Example - Reshaping in Transformers:**

python

```
\# Multi-head attention: reshape for parallel head computation
batch_size = 32
seq_len = 100
d_model = 512
num_heads = 8
head_dim = d_model // num_heads  \# 64

\# Linear projection output: [32, 100, 512]
Q = torch.randn(batch_size, seq_len, d_model)

\# Reshape to separate heads: [32, 100, 8, 64]
Q = Q.view(batch_size, seq_len, num_heads, head_dim)

\# Transpose for batched matmul: [32, 8, 100, 64]
Q = Q.transpose(1, 2)

\# Now can do batched attention per head!
```

**Complex Reshaping Example:**

python

```
\# Start with 4D tensor
x = torch.arange(120).view(2, 3, 4, 5)  \# [2, 3, 4, 5]
print(x.shape)  \# torch.Size([2, 3, 4, 5])

\# Reshape to merge middle dimensions
y = x.view(2, 12, 5)  \# [2, 12, 5]
\# Groups (3\*4=12) middle elements together

\# Reshape to split dimensions
z = x.view(2, 3, 2, 2, 5)  \# [2, 3, 2, 2, 5]
\# Splits dim 2 (size 4) into (2, 2)

\# Flatten all but batch dimension
w = x.view(2, -1)  \# [2, 60]
\# Common pattern: keep batch, flatten rest
```

**Flattening Patterns:**

python

```
\# Input image: [batch, channels, height, width]
x = torch.randn(32, 3, 224, 224)  \# [32, 3, 224, 224]

\# Flatten spatial dimensions
x = x.view(32, 3, -1)  \# [32, 3, 50176]

\# Flatten everything except batch
x = x.view(32, -1)  \# [32, 150528]

\# Complete flatten
x = x.view(-1)  \# [4816896] - single 1D tensor
```

**Important Constraints:**

**1. Total elements must match:**

python

```
x = torch.randn(2, 3, 4)  \# 24 elements
y = x.view(4, 6)          \# ✓ 24 elements
z = x.view(5, 5)          \# ✗ Error: 25 =/= 24
```

**2. Tensor must be contiguous:**

python

```
x = torch.randn(2, 3, 4)
y = x.transpose(0, 1)     \# Non-contiguous!
z = y.view(-1)            \# ✗ Error

\# Fix: make contiguous first
z = y.contiguous().view(-1)  \# ✓ Works
\# Or use reshape (handles non-contiguous automatically)
z = y.reshape(-1)         \# ✓ Works
```

**Common Patterns in Neural Networks:**

**1. Batch Flattening (CNN --> FC):**

python

```
\# After conv layers: [batch, channels, H, W]
x = torch.randn(32, 64, 7, 7)
x = x.view(32, -1)  \# [32, 3136] for fully connected layer
```

**2. Multi-Head Attention Reshaping:**

python

```
\# Split embedding into multiple heads
x = torch.randn(32, 100, 512)  \# [batch, seq, embedding]
x = x.view(32, 100, 8, 64)     \# [batch, seq, heads, head_dim]
x = x.transpose(1, 2)          \# [batch, heads, seq, head_dim]
```

**3. Combining Batch and Sequence:**

python

```
\# Process all sequences together
x = torch.randn(32, 10, 512)  \# [batch, seq, features]
x = x.view(-1, 512)           \# [320, features] - merge batch & seq
```

**.view() vs .reshape() vs .flatten():**

python

```
x = torch.randn(2, 3, 4)

\# .view() - requires contiguous, no copy
y = x.view(2, 12)  \# Fails if non-contiguous

\# .reshape() - handles non-contiguous, may copy
y = x.reshape(2, 12)  \# Always works

\# .flatten() - specialized for flattening
y = x.flatten()        \# [24] - flatten all dims
y = x.flatten(1)       \# [2, 12] - flatten from dim 1 onward
```

**Key Takeaway:** `.view()` reinterprets tensor memory with new dimensions following row-major order (rightmost dimensions vary fastest). Total elements must match, and tensor must be contiguous. Essential for reshaping in neural networks, especially for batched operations and dimension manipulation in attention mechanisms.

---

<!-- note_key: b493a2ee1322 -->
BF: How does `.transpose` transform tensors in `PyTorch`? Specifically how does it work for `len(shape) > 2`
BB: **Definition:** `.transpose(dim0, dim1)` swaps two specified dimensions of a tensor. Returns a view (no data copying) but makes the tensor **non-contiguous**.

**Basic Usage:**

python

```
\# 2D transpose (matrix transpose)
x = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])  \# [2, 3]

y = x.transpose(0, 1)  \# Swap dims 0 and 1
\# [[1, 4],
\#  [2, 5],
\#  [3, 6]]  \# [3, 2]

\# Shorthand for 2D
y = x.T  \# Same as x.transpose(0, 1)
```

**Higher Dimensions (len(shape) > 2):**

**Key Point:** `.transpose()` **only swaps TWO specific dimensions**, leaving all others unchanged.

python

```
\# 3D tensor
x = torch.arange(24).view(2, 3, 4)  \# [2, 3, 4]
print(x.shape)  \# torch.Size([2, 3, 4])

\# Swap dimensions 0 and 1
y = x.transpose(0, 1)  \# [3, 2, 4]
\# Dimension 0 (size 2) ↔ Dimension 1 (size 3)
\# Dimension 2 (size 4) unchanged

\# Swap dimensions 1 and 2  
z = x.transpose(1, 2)  \# [2, 4, 3]
\# Dimension 0 (size 2) unchanged
\# Dimension 1 (size 3) ↔ Dimension 2 (size 4)

\# Swap dimensions 0 and 2
w = x.transpose(0, 2)  \# [4, 3, 2]
\# Dimension 0 (size 2) ↔ Dimension 2 (size 4)
\# Dimension 1 (size 3) unchanged
```

**Concrete Example with Values:**

python

```
x = torch.arange(24).view(2, 3, 4)  \# [2, 3, 4]
\# [[[ 0,  1,  2,  3],
\#   [ 4,  5,  6,  7],
\#   [ 8,  9, 10, 11]],
\#
\#  [[12, 13, 14, 15],
\#   [16, 17, 18, 19],
\#   [20, 21, 22, 23]]]

\# Transpose dims 1 and 2 (last two dimensions)
y = x.transpose(1, 2)  \# [2, 4, 3]
\# [[[ 0,  4,  8],      ← Elements from column 0 of each row
\#   [ 1,  5,  9],      ← Elements from column 1
\#   [ 2,  6, 10],      ← Elements from column 2
\#   [ 3,  7, 11]],     ← Elements from column 3
\#
\#  [[12, 16, 20],
\#   [13, 17, 21],
\#   [14, 18, 22],
\#   [15, 19, 23]]]

\# Original: x[0, 1, 2] = 6
\# After transpose: y[0, 2, 1] = 6
```

**Negative Indexing:**

python

```
x = torch.randn(2, 3, 4, 5)  \# [2, 3, 4, 5]

\# These are equivalent:
y = x.transpose(2, 3)      \# Swap dims 2 and 3
y = x.transpose(-2, -1)    \# Swap last two dims (common pattern!)

\# Result: [2, 3, 5, 4]
```

**Practical Use Cases:**

**1. Attention Score Computation:**

python

```
\# Q: [batch, seq, d_model] --> [32, 100, 512]
\# K: [batch, seq, d_model] --> [32, 100, 512]

\# Need K^T for Q @ K^T
K_T = K.transpose(-2, -1)  \# [32, 512, 100]
\# Swaps last two dimensions (seq and d_model)

scores = Q @ K_T  \# [32, 100, 100]
```

**2. Multi-Head Attention Reshaping:**

python

```
\# After splitting heads: [batch, seq, heads, head_dim]
x = torch.randn(32, 100, 8, 64)  \# [32, 100, 8, 64]

\# Transpose to group by head: [batch, heads, seq, head_dim]
x = x.transpose(1, 2)  \# [32, 8, 100, 64]
\# Now dimension 1 (heads) and dimension 2 (seq) are swapped
\# Can now do batched matmul across heads!
```

**3. Image Processing (Channel Permutation):**

python

```
\# PyTorch format: [batch, channels, height, width]
x = torch.randn(32, 3, 224, 224)  \# [32, 3, 224, 224]

\# Convert to channels-last for some operations
x = x.transpose(1, 3)  \# [32, 224, 224, 3]
\# Only dims 1 and 3 swapped, dims 0 and 2 unchanged
```

**Multiple Transposes:**

python

```
x = torch.randn(2, 3, 4, 5)  \# [2, 3, 4, 5]

\# Chain multiple transposes
y = x.transpose(0, 1).transpose(2, 3)  \# [3, 2, 5, 4]
\# First: swap dims 0,1 --> [3, 2, 4, 5]
\# Then: swap dims 2,3 --> [3, 2, 5, 4]
```

**Contiguity Issue:**

python

```
x = torch.randn(2, 3, 4)
y = x.transpose(0, 1)  \# Non-contiguous!

\# This fails:
z = y.view(6, 4)  \# ✗ Error: tensor not contiguous

\# Solutions:
z = y.contiguous().view(6, 4)  \# ✓ Make contiguous first
z = y.reshape(6, 4)             \# ✓ reshape handles it automatically
```

**Why Non-Contiguous?**

python

```
\# Original memory layout (row-major)
x = torch.tensor([[1, 2], [3, 4], [5, 6]])  \# [3, 2]
\# Memory: [1, 2, 3, 4, 5, 6]

\# After transpose
y = x.transpose(0, 1)  \# [2, 3]
\# [[1, 3, 5],
\#  [2, 4, 6]]
\# Would need memory: [1, 3, 5, 2, 4, 6]
\# But still points to: [1, 2, 3, 4, 5, 6]
\# Non-contiguous! Uses strides to reinterpret.
```

**Transpose vs Permute:**

python

```
x = torch.randn(2, 3, 4, 5)

\# .transpose() - swaps TWO dimensions only
y = x.transpose(1, 2)  \# [2, 4, 3, 5]

\# .permute() - arbitrary reordering of ALL dimensions
y = x.permute(0, 2, 1, 3)  \# [2, 4, 3, 5] - same result
y = x.permute(3, 1, 0, 2)  \# [5, 3, 2, 4] - more flexible!

\# For swapping 2 dims, transpose is simpler
\# For complex reordering, permute is clearer
```

**Common Patterns:**

python

```
\# 1. Transpose last two dimensions (very common!)
x.transpose(-2, -1)  \# or x.transpose(-1, -2) - same thing

\# 2. Batch-first to sequence-first (RNNs)
x = torch.randn(32, 100, 512)  \# [batch, seq, features]
x = x.transpose(0, 1)          \# [seq, batch, features]

\# 3. Channel repositioning
x = torch.randn(32, 3, 224, 224)  \# [B, C, H, W]
x = x.transpose(1, 2)             \# [B, H, C, W]
```

**4D Example (Common in CNNs/Attention):**

python

```
\# 4D tensor: [batch, heads, seq, head_dim]
x = torch.randn(8, 12, 100, 64)  \# [8, 12, 100, 64]

\# Transpose to move heads after seq
y = x.transpose(1, 2)  \# [8, 100, 12, 64]
\# Dimensions: 0-->0, 1-->2, 2-->1, 3-->3

\# Transpose last two (for K^T in attention)
z = x.transpose(-2, -1)  \# [8, 12, 64, 100]
\# Dimensions: 0-->0, 1-->1, 2-->3, 3-->2
```

**Memory and Performance:**

python

```
\# transpose() is cheap - just changes metadata (strides)
x = torch.randn(1000, 1000, 1000)
y = x.transpose(0, 2)  \# Instant! No data copying

\# But operations on non-contiguous tensors may be slower
z = y + 1  \# Might be slower than if y was contiguous

\# Make contiguous if doing many operations
y = y.contiguous()  \# Now subsequent ops faster
```

**Key Takeaway:** `.transpose(dim0, dim1)` swaps exactly two dimensions while keeping all others in place. It returns a view (fast, no copying) but creates a non-contiguous tensor. Essential for attention mechanisms (K^T), dimension reordering in transformers, and format conversions. Use negative indices for last dimensions: `.transpose(-2, -1)` is a very common pattern.
