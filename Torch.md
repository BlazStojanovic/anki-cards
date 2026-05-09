# Torch
*19 cards · regenerated 2026-05-09 01:18 UTC from Anki via MCP · do not hand-edit*

## What are the three core components of the PyTorch library?
*Basic · id 1760207386105*

1. The `Tensor` Library -> A library for array operations, optimized for efficiency across different device types
2. The automatic-differentiation engine -> Enables automatic computation of gradients for tensor operations
3. The Deep learning library -> Building blocks, for ML. Including pre-trained models, loss functions, and optimizers

---

## What is the difference between `.reshape` and `.view` in `PyTorch`?
*Basic · id 1760232071457*

Both methods are used to reshape the `tensor` object. `.view` is more commonly used. 

The difference is subtle, `.view` requires the original data to be contiguous in memory and will fail if it isn't, `.reshape()` will work regardless, copying the data if necessary.

<b>Rule of thumb:
</b>Use `.view()` when you need to ensure no-copy occurs (performance-critical code)
Use `.reshape()` when unsure.

---

## Why do some `PyTorch` operations on tensors like `.transpose()` or `.permute()` result in non-contiguous tensors in memory?
*Basic · id 1760233035366*

Tensors are stored as 1D arrays in memory!

<span style="color: rgb(12, 13, 14);">A contiguous array is just an array stored in an unbroken block of memory: to access the next value in the array, we just move to the next memory address.
</span>
Transposing and permuting is performed internally by changing the "stride" property of the array/tensor, because this is an O(1) operation, instead of copying! 

Contiguity breaks because you can no longer access next element at the next index in memory!

`.contiguous` makes a copy of the data!

---

## How does `.view` work in `PyTorch`?
*Basic · id 1760233175963*

<code>.view()</code> creates a <b>new tensor object</b> that shares the same underlying memory as the original tensor, but interprets it with different dimensions.

<b>Core components of the tensor:</b>
<ol><li><b>Data pointer</b>: Points to the actual memory buffer</li><li><b>Shape</b>: The dimensions, e.g., <code>(3, 4)</code></li><li><b>Strides</b>: How many elements to skip in memory to move along each dimension</li><li><b>Storage offset</b>: Where in the memory buffer this tensor starts

</li></ol><b>What `.view` does?</b>

<pre><code>x <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">3</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">4</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Shape (3,4)</span>
y <span style="color: rgb(64, 120, 242);">=</span> x<span style="color: rgb(56, 58, 66);">.</span>view<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">12</span><span style="color: rgb(56, 58, 66);">)</span>         <span style="color: rgb(160, 161, 167); font-style: italic;"># Shape (12,)</span></code></pre>

<b>Steps</b>:
<ol><li><b>Check compatibility</b>: Verify the total number of elements is the same (3×4 = 12 ✓)</li><li><b>Check contiguity</b>: Verify the memory layout allows the new shape to be represented with valid strides</li><li><b>Create new tensor metadata</b>:<ul><li>Same data pointer (shares memory!)</li><li>New shape: <code>(12,)</code></li><li>New strides: <code>(1,)</code></li><li>Same storage offset</li></ul></li></ol>
It is a <b>metadata only </b>operation! If incompatible strides are required for the operation then it throws an error

---

## How does automatic differentiation work in popular ML libraries?
*Basic · id 1760236232901*

<strong>Core Mechanism:</strong> Automatic differentiation (autodiff) computes derivatives by applying the chain rule systematically to elementary operations. Modern ML libraries use <strong>reverse-mode autodiff</strong> (backpropagation).
<strong>Key Components:</strong>
<ol><li><strong>Computational Graph</strong>: Operations are represented as nodes, with data flowing along edges. Each operation stores:<ul><li>Input values</li><li>Output values</li><li>Local gradient functions</li></ul></li><li><strong>Forward Pass</strong>:<ul><li>Executes operations and computes outputs</li><li>Records operations in a dynamic computation graph (tape)</li><li>Example: PyTorch builds graph on-the-fly; TensorFlow can use static or dynamic graphs</li></ul></li><li><strong>Backward Pass</strong>:<ul><li>Starts from loss (scalar output)</li><li>Applies chain rule in reverse topological order</li><li>Each node computes: <code>∂Loss/∂input = ∂Loss/∂output × ∂output/∂input</code></li><li>Accumulates gradients for parameters</li></ul></li></ol><strong>Implementation Details:</strong>
<ul><li><strong>PyTorch</strong>: Uses <code>autograd</code> with dynamic computation graphs. Tensors track operations when <code>requires_grad=True</code></li><li><strong>TensorFlow</strong>: Uses <code>GradientTape</code> to record operations for differentiation</li><li><strong>JAX</strong>: Uses function transformations (<code>grad</code>, <code>jit</code>) for pure functional autodiff</li></ul><strong>Advantages over alternatives:</strong>
<ul><li>More accurate than numerical differentiation</li><li>More efficient than symbolic differentiation for high-dimensional problems</li><li>Handles complex control flow in dynamic graphs</li></ul>

---

## How to get the number of elements of a tensor?
*Basic · id 1760237453958*

`tensor.numel()`

---

## Why would you want to use torch.no_grad()?
*Basic · id 1760238381178*

<h1><strong style="font-size: 20px;">Purpose:</strong><span style="font-size: 20px;"> </span><span style="font-size: 20px;">Disables gradient computation and tracking of operations in the computational graph.</span>
</h1><strong>Key Use Cases:</strong>
<ol><li><strong>Inference/Evaluation</strong>:<ul><li>No need to compute gradients during model evaluation</li><li>Significantly reduces memory consumption</li><li>Speeds up forward pass</li></ul></li></ol><pre><code>   with torch.no_grad():
   outputs = model(test_data)</code></pre>

<ol><li><strong>Manual Parameter Updates</strong>:<ul><li>When updating weights manually (not through optimizer)</li><li>Prevents these operations from being tracked</li></ul></li></ol><pre><code>   with torch.no_grad():
   param -= learning_rate * param.grad</code></pre>

<ol><li><strong>Data Preprocessing/Augmentation</strong>:<ul><li>Operations on input data that shouldn't be part of computational graph</li><li>Saves memory by not storing intermediate activations</li></ul></li></ol><strong>Benefits:</strong>
<ul><li><strong>Memory Efficiency</strong>: Doesn't store intermediate activations needed for backprop (major savings for large models)</li><li><strong>Speed</strong>: Faster execution without gradient bookkeeping overhead</li><li><strong>Prevents Errors</strong>: Avoids accidentally computing gradients where they're not needed</li></ul><strong>Alternative:</strong>
<ul><li><code>@torch.inference_mode()</code>: Similar but more aggressive optimization, cannot be used if you might enable gradients later</li><li><code>.detach()</code>: For specific tensors rather than context blocks</li></ul><strong>Common Pattern:</strong>
<pre><code>model.eval()  <span style="font-style: italic;"># Sets batch norm, dropout to eval mode</span>
with torch.no_grad():  <span style="font-style: italic;"># Disables gradient computation</span>
    predictions = model(data)</code></pre>

---

## What are logits, and why do usually models in <code>PyTorch</code> skip the last non-linearity?
*Basic · id 1760238564202*

<strong>What are Logits?</strong> Logits are the raw, unnormalized output scores from a model before applying the final activation function (e.g., softmax or sigmoid). They represent the model's "confidence" for each class in log-odds space.

<strong>Why Skip the Final Non-linearity?</strong>
<ol><li><strong>Numerical Stability</strong>:<ul><li>Combined operations (softmax + log) are more numerically stable when fused</li><li>Example: <code>CrossEntropyLoss</code> internally applies log-softmax</li><li>Avoids issues like log(0) or overflow in exp()</li></ul></li></ol><pre><code>   <span style="font-style: italic;"># Unstable</span>
   probs = softmax(logits)
   loss = -log(probs[target])
   
   <span style="font-style: italic;"># Stable (what PyTorch does internally)</span>
   loss = log_softmax(logits)[target]</code></pre>

<ol><li><strong>Computational Efficiency</strong>:<ul><li>Fused operations are faster and more memory-efficient</li><li><code>log(softmax(x))</code> simplifies mathematically: <code>x - log(sum(exp(x)))</code></li><li>Avoids computing intermediate softmax probabilities</li></ul></li><li><strong>Gradient Flow</strong>:<ul><li>Combined implementation has better gradient properties</li><li>Prevents vanishing gradients from separate softmax + log operations</li></ul></li></ol><strong>Common PyTorch Pattern:</strong>

