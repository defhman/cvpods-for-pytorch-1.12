# Fix Bugs.


## 1.Changes


### 1.1.Seacrh

```
#include <THC/THC.h>
```

### 1.2.Fix THCudaCheck

- Replace

```
THCudaCheck(cudaGetLastError());
```

replaced by

```
AT_CUDA_CHECK(cudaGetLastError());
```

### 1.3.Fix THCCeilDiv

- Replace

```
THCCeilDiv(x,y)
```

replaced by

```
(x + y - 1 ) / y
```

### 1.4.Fix THCudaMalloc/THCudaFree/THCState

- Add include

```
#include <ATen/cuda/ThrustAllocator.h>
```

- Uncomment line

```
// THCState *state = at::globalContext().lazyInitCUDA(); // TODO replace with getTHCState
```

- Replace

```
mask_dev = (unsigned long long*) THCudaMalloc(state, boxes_num * col_blocks * sizeof(unsigned long long));
```

replaced by

```
mask_dev = (unsigned long long*) c10::cuda::CUDACachingAllocator::raw_alloc(boxes_num * col_blocks * sizeof(unsigned long long));
```

- Replace

```
THCudaFree(state, mask_dev);
```

replaced by

```
c10::cuda::CUDACachingAllocator::raw_delete(mask_dev);
```