python
<pre><code><span style="font-style: italic;"># Model outputs logits</span>
logits = model(x)  <span style="font-style: italic;"># Shape: [batch, num_classes]</span>

<span style="font-style: italic;"># Loss function handles softmax internally</span>
loss = nn.CrossEntropyLoss()(logits, targets)

<span style="font-style: italic;"># For predictions, apply softmax manually</span>
probs = torch.softmax(logits, dim=1)</code></pre>

<strong>Key Takeaway:</strong> Models output logits; loss functions apply the activation. This separation improves stability, efficiency, and training dynamics.

---

## What are the <code>Dataset</code> and <code>DataLoader</code> classes for?
*Basic · id 1760238724096*

<h1><strong style="font-size: 20px;">Purpose:</strong><span style="font-size: 20px;"> </span><span style="font-size: 20px;">PyTorch classes that provide a standardized interface for loading and iterating over data during training/evaluation.</span></h1><strong>Dataset Class:</strong> Represents the data itself and defines how to access individual samples.
<strong>Key Methods:</strong>
<ul><li><code>__len__()</code>: Returns total number of samples</li><li><code>__getitem__(idx)</code>: Returns a single sample (features, label) at index <code>idx</code></li></ul>

python
<pre><code>class CustomDataset(Dataset):
    def __init__(self, data, labels):
        self.data = data
        self.labels = labels
    
    def __len__(self):
        return len(self.data)
    
    def __getitem__(self, idx):
        return self.data[idx], self.labels[idx]</code></pre>

<strong>DataLoader Class:</strong> Wraps a Dataset and provides batching, shuffling, parallel loading, and more.
<strong>Key Features:</strong>
<ul><li><strong>Batching</strong>: Groups samples into batches automatically</li><li><strong>Shuffling</strong>: Randomizes data order each epoch</li><li><strong>Parallel Loading</strong>: Uses multiple workers to load data asynchronously</li><li><strong>Automatic Collation</strong>: Combines samples into batches (with <code>collate_fn</code>)</li></ul>

python
<pre><code>dataloader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True,
    num_workers=4,
    pin_memory=True  <span style="font-style: italic;"># Faster GPU transfer</span>
)

for batch_features, batch_labels in dataloader:
    <span style="font-style: italic;"># batch_features: [32, ...], batch_labels: [32]</span>
    outputs = model(batch_features)</code></pre>

<strong>Benefits:</strong>
<ul><li><strong>Separation of Concerns</strong>: Dataset handles data access; DataLoader handles iteration</li><li><strong>Memory Efficiency</strong>: Loads data on-demand, not all at once</li><li><strong>Performance</strong>: Multi-process loading prevents I/O bottlenecks</li><li><strong>Flexibility</strong>: Easy to swap datasets or change loading parameters</li></ul><strong>Common Use:</strong> Dataset = "what the data is", DataLoader = "how to iterate over it"

---

## Why are <code>model.train()</code> and <code>model.eval()</code> necessary?
*Basic · id 1760239665437*

<strong>Purpose:</strong> These methods switch the model between training and evaluation modes, changing the behavior of certain layers.

<strong>Layers Affected:</strong>
<ol><li><strong>Dropout</strong>:<ul><li><code>train()</code>: Randomly drops neurons (e.g., with p=0.5)</li><li><code>eval()</code>: Disables dropout, uses all neurons</li><li>Why: Dropout is a regularization technique only needed during training</li></ul></li><li><strong>Batch Normalization</strong>:<ul><li><code>train()</code>: Uses batch statistics (mean/std of current batch) and updates running statistics</li><li><code>eval()</code>: Uses running statistics accumulated during training (fixed)</li><li>Why: Test batches may be small or single samples; batch stats would be unreliable</li></ul></li><li><strong>Other Layers</strong>: Layer Normalization variants, DropPath, Stochastic Depth, etc.</li></ol><strong>Example Usage:</strong>

python
<pre><code><span style="font-style: italic;"># Training</span>
model.train()
for data, labels in train_loader:
    optimizer.zero_grad()
    outputs = model(data)  <span style="font-style: italic;"># Dropout active, BN uses batch stats</span>
    loss = criterion(outputs, labels)
    loss.backward()
    optimizer.step()

<span style="font-style: italic;"># Evaluation</span>
model.eval()
with torch.no_grad():
    for data, labels in test_loader:
        outputs = model(data)  <span style="font-style: italic;"># Dropout off, BN uses running stats</span>
        <span style="font-style: italic;"># compute metrics</span></code></pre>

<strong>Important:</strong>
<ul><li><code>model.eval()</code> does NOT disable gradient computation (use <code>torch.no_grad()</code> for that)</li><li>Forgetting <code>model.eval()</code> during inference causes incorrect predictions due to dropout/BN behavior</li><li>Always pair: <code>model.eval() + torch.no_grad()</code> for inference</li></ul><strong>Key Takeaway:</strong> Controls layer behavior to ensure proper training (with regularization) and deterministic, accurate inference (without randomness).

---

## Why is <code>optimizer.zero_grad()</code> necessary?
*Basic · id 1760239760483*

<strong>Purpose:</strong> Clears (zeros out) the gradients of all parameters tracked by the optimizer before computing new gradients.

<strong>Why It's Necessary:</strong>
<strong>PyTorch accumulates gradients by default</strong> - calling <code>.backward()</code> <em>adds</em> gradients to existing <code>.grad</code> attributes rather than replacing them.

python
<pre><code><span style="font-style: italic;"># Without zero_grad()</span>
loss1.backward()  <span style="font-style: italic;"># param.grad = grad1</span>
loss2.backward()  <span style="font-style: italic;"># param.grad = grad1 + grad2 ❌</span>

<span style="font-style: italic;"># With zero_grad()</span>
loss1.backward()       <span style="font-style: italic;"># param.grad = grad1</span>
optimizer.zero_grad()  <span style="font-style: italic;"># param.grad = 0</span>
loss2.backward()       <span style="font-style: italic;"># param.grad = grad2 ✓</span></code></pre>

<strong>Standard Training Loop:</strong>

python
<pre><code>for data, labels in dataloader:
    optimizer.zero_grad()        <span style="font-style: italic;"># 1. Clear old gradients</span>
    
    outputs = model(data)        <span style="font-style: italic;"># 2. Forward pass</span>
    loss = criterion(outputs, labels)
    
    loss.backward()              <span style="font-style: italic;"># 3. Compute gradients</span>
    optimizer.step()             <span style="font-style: italic;"># 4. Update parameters</span></code></pre>

<strong>When Gradient Accumulation IS Desired:</strong>

python
<pre><code><span style="font-style: italic;"># Accumulate gradients over multiple batches (for large effective batch size)</span>
for i, (data, labels) in enumerate(dataloader):
    outputs = model(data)
    loss = criterion(outputs, labels) / accumulation_steps
    loss.backward()  <span style="font-style: italic;"># Accumulate gradients</span>
    
    if (i + 1) % accumulation_steps == 0:
        optimizer.step()
        optimizer.zero_grad()  <span style="font-style: italic;"># Clear after update</span></code></pre>

<strong>Common Mistake:</strong> Forgetting <code>zero_grad()</code> leads to:
<ul><li>Gradients from multiple batches mixing incorrectly</li><li>Exploding gradients</li><li>Model failing to converge</li></ul><strong>Key Takeaway:</strong> Call <code>optimizer.zero_grad()</code> before each backward pass to ensure clean gradient computation, unless intentionally accumulating gradients.

---

## How do you store and load `PyTorch` models?
*Basic · id 1760247683102*

<strong>1. Save/Load State Dictionary (RECOMMENDED)</strong> Saves only the model parameters (weights and biases), not the model architecture.

python
<pre><code><span style="font-style: italic;"># Saving</span>
torch.save(model.state_dict(), 'model_weights.pth')

<span style="font-style: italic;"># Loading</span>
model = MyModel()  <span style="font-style: italic;"># Must instantiate architecture first</span>
model.load_state_dict(torch.load('model_weights.pth'))
model.eval()  <span style="font-style: italic;"># Set to evaluation mode</span></code></pre>

<strong>2. Save/Load Entire Model</strong> Saves both architecture and parameters (less flexible, not recommended).

python
<pre><code><span style="font-style: italic;"># Saving</span>
torch.save(model, 'entire_model.pth')

<span style="font-style: italic;"># Loading</span>
model = torch.load('entire_model.pth')
model.eval()</code></pre>

<strong>Best Practice - Save Checkpoint with Extra Info:</strong>

python
<pre><code><span style="font-style: italic;"># Saving</span>
torch.save({
    'epoch': epoch,
    'model_state_dict': model.state_dict(),
    'optimizer_state_dict': optimizer.state_dict(),
    'loss': loss,
    'hyperparameters': config
}, 'checkpoint.pth')

<span style="font-style: italic;"># Loading</span>
checkpoint = torch.load('checkpoint.pth')
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']</code></pre>

<strong>Important Considerations:</strong>
<ul><li><strong>Device Mapping</strong>: When loading on different device:</li></ul>

python
<pre><code>  <span style="font-style: italic;"># Load model trained on GPU to CPU</span>
  model.load_state_dict(torch.load('model.pth', map_location='cpu'))
  
  <span style="font-style: italic;"># Load to specific GPU</span>
  model.load_state_dict(torch.load('model.pth', map_location='cuda:0'))</code></pre>

<ul><li><strong>File Extension</strong>: <code>.pth</code> or <code>.pt</code> are conventional</li><li><strong>Why state_dict is preferred</strong>:<ul><li>More portable and flexible</li><li>Survives code refactoring</li><li>Smaller file size</li><li>Compatible across PyTorch versions</li></ul></li></ul><strong>Key Takeaway:</strong> Use <code>state_dict()</code> for production; save optimizer state for resuming training; use <code>map_location</code> for device compatibility.

---

## How does using `PyTorch` work on a GPU? How can you train on multiple GPUs?
*Basic · id 1760247932518*

<strong>Single GPU Usage:</strong>
Move model and data to GPU using <code>.to()</code> or <code>.cuda()</code>:

python
<pre><code>device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

<span style="font-style: italic;"># Move model to GPU</span>
model = MyModel().to(device)

<span style="font-style: italic;"># Move data to GPU in training loop</span>
for data, labels in dataloader:
    data = data.to(device)
    labels = labels.to(device)
    
    outputs = model(data)  <span style="font-style: italic;"># Computation happens on GPU</span>
    loss = criterion(outputs, labels)

<strong>Multi-GPU Training Approaches:</strong>
<strong>1. DataParallel (Simple but Limited)</strong>
<ul><li>Splits batch across GPUs, replicates model on each</li><li>Single-process, limited scalability</li></ul>

python
<pre><code>model = nn.DataParallel(model)
model.to(device)
<span style="font-style: italic;"># Use normally - PyTorch handles the rest</span></code></pre>

<strong>2. DistributedDataParallel (RECOMMENDED)</strong>
<ul><li>Multi-process, one GPU per process</li><li>Better performance and scaling</li><li>More efficient gradient synchronization</li></ul>

python
<pre><code><span style="font-style: italic;"># Basic setup</span>
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

<span style="font-style: italic;"># Initialize process group</span>
dist.init_process_group(backend='nccl')
local_rank = int(os.environ['LOCAL_RANK'])

<span style="font-style: italic;"># Move model to specific GPU</span>
model = MyModel().to(local_rank)
model = DDP(model, device_ids=[local_rank])

<span style="font-style: italic;"># Use DistributedSampler for data</span>
sampler = DistributedSampler(dataset)
dataloader = DataLoader(dataset, sampler=sampler)</code></pre>

<strong>Launch DDP with:</strong>

bash
<pre><code>torchrun --nproc_per_node=4 train.py
<span style="font-style: italic;"># or</span>
python -m torch.distributed.launch --nproc_per_node=4 train.py</code></pre>

<strong>3. Other Strategies:</strong>
<ul><li><strong>Model Parallelism</strong>: Split model across GPUs (for models too large for one GPU)</li><li><strong>Pipeline Parallelism</strong>: Different layers on different GPUs</li><li><strong>FSDP</strong> (Fully Sharded Data Parallel): For very large models, shards parameters across GPUs</li></ul><strong>Key Differences:</strong>
<pre><table><tbody><tr><th>Feature</th><th>DataParallel</th><th>DistributedDataParallel</th></tr><tr><td>Processes</td><td>Single</td><td>Multiple</td></tr><tr><td>Performance</td><td>Good</td><td>Better</td></tr><tr><td>Scalability</td><td>Limited</td><td>Excellent</td></tr><tr><td>Multi-node</td><td>No</td><td>Yes</td></tr></tbody></table></pre><strong>Memory Considerations:</strong>
<ul><li>GPU memory freed with <code>del tensor</code> or <code>torch.cuda.empty_cache()</code></li><li>Monitor with <code>torch.cuda.memory_allocated()</code></li></ul><strong>Key Takeaway:</strong> Single GPU: use <code>.to(device)</code>. Multiple GPUs: prefer DDP over DataParallel for better performance and scalability.

</code></pre>

---

## What is the best practice line for making `PyTorch` code resilient on device it runs on?
*Basic · id 1760248006207*

device = torch.device("cude" if torch.cuda.is_available() else "cpu")

# .to(device)

---

## What is the intuition behind a <code>torch.nn.Embedding</code> layer? How does it relate to a one-hot encoding?
*Basic · id 1760310269942*

An embedding layer maps discrete tokens (words, categories, IDs) to dense, continuous vector representations. It's a <strong>learnable lookup table</strong> where each token has an associated vector.

Embedding is mathematically equivalent to:
<ol><li>One-hot encoding the input</li><li>Multiplying by a weight matrix</li></ol><pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Manual process (inefficient)</span>
vocab_size <span style="color: rgb(64, 120, 242);">=</span> <span style="color: rgb(183, 107, 1);">10000</span>
one_hot <span style="color: rgb(64, 120, 242);">=</span> F<span style="color: rgb(56, 58, 66);">.</span>one_hot<span style="color: rgb(56, 58, 66);">(</span>token_id<span style="color: rgb(56, 58, 66);">,</span> num_classes<span style="color: rgb(64, 120, 242);">=</span>vocab_size<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># [vocab_size]</span>
weight_matrix <span style="color: rgb(64, 120, 242);">=</span> embedding<span style="color: rgb(56, 58, 66);">.</span>weight  <span style="color: rgb(160, 161, 167); font-style: italic;"># [vocab_size, embedding_dim]</span>
result <span style="color: rgb(64, 120, 242);">=</span> one_hot @ weight_matrix  <span style="color: rgb(160, 161, 167); font-style: italic;"># [embedding_dim]</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Embedding layer (efficient)</span>
result <span style="color: rgb(64, 120, 242);">=</span> embedding<span style="color: rgb(56, 58, 66);">(</span>token_id<span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Direct lookup, same result!

</span><strong></strong>
<ol><li><strong>Efficiency</strong>:<ul><li>One-hot: stores sparse vector of size <code>vocab_size</code> (e.g., 10,000 dimensions)</li><li>Embedding: direct lookup, returns dense vector (e.g., 300 dimensions)</li><li>No matrix multiplication needed</li></ul></li><li><strong>Learnable Representations</strong>:<ul><li>Embedding vectors are learned during training</li><li>Similar tokens learn similar vectors (semantic meaning captured)</li><li>One-hot has no notion of similarity (all tokens equally distant)</li></ul></li><li><strong>Memory</strong>:<ul><li>Stores only <code>vocab_size × embedding_dim</code> parameters</li><li>vs one-hot + linear layer: same parameters but slower computation</li></ul></li></ol><strong>Key Takeaway:</strong> Embeddings are efficient, learnable lookup tables that convert discrete tokens to dense vectors. They're equivalent to one-hot encoding + linear layer but much faster via direct indexing.<span style="color: rgb(160, 161, 167); font-style: italic;">
</span></code></pre>

---

## Explain <code>register_buffer()</code> in <code>PyTorch</code>!
*Basic · id 1760406945734*

<strong>Definition:</strong> <code>register_buffer()</code> registers a tensor as part of the model's state that should be saved/loaded but <strong>not</strong> trained (no gradients computed).
<strong>Basic Usage:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">MyModel</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>fc <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Linear<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">5</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Trainable parameter</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Register buffer - not trainable</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_mean'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">5</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Use buffer like any tensor</span>
        x <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>fc<span style="color: rgb(56, 58, 66);">(</span>x<span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(166, 38, 164);">return</span> x <span style="color: rgb(64, 120, 242);">-</span> self<span style="color: rgb(56, 58, 66);">.</span>running_mean</code></pre>

<strong>Key Characteristics:</strong>
<strong>1. Not a Parameter (No Gradients):</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Parameters (trainable)</span>
<span style="color: rgb(80, 161, 79);">list</span><span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">.</span>parameters<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Contains fc.weight, fc.bias</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Buffers (not trainable)</span>
<span style="color: rgb(80, 161, 79);">list</span><span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">.</span>buffers<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Contains running_mean</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># All tensors</span>
model<span style="color: rgb(56, 58, 66);">.</span>state_dict<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Contains both parameters AND buffers</span></code></pre>

<strong>2. Saved/Loaded with Model:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Saving</span>
torch<span style="color: rgb(56, 58, 66);">.</span>save<span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">.</span>state_dict<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">'model.pth'</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Saves both parameters AND buffers</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Loading</span>
model<span style="color: rgb(56, 58, 66);">.</span>load_state_dict<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>load<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'model.pth'</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># Restores both parameters AND buffers</span></code></pre>

<strong>3. Moved with Model:</strong>

python
<pre><code>model<span style="color: rgb(56, 58, 66);">.</span>to<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'cuda'</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># Moves parameters AND buffers to GPU</span>
model<span style="color: rgb(56, 58, 66);">.</span>to<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'cpu'</span><span style="color: rgb(56, 58, 66);">)</span>   <span style="color: rgb(160, 161, 167); font-style: italic;"># Moves everything back to CPU</span></code></pre>

<strong>When to Use Buffers:</strong>
<strong>1. Running Statistics (BatchNorm):</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">BatchNorm</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Trainable parameters</span>
        self<span style="color: rgb(56, 58, 66);">.</span>weight <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>bias <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Buffers for running statistics</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_mean'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_var'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>num_features<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'num_batches_tracked'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">0</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>2. Fixed Masks/Constants:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">CausalAttention</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> max_len<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Causal mask doesn't need gradients</span>
        mask <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>triu<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span>max_len<span style="color: rgb(56, 58, 66);">,</span> max_len<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> diagonal<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span><span style="color: rgb(80, 161, 79);">bool</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'causal_mask'</span><span style="color: rgb(56, 58, 66);">,</span> mask<span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Use pre-computed mask</span>
        scores <span style="color: rgb(64, 120, 242);">=</span> scores<span style="color: rgb(56, 58, 66);">.</span>masked_fill<span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">.</span>causal_mask<span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">float</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'-inf'</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>3. Positional Encodings:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">PositionalEncoding</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> d_model<span style="color: rgb(56, 58, 66);">,</span> max_len<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Pre-compute sinusoidal encodings</span>
        pe <span style="color: rgb(64, 120, 242);">=</span> self<span style="color: rgb(56, 58, 66);">.</span>_create_positional_encoding<span style="color: rgb(56, 58, 66);">(</span>d_model<span style="color: rgb(56, 58, 66);">,</span> max_len<span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'pe'</span><span style="color: rgb(56, 58, 66);">,</span> pe<span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(166, 38, 164);">return</span> x <span style="color: rgb(64, 120, 242);">+</span> self<span style="color: rgb(56, 58, 66);">.</span>pe<span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(56, 58, 66);">:</span>x<span style="color: rgb(56, 58, 66);">.</span>size<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">1</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">]</span></code></pre>

<strong>Buffer vs Parameter vs Regular Tensor:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">ComparisonModel</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># 1. Parameter - trainable, saved, moved with model</span>
        self<span style="color: rgb(56, 58, 66);">.</span>weight <span style="color: rgb(64, 120, 242);">=</span> nn<span style="color: rgb(56, 58, 66);">.</span>Parameter<span style="color: rgb(56, 58, 66);">(</span>torch<span style="color: rgb(56, 58, 66);">.</span>randn<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># 2. Buffer - NOT trainable, saved, moved with model</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'mask'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>ones<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        
        <span style="color: rgb(160, 161, 167); font-style: italic;"># 3. Regular attribute - NOT trainable, NOT saved, NOT moved</span>
        self<span style="color: rgb(56, 58, 66);">.</span>temp <span style="color: rgb(64, 120, 242);">=</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">10</span><span style="color: rgb(56, 58, 66);">)</span>

model <span style="color: rgb(64, 120, 242);">=</span> ComparisonModel<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Moving to GPU</span>
model<span style="color: rgb(56, 58, 66);">.</span>to<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'cuda'</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># weight: ✓ moved to cuda</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># mask: ✓ moved to cuda</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># temp: ✗ still on cpu!</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Saving</span>
torch<span style="color: rgb(56, 58, 66);">.</span>save<span style="color: rgb(56, 58, 66);">(</span>model<span style="color: rgb(56, 58, 66);">.</span>state_dict<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(80, 161, 79);">'model.pth'</span><span style="color: rgb(56, 58, 66);">)</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># weight: ✓ saved</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># mask: ✓ saved</span>
<span style="color: rgb(160, 161, 167); font-style: italic;"># temp: ✗ NOT saved</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Gradients</span>
model<span style="color: rgb(56, 58, 66);">.</span>weight<span style="color: rgb(56, 58, 66);">.</span>requires_grad  <span style="color: rgb(160, 161, 167); font-style: italic;"># True</span>
model<span style="color: rgb(56, 58, 66);">.</span>mask<span style="color: rgb(56, 58, 66);">.</span>requires_grad    <span style="color: rgb(160, 161, 167); font-style: italic;"># False</span>
model<span style="color: rgb(56, 58, 66);">.</span>temp<span style="color: rgb(56, 58, 66);">.</span>requires_grad    <span style="color: rgb(160, 161, 167); font-style: italic;"># False (if even still exists)</span></code></pre>

<strong>Persistent vs Non-Persistent Buffers:</strong>

python
<pre><code><span style="color: rgb(160, 161, 167); font-style: italic;"># Persistent buffer (default) - saved in state_dict</span>
self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'running_mean'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">5</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> persistent<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">True</span><span style="color: rgb(56, 58, 66);">)</span>

<span style="color: rgb(160, 161, 167); font-style: italic;"># Non-persistent buffer - NOT saved in state_dict, but still moved with model</span>
self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'temp_cache'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>zeros<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(183, 107, 1);">5</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">,</span> persistent<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(183, 107, 1);">False</span><span style="color: rgb(56, 58, 66);">)</span></code></pre>

<strong>Common Use Cases Summary:</strong>
<pre><table><tbody><tr><th>Use Case</th><th>Example</th><th>Why Buffer?</th></tr><tr><td>Running stats</td><td>BatchNorm mean/var</td><td>Need to save, not train</td></tr><tr><td>Fixed masks</td><td>Causal attention mask</td><td>Computed once, reused</td></tr><tr><td>Positional encodings</td><td>Sinusoidal PE</td><td>Pre-computed constants</td></tr><tr><td>Normalization constants</td><td>Dataset mean/std</td><td>Fixed preprocessing values</td></tr></tbody></table></pre><strong>Practical Example - Normalization Layer:</strong>

python
<pre><code><span style="color: rgb(166, 38, 164);">class</span> <span style="color: rgb(183, 107, 1);">NormalizeLayer</span><span style="color: rgb(56, 58, 66);">(</span>nn<span style="color: rgb(56, 58, 66);">.</span>Module<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">__init__</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> mean<span style="color: rgb(56, 58, 66);">,</span> std<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(80, 161, 79);">super</span><span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">.</span>__init__<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(56, 58, 66);">)</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Register as buffers so they move with model</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'mean'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span>mean<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
        self<span style="color: rgb(56, 58, 66);">.</span>register_buffer<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'std'</span><span style="color: rgb(56, 58, 66);">,</span> torch<span style="color: rgb(56, 58, 66);">.</span>tensor<span style="color: rgb(56, 58, 66);">(</span>std<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">)</span>
    
    <span style="color: rgb(166, 38, 164);">def</span> <span style="color: rgb(64, 120, 242);">forward</span><span style="color: rgb(56, 58, 66);">(</span>self<span style="color: rgb(56, 58, 66);">,</span> x<span style="color: rgb(56, 58, 66);">)</span><span style="color: rgb(56, 58, 66);">:</span>
        <span style="color: rgb(160, 161, 167); font-style: italic;"># Normalize using stored statistics</span>
        <span style="color: rgb(166, 38, 164);">return</span> <span style="color: rgb(56, 58, 66);">(</span>x <span style="color: rgb(64, 120, 242);">-</span> self<span style="color: rgb(56, 58, 66);">.</span>mean<span style="color: rgb(56, 58, 66);">)</span> <span style="color: rgb(64, 120, 242);">/</span> self<span style="color: rgb(56, 58, 66);">.</span>std

<span style="color: rgb(160, 161, 167); font-style: italic;"># Usage</span>
model <span style="color: rgb(64, 120, 242);">=</span> NormalizeLayer<span style="color: rgb(56, 58, 66);">(</span>mean<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.485</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.456</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.406</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">,</span> 
                       std<span style="color: rgb(64, 120, 242);">=</span><span style="color: rgb(56, 58, 66);">[</span><span style="color: rgb(183, 107, 1);">0.229</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.224</span><span style="color: rgb(56, 58, 66);">,</span> <span style="color: rgb(183, 107, 1);">0.225</span><span style="color: rgb(56, 58, 66);">]</span><span style="color: rgb(56, 58, 66);">)</span>
model<span style="color: rgb(56, 58, 66);">.</span>to<span style="color: rgb(56, 58, 66);">(</span><span style="color: rgb(80, 161, 79);">'cuda'</span><span style="color: rgb(56, 58, 66);">)</span>  <span style="color: rgb(160, 161, 167); font-style: italic;"># mean and std automatically moved to GPU</span></code></pre>

<strong>Key Takeaway:</strong> <code>register_buffer()</code> creates tensors that are part of the model's state (saved/loaded and moved with the model) but are <strong>not trainable parameters</strong>. Essential for running statistics, fixed masks, and pre-computed constants that need to persist with the model.

---

## How does the @ operator work on <code>PyTorch</code>? How does it work for <code>len(shape) > 2</code>​?
*Basic · id 1760409360803*

<strong>Definition:</strong> The <code>@</code> operator performs matrix multiplication in PyTorch (equivalent to <code>torch.matmul()</code>).
<strong>Basic Cases:</strong>
<strong>1. 2D × 2D (Standard Matrix Multiplication):</strong>

python
<pre><code>A = torch.randn(3, 4)  <span style="font-style: italic;"># [3, 4]</span>
B = torch.randn(4, 5)  <span style="font-style: italic;"># [4, 5]</span>
C = A @ B              <span style="font-style: italic;"># [3, 5]</span>

<span style="font-style: italic;"># Rule: (m, n) @ (n, p) = (m, p)</span>
<span style="font-style: italic;"># Inner dimensions must match!</span></code></pre>

<strong>2. 1D × 1D (Dot Product):</strong>

python
<pre><code>a = torch.randn(5)  <span style="font-style: italic;"># [5]</span>
b = torch.randn(5)  <span style="font-style: italic;"># [5]</span>
c = a @ b           <span style="font-style: italic;"># scalar</span>

<span style="font-style: italic;"># Equivalent to: sum(a * b)</span></code></pre>

<strong>3. 2D × 1D (Matrix-Vector Product):</strong>

python
<pre><code>A = torch.randn(3, 4)  <span style="font-style: italic;"># [3, 4]</span>
b = torch.randn(4)     <span style="font-style: italic;"># [4]</span>
c = A @ b              <span style="font-style: italic;"># [3]</span>

<span style="font-style: italic;"># Treats b as column vector</span></code></pre>

<strong>Higher-Dimensional Behavior (len(shape) > 2):</strong>
<strong>Key Rule:</strong> The <code>@</code> operator treats the last two dimensions as matrices and performs <strong>batched matrix multiplication</strong> on them, while broadcasting over leading dimensions.
<strong>4. 3D × 3D (Batched Matrix Multiplication):</strong>

python
<pre><code>A = torch.randn(10, 3, 4)  <span style="font-style: italic;"># [batch, 3, 4]</span>
B = torch.randn(10, 4, 5)  <span style="font-style: italic;"># [batch, 4, 5]</span>
C = A @ B                  <span style="font-style: italic;"># [batch, 3, 5]</span>

<span style="font-style: italic;"># Performs 10 separate matrix multiplications:</span>
<span style="font-style: italic;"># C[i] = A[i] @ B[i] for i in range(10)</span></code></pre>

<strong>5. Broadcasting with Batch Dimensions:</strong>

python
<pre><code><span style="font-style: italic;"># Different batch sizes - broadcasts like other PyTorch ops</span>
A = torch.randn(2, 1, 3, 4)   <span style="font-style: italic;"># [2, 1, 3, 4]</span>
B = torch.randn(1, 5, 4, 6)   <span style="font-style: italic;"># [1, 5, 4, 6]</span>
C = A @ B                     <span style="font-style: italic;"># [2, 5, 3, 6]</span>

<span style="font-style: italic;"># Broadcasting rules:</span>
<span style="font-style: italic;"># - Last 2 dims: matrix multiply (3,4) @ (4,6) = (3,6)</span>
<span style="font-style: italic;"># - Leading dims: broadcast (2,1) and (1,5) → (2,5)</span></code></pre>

<strong>Real-World Example - Transformer Attention:</strong>

python
<pre><code><span style="font-style: italic;"># Q, K, V in multi-head attention</span>
batch_size = 32
num_heads = 8
seq_len = 100
head_dim = 64

Q = torch.randn(batch_size, num_heads, seq_len, head_dim)  <span style="font-style: italic;"># [32, 8, 100, 64]</span>
K = torch.randn(batch_size, num_heads, seq_len, head_dim)  <span style="font-style: italic;"># [32, 8, 100, 64]</span>
V = torch.randn(batch_size, num_heads, seq_len, head_dim)  <span style="font-style: italic;"># [32, 8, 100, 64]</span>

<span style="font-style: italic;"># Compute attention scores: Q @ K^T</span>
scores = Q @ K.transpose(-2, -1)  <span style="font-style: italic;"># [32, 8, 100, 100]</span>
<span style="font-style: italic;"># For each of 32 batches and 8 heads:</span>
<span style="font-style: italic;">#   (100, 64) @ (64, 100) = (100, 100)</span>

<span style="font-style: italic;"># Apply attention to values</span>
attention_weights = torch.softmax(scores, dim=-1)  <span style="font-style: italic;"># [32, 8, 100, 100]</span>
output = attention_weights @ V  <span style="font-style: italic;"># [32, 8, 100, 64]</span>
<span style="font-style: italic;"># For each batch and head:</span>
<span style="font-style: italic;">#   (100, 100) @ (100, 64) = (100, 64)</span></code></pre>

<strong>Dimension Rules Summary:</strong>

python
<pre><code><span style="font-style: italic;"># Last 2 dimensions: matrix multiplication</span>
<span style="font-style: italic;"># Earlier dimensions: batch dimensions (broadcasted)</span>

Shape compatibility:
(..., m, n) @ (..., n, p) = (..., m, p)
     ↑____↑        ↑____↑
     must match

Examples:
[10, 3, 4] @ [10, 4, 5] = [10, 3, 5]        ✓
[2, 3, 4] @ [4, 5] = [2, 3, 5]              ✓ (broadcasts)
[5, 1, 3, 4] @ [1, 7, 4, 6] = [5, 7, 3, 6]  ✓ (broadcasts)
[3, 4] @ [5, 6] = ERROR                      ✗ (4 ≠ 5)</code></pre>

<strong>Common Patterns:</strong>
<strong>1. Batch of Matrix-Vector Products:</strong>

python
<pre><code>A = torch.randn(100, 3, 4)  <span style="font-style: italic;"># 100 matrices of size [3, 4]</span>
x = torch.randn(100, 4, 1)  <span style="font-style: italic;"># 100 vectors of size [4]</span>
y = A @ x                   <span style="font-style: italic;"># [100, 3, 1]</span></code></pre>

<strong>2. Single Matrix with Batch of Vectors:</strong>

python
<pre><code>A = torch.randn(3, 4)       <span style="font-style: italic;"># Single matrix [3, 4]</span>
X = torch.randn(100, 4, 1)  <span style="font-style: italic;"># 100 vectors [100, 4, 1]</span>
Y = A @ X                   <span style="font-style: italic;"># [100, 3, 1] - broadcasts A</span></code></pre>

<strong>3. Einstein Notation Alternative:</strong>

python
<pre><code><span style="font-style: italic;"># @ operator</span>
C = A @ B

<span style="font-style: italic;"># Equivalent with einsum (more explicit)</span>
C = torch.einsum('...ij,...jk->...ik', A, B)</code></pre>

<strong>Comparison with Other Operations:</strong>

python
<pre><code>A = torch.randn(2, 3, 4)
B = torch.randn(2, 4, 5)

<span style="font-style: italic;"># Matrix multiplication</span>
A @ B              <span style="font-style: italic;"># [2, 3, 5] - last 2 dims multiply</span>

<span style="font-style: italic;"># Element-wise multiplication</span>
A * B              <span style="font-style: italic;"># Broadcasting error (shapes don't match)</span>

<span style="font-style: italic;"># Batch matrix multiply (explicit)</span>
torch.bmm(A, B)    <span style="font-style: italic;"># [2, 3, 5] - requires 3D exactly</span>

<span style="font-style: italic;"># General matmul (most flexible)</span>
torch.matmul(A, B) <span style="font-style: italic;"># [2, 3, 5] - same as @</span></code></pre>

<strong>Key Differences:</strong>
<pre><table><tbody><tr><th>Operation</th><th>Dimensions</th><th>Broadcasting</th><th>Use Case</th></tr><tr><td><code>@</code></td><td>Any (≥1D)</td><td>Yes</td><td>General purpose</td></tr><tr><td><code>torch.mm</code></td><td>Exactly 2D</td><td>No</td><td>Simple matrices only</td></tr><tr><td><code>torch.bmm</code></td><td>Exactly 3D</td><td>No</td><td>Batched, no broadcasting</td></tr><tr><td><code>torch.matmul</code></td><td>Any</td><td>Yes</td><td>Same as @</td></tr></tbody></table></pre><strong>Key Takeaway:</strong> The <code>@</code> operator performs matrix multiplication on the <strong>last two dimensions</strong> while treating all earlier dimensions as batch dimensions that follow broadcasting rules. Essential for batched operations in neural networks, especially in attention mechanisms.

---

## How does <code>.view</code> transform tensors in <code>PyTorch</code>? Specifically how does it work for <code>len(shape) > 2</code>
*Basic · id 1760409532653*

<strong>Definition:</strong> <code>.view()</code> reshapes a tensor without copying data. It returns a new view with different dimensions but sharing the same underlying memory.
<strong>Basic Principle:</strong> Elements are stored in <strong>row-major (C-contiguous) order</strong> in memory. <code>.view()</code> reinterprets this flat sequence with new dimensions.
<strong>Simple Examples:</strong>

python
<pre><code><span style="font-style: italic;"># 1D to 2D</span>
x = torch.tensor([1, 2, 3, 4, 5, 6])  <span style="font-style: italic;"># [6]</span>
y = x.view(2, 3)  <span style="font-style: italic;"># [2, 3]</span>
<span style="font-style: italic;"># [[1, 2, 3],</span>
<span style="font-style: italic;">#  [4, 5, 6]]</span>

<span style="font-style: italic;"># 2D to 1D (flatten)</span>
z = y.view(-1)  <span style="font-style: italic;"># [6] - same as x</span>
<span style="font-style: italic;"># [1, 2, 3, 4, 5, 6]</span>

<span style="font-style: italic;"># Using -1 for automatic dimension inference</span>
x = torch.randn(24)
y = x.view(4, -1)  <span style="font-style: italic;"># [4, 6] - PyTorch infers 6</span></code></pre>

<strong>How It Works (Memory Layout):</strong>

python
<pre><code><span style="font-style: italic;"># Memory is always a flat 1D array</span>
x = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])  <span style="font-style: italic;"># Shape: [2, 3]</span>

<span style="font-style: italic;"># In memory: [1, 2, 3, 4, 5, 6]</span>
<span style="font-style: italic;">#             ↑row 0↑  ↑row 1↑</span>

<span style="font-style: italic;"># Reshape to [3, 2]</span>
y = x.view(3, 2)
<span style="font-style: italic;"># [[1, 2],    ← elements 0-1</span>
<span style="font-style: italic;">#  [3, 4],    ← elements 2-3</span>
<span style="font-style: italic;">#  [5, 6]]    ← elements 4-5</span>

<span style="font-style: italic;"># Same memory, different interpretation!</span></code></pre>

<strong>Higher Dimensions (len(shape) > 2):</strong>
<strong>Key Insight:</strong> The <strong>rightmost dimensions vary fastest</strong> in memory (row-major order).

python
<pre><code><span style="font-style: italic;"># 3D tensor</span>
x = torch.arange(24).view(2, 3, 4)  <span style="font-style: italic;"># [2, 3, 4]</span>
<span style="font-style: italic;"># Shape: [batch=2, rows=3, cols=4]</span>

<span style="font-style: italic;"># Memory layout:</span>
<span style="font-style: italic;"># [0,1,2,3,  4,5,6,7,  8,9,10,11,  12,13,14,15,  16,17,18,19,  20,21,22,23]</span>
<span style="font-style: italic;">#  ↑batch 0, row 0↑  ↑b0,r1↑  ↑b0,r2↑  ↑batch 1, row 0↑  ↑b1,r1↑  ↑b1,r2↑</span>

print(x)
<span style="font-style: italic;"># [[[0,  1,  2,  3],</span>
<span style="font-style: italic;">#   [4,  5,  6,  7],</span>
<span style="font-style: italic;">#   [8,  9,  10, 11]],</span>
<span style="font-style: italic;">#</span>
<span style="font-style: italic;">#  [[12, 13, 14, 15],</span>
<span style="font-style: italic;">#   [16, 17, 18, 19],</span>
<span style="font-style: italic;">#   [20, 21, 22, 23]]]</span></code></pre>

<strong>Practical Example - Reshaping in Transformers:</strong>

python
<pre><code><span style="font-style: italic;"># Multi-head attention: reshape for parallel head computation</span>
batch_size = 32
seq_len = 100
d_model = 512
num_heads = 8
head_dim = d_model // num_heads  <span style="font-style: italic;"># 64</span>

<span style="font-style: italic;"># Linear projection output: [32, 100, 512]</span>
Q = torch.randn(batch_size, seq_len, d_model)

<span style="font-style: italic;"># Reshape to separate heads: [32, 100, 8, 64]</span>
Q = Q.view(batch_size, seq_len, num_heads, head_dim)

<span style="font-style: italic;"># Transpose for batched matmul: [32, 8, 100, 64]</span>
Q = Q.transpose(1, 2)

<span style="font-style: italic;"># Now can do batched attention per head!</span></code></pre>

<strong>Complex Reshaping Example:</strong>

python
<pre><code><span style="font-style: italic;"># Start with 4D tensor</span>
x = torch.arange(120).view(2, 3, 4, 5)  <span style="font-style: italic;"># [2, 3, 4, 5]</span>
print(x.shape)  <span style="font-style: italic;"># torch.Size([2, 3, 4, 5])</span>

<span style="font-style: italic;"># Reshape to merge middle dimensions</span>
y = x.view(2, 12, 5)  <span style="font-style: italic;"># [2, 12, 5]</span>
<span style="font-style: italic;"># Groups (3*4=12) middle elements together</span>

<span style="font-style: italic;"># Reshape to split dimensions</span>
z = x.view(2, 3, 2, 2, 5)  <span style="font-style: italic;"># [2, 3, 2, 2, 5]</span>
<span style="font-style: italic;"># Splits dim 2 (size 4) into (2, 2)</span>

<span style="font-style: italic;"># Flatten all but batch dimension</span>
w = x.view(2, -1)  <span style="font-style: italic;"># [2, 60]</span>
<span style="font-style: italic;"># Common pattern: keep batch, flatten rest</span></code></pre>

<strong>Flattening Patterns:</strong>

python
<pre><code><span style="font-style: italic;"># Input image: [batch, channels, height, width]</span>
x = torch.randn(32, 3, 224, 224)  <span style="font-style: italic;"># [32, 3, 224, 224]</span>

<span style="font-style: italic;"># Flatten spatial dimensions</span>
x = x.view(32, 3, -1)  <span style="font-style: italic;"># [32, 3, 50176]</span>

<span style="font-style: italic;"># Flatten everything except batch</span>
x = x.view(32, -1)  <span style="font-style: italic;"># [32, 150528]</span>

<span style="font-style: italic;"># Complete flatten</span>
x = x.view(-1)  <span style="font-style: italic;"># [4816896] - single 1D tensor</span></code></pre>

<strong>Important Constraints:</strong>
<strong>1. Total elements must match:</strong>

python
<pre><code>x = torch.randn(2, 3, 4)  <span style="font-style: italic;"># 24 elements</span>
y = x.view(4, 6)          <span style="font-style: italic;"># ✓ 24 elements</span>
z = x.view(5, 5)          <span style="font-style: italic;"># ✗ Error: 25 ≠ 24</span></code></pre>

<strong>2. Tensor must be contiguous:</strong>

python
<pre><code>x = torch.randn(2, 3, 4)
y = x.transpose(0, 1)     <span style="font-style: italic;"># Non-contiguous!</span>
z = y.view(-1)            <span style="font-style: italic;"># ✗ Error</span>

<span style="font-style: italic;"># Fix: make contiguous first</span>
z = y.contiguous().view(-1)  <span style="font-style: italic;"># ✓ Works</span>
<span style="font-style: italic;"># Or use reshape (handles non-contiguous automatically)</span>
z = y.reshape(-1)         <span style="font-style: italic;"># ✓ Works</span></code></pre>

<strong>Common Patterns in Neural Networks:</strong>
<strong>1. Batch Flattening (CNN → FC):</strong>

python
<pre><code><span style="font-style: italic;"># After conv layers: [batch, channels, H, W]</span>
x = torch.randn(32, 64, 7, 7)
x = x.view(32, -1)  <span style="font-style: italic;"># [32, 3136] for fully connected layer</span></code></pre>

<strong>2. Multi-Head Attention Reshaping:</strong>

python
<pre><code><span style="font-style: italic;"># Split embedding into multiple heads</span>
x = torch.randn(32, 100, 512)  <span style="font-style: italic;"># [batch, seq, embedding]</span>
x = x.view(32, 100, 8, 64)     <span style="font-style: italic;"># [batch, seq, heads, head_dim]</span>
x = x.transpose(1, 2)          <span style="font-style: italic;"># [batch, heads, seq, head_dim]</span></code></pre>

<strong>3. Combining Batch and Sequence:</strong>

python
<pre><code><span style="font-style: italic;"># Process all sequences together</span>
x = torch.randn(32, 10, 512)  <span style="font-style: italic;"># [batch, seq, features]</span>
x = x.view(-1, 512)           <span style="font-style: italic;"># [320, features] - merge batch & seq</span></code></pre>

<strong>.view() vs .reshape() vs .flatten():</strong>

python
<pre><code>x = torch.randn(2, 3, 4)

<span style="font-style: italic;"># .view() - requires contiguous, no copy</span>
y = x.view(2, 12)  <span style="font-style: italic;"># Fails if non-contiguous</span>

<span style="font-style: italic;"># .reshape() - handles non-contiguous, may copy</span>
y = x.reshape(2, 12)  <span style="font-style: italic;"># Always works</span>

<span style="font-style: italic;"># .flatten() - specialized for flattening</span>
y = x.flatten()        <span style="font-style: italic;"># [24] - flatten all dims</span>
y = x.flatten(1)       <span style="font-style: italic;"># [2, 12] - flatten from dim 1 onward</span></code></pre>

<strong>Key Takeaway:</strong> <code>.view()</code> reinterprets tensor memory with new dimensions following row-major order (rightmost dimensions vary fastest). Total elements must match, and tensor must be contiguous. Essential for reshaping in neural networks, especially for batched operations and dimension manipulation in attention mechanisms.

---

## How does <code>.transpose</code> transform tensors in <code>PyTorch</code>? Specifically how does it work for <code>len(shape) > 2</code>
*Basic · id 1760409607500*

<strong>Definition:</strong> <code>.transpose(dim0, dim1)</code> swaps two specified dimensions of a tensor. Returns a view (no data copying) but makes the tensor <strong>non-contiguous</strong>.
<strong>Basic Usage:</strong>

python
<pre><code><span style="font-style: italic;"># 2D transpose (matrix transpose)</span>
x = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])  <span style="font-style: italic;"># [2, 3]</span>

y = x.transpose(0, 1)  <span style="font-style: italic;"># Swap dims 0 and 1</span>
<span style="font-style: italic;"># [[1, 4],</span>
<span style="font-style: italic;">#  [2, 5],</span>
<span style="font-style: italic;">#  [3, 6]]  # [3, 2]</span>

<span style="font-style: italic;"># Shorthand for 2D</span>
y = x.T  <span style="font-style: italic;"># Same as x.transpose(0, 1)</span></code></pre>

<strong>Higher Dimensions (len(shape) > 2):</strong>
<strong>Key Point:</strong> <code>.transpose()</code> <strong>only swaps TWO specific dimensions</strong>, leaving all others unchanged.

python
<pre><code><span style="font-style: italic;"># 3D tensor</span>
x = torch.arange(24).view(2, 3, 4)  <span style="font-style: italic;"># [2, 3, 4]</span>
print(x.shape)  <span style="font-style: italic;"># torch.Size([2, 3, 4])</span>

<span style="font-style: italic;"># Swap dimensions 0 and 1</span>
y = x.transpose(0, 1)  <span style="font-style: italic;"># [3, 2, 4]</span>
<span style="font-style: italic;"># Dimension 0 (size 2) ↔ Dimension 1 (size 3)</span>
<span style="font-style: italic;"># Dimension 2 (size 4) unchanged</span>

<span style="font-style: italic;"># Swap dimensions 1 and 2  </span>
z = x.transpose(1, 2)  <span style="font-style: italic;"># [2, 4, 3]</span>
<span style="font-style: italic;"># Dimension 0 (size 2) unchanged</span>
<span style="font-style: italic;"># Dimension 1 (size 3) ↔ Dimension 2 (size 4)</span>

<span style="font-style: italic;"># Swap dimensions 0 and 2</span>
w = x.transpose(0, 2)  <span style="font-style: italic;"># [4, 3, 2]</span>
<span style="font-style: italic;"># Dimension 0 (size 2) ↔ Dimension 2 (size 4)</span>
<span style="font-style: italic;"># Dimension 1 (size 3) unchanged</span></code></pre>

<strong>Concrete Example with Values:</strong>

python
<pre><code>x = torch.arange(24).view(2, 3, 4)  <span style="font-style: italic;"># [2, 3, 4]</span>
<span style="font-style: italic;"># [[[ 0,  1,  2,  3],</span>
<span style="font-style: italic;">#   [ 4,  5,  6,  7],</span>
<span style="font-style: italic;">#   [ 8,  9, 10, 11]],</span>
<span style="font-style: italic;">#</span>
<span style="font-style: italic;">#  [[12, 13, 14, 15],</span>
<span style="font-style: italic;">#   [16, 17, 18, 19],</span>
<span style="font-style: italic;">#   [20, 21, 22, 23]]]</span>

<span style="font-style: italic;"># Transpose dims 1 and 2 (last two dimensions)</span>
y = x.transpose(1, 2)  <span style="font-style: italic;"># [2, 4, 3]</span>
<span style="font-style: italic;"># [[[ 0,  4,  8],      ← Elements from column 0 of each row</span>
<span style="font-style: italic;">#   [ 1,  5,  9],      ← Elements from column 1</span>
<span style="font-style: italic;">#   [ 2,  6, 10],      ← Elements from column 2</span>
<span style="font-style: italic;">#   [ 3,  7, 11]],     ← Elements from column 3</span>
<span style="font-style: italic;">#</span>
<span style="font-style: italic;">#  [[12, 16, 20],</span>
<span style="font-style: italic;">#   [13, 17, 21],</span>
<span style="font-style: italic;">#   [14, 18, 22],</span>
<span style="font-style: italic;">#   [15, 19, 23]]]</span>

<span style="font-style: italic;"># Original: x[0, 1, 2] = 6</span>
<span style="font-style: italic;"># After transpose: y[0, 2, 1] = 6</span></code></pre>

<strong>Negative Indexing:</strong>

python
<pre><code>x = torch.randn(2, 3, 4, 5)  <span style="font-style: italic;"># [2, 3, 4, 5]</span>

<span style="font-style: italic;"># These are equivalent:</span>
y = x.transpose(2, 3)      <span style="font-style: italic;"># Swap dims 2 and 3</span>
y = x.transpose(-2, -1)    <span style="font-style: italic;"># Swap last two dims (common pattern!)</span>

<span style="font-style: italic;"># Result: [2, 3, 5, 4]</span></code></pre>

<strong>Practical Use Cases:</strong>
<strong>1. Attention Score Computation:</strong>

python
<pre><code><span style="font-style: italic;"># Q: [batch, seq, d_model] → [32, 100, 512]</span>
<span style="font-style: italic;"># K: [batch, seq, d_model] → [32, 100, 512]</span>

<span style="font-style: italic;"># Need K^T for Q @ K^T</span>
K_T = K.transpose(-2, -1)  <span style="font-style: italic;"># [32, 512, 100]</span>
<span style="font-style: italic;"># Swaps last two dimensions (seq and d_model)</span>

scores = Q @ K_T  <span style="font-style: italic;"># [32, 100, 100]</span></code></pre>

<strong>2. Multi-Head Attention Reshaping:</strong>

python
<pre><code><span style="font-style: italic;"># After splitting heads: [batch, seq, heads, head_dim]</span>
x = torch.randn(32, 100, 8, 64)  <span style="font-style: italic;"># [32, 100, 8, 64]</span>

<span style="font-style: italic;"># Transpose to group by head: [batch, heads, seq, head_dim]</span>
x = x.transpose(1, 2)  <span style="font-style: italic;"># [32, 8, 100, 64]</span>
<span style="font-style: italic;"># Now dimension 1 (heads) and dimension 2 (seq) are swapped</span>
<span style="font-style: italic;"># Can now do batched matmul across heads!</span></code></pre>

<strong>3. Image Processing (Channel Permutation):</strong>

python
<pre><code><span style="font-style: italic;"># PyTorch format: [batch, channels, height, width]</span>
x = torch.randn(32, 3, 224, 224)  <span style="font-style: italic;"># [32, 3, 224, 224]</span>

<span style="font-style: italic;"># Convert to channels-last for some operations</span>
x = x.transpose(1, 3)  <span style="font-style: italic;"># [32, 224, 224, 3]</span>
<span style="font-style: italic;"># Only dims 1 and 3 swapped, dims 0 and 2 unchanged</span></code></pre>

<strong>Multiple Transposes:</strong>

python
<pre><code>x = torch.randn(2, 3, 4, 5)  <span style="font-style: italic;"># [2, 3, 4, 5]</span>

<span style="font-style: italic;"># Chain multiple transposes</span>
y = x.transpose(0, 1).transpose(2, 3)  <span style="font-style: italic;"># [3, 2, 5, 4]</span>
<span style="font-style: italic;"># First: swap dims 0,1 → [3, 2, 4, 5]</span>
<span style="font-style: italic;"># Then: swap dims 2,3 → [3, 2, 5, 4]</span></code></pre>

<strong>Contiguity Issue:</strong>

python
<pre><code>x = torch.randn(2, 3, 4)
y = x.transpose(0, 1)  <span style="font-style: italic;"># Non-contiguous!</span>

<span style="font-style: italic;"># This fails:</span>
z = y.view(6, 4)  <span style="font-style: italic;"># ✗ Error: tensor not contiguous</span>

<span style="font-style: italic;"># Solutions:</span>
z = y.contiguous().view(6, 4)  <span style="font-style: italic;"># ✓ Make contiguous first</span>
z = y.reshape(6, 4)             <span style="font-style: italic;"># ✓ reshape handles it automatically</span></code></pre>

<strong>Why Non-Contiguous?</strong>

python
<pre><code><span style="font-style: italic;"># Original memory layout (row-major)</span>
x = torch.tensor([[1, 2], [3, 4], [5, 6]])  <span style="font-style: italic;"># [3, 2]</span>
<span style="font-style: italic;"># Memory: [1, 2, 3, 4, 5, 6]</span>

<span style="font-style: italic;"># After transpose</span>
y = x.transpose(0, 1)  <span style="font-style: italic;"># [2, 3]</span>
<span style="font-style: italic;"># [[1, 3, 5],</span>
<span style="font-style: italic;">#  [2, 4, 6]]</span>
<span style="font-style: italic;"># Would need memory: [1, 3, 5, 2, 4, 6]</span>
<span style="font-style: italic;"># But still points to: [1, 2, 3, 4, 5, 6]</span>
<span style="font-style: italic;"># Non-contiguous! Uses strides to reinterpret.</span></code></pre>

<strong>Transpose vs Permute:</strong>

python
<pre><code>x = torch.randn(2, 3, 4, 5)

<span style="font-style: italic;"># .transpose() - swaps TWO dimensions only</span>
y = x.transpose(1, 2)  <span style="font-style: italic;"># [2, 4, 3, 5]</span>

<span style="font-style: italic;"># .permute() - arbitrary reordering of ALL dimensions</span>
y = x.permute(0, 2, 1, 3)  <span style="font-style: italic;"># [2, 4, 3, 5] - same result</span>
y = x.permute(3, 1, 0, 2)  <span style="font-style: italic;"># [5, 3, 2, 4] - more flexible!</span>

<span style="font-style: italic;"># For swapping 2 dims, transpose is simpler</span>
<span style="font-style: italic;"># For complex reordering, permute is clearer</span></code></pre>

<strong>Common Patterns:</strong>

python
<pre><code><span style="font-style: italic;"># 1. Transpose last two dimensions (very common!)</span>
x.transpose(-2, -1)  <span style="font-style: italic;"># or x.transpose(-1, -2) - same thing</span>

<span style="font-style: italic;"># 2. Batch-first to sequence-first (RNNs)</span>
x = torch.randn(32, 100, 512)  <span style="font-style: italic;"># [batch, seq, features]</span>
x = x.transpose(0, 1)          <span style="font-style: italic;"># [seq, batch, features]</span>

<span style="font-style: italic;"># 3. Channel repositioning</span>
x = torch.randn(32, 3, 224, 224)  <span style="font-style: italic;"># [B, C, H, W]</span>
x = x.transpose(1, 2)             <span style="font-style: italic;"># [B, H, C, W]</span></code></pre>

<strong>4D Example (Common in CNNs/Attention):</strong>

python
<pre><code><span style="font-style: italic;"># 4D tensor: [batch, heads, seq, head_dim]</span>
x = torch.randn(8, 12, 100, 64)  <span style="font-style: italic;"># [8, 12, 100, 64]</span>

<span style="font-style: italic;"># Transpose to move heads after seq</span>
y = x.transpose(1, 2)  <span style="font-style: italic;"># [8, 100, 12, 64]</span>
<span style="font-style: italic;"># Dimensions: 0→0, 1→2, 2→1, 3→3</span>

<span style="font-style: italic;"># Transpose last two (for K^T in attention)</span>
z = x.transpose(-2, -1)  <span style="font-style: italic;"># [8, 12, 64, 100]</span>
<span style="font-style: italic;"># Dimensions: 0→0, 1→1, 2→3, 3→2</span></code></pre>

<strong>Memory and Performance:</strong>

python
<pre><code><span style="font-style: italic;"># transpose() is cheap - just changes metadata (strides)</span>
x = torch.randn(1000, 1000, 1000)
y = x.transpose(0, 2)  <span style="font-style: italic;"># Instant! No data copying</span>

<span style="font-style: italic;"># But operations on non-contiguous tensors may be slower</span>
z = y + 1  <span style="font-style: italic;"># Might be slower than if y was contiguous</span>

<span style="font-style: italic;"># Make contiguous if doing many operations</span>
y = y.contiguous()  <span style="font-style: italic;"># Now subsequent ops faster</span></code></pre>

<strong>Key Takeaway:</strong> <code>.transpose(dim0, dim1)</code> swaps exactly two dimensions while keeping all others in place. It returns a view (fast, no copying) but creates a non-contiguous tensor. Essential for attention mechanisms (K^T), dimension reordering in transformers, and format conversions. Use negative indices for last dimensions: <code>.transpose(-2, -1)</code> is a very common pattern.
