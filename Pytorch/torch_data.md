# 数据

```python
import torch
#从numpy 创建
torch.from_numpy(ndarry)
torch.tensor(data, *dtype*=None, *device*=None, *requires_grad*=False, *pin_memory*=False) #与原ndarray共享内存，当修改其中一个的数据，另外一个也将会被改动

# 直接创建
torch.zeros(*size, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)
torch.ones(*size, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)
torch.full(size, fill_value, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)
torch.zeros_like(input, *dtype*=None, *layout*=None, *device*=None, *requires_grad*=False)
torch.ones_like(input, *dtype*=None, *layout*=None, *device*=None, *requires_grad*=False)
torch.full_like(input, fill_value, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False, *memory_format*=torch.preserve_format)         #_like都带 后边省略 memory_format=torch.preserve_format

# 根据数值
torch.arange(*start*=0, end, *step*=1, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False) #等差区间为[start, end]
torch.linspace(start, end, *steps*=100, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)#数值均分区间为[start, end]
torch.logspace(start, end, *steps*=100, *base*=10.0, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)  #对数均分
torch.eye(n, *m*=None, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)    #单位对角矩阵

# 根据概率
torch.normal(mean, std, *, *generator*=None, *out*=None)  #torch.normal(2, 3, size=(1, 4))    #生成正态分布（高斯分布）

# mean为标量，std为标量;mean为标量，std为张量;mean为张量，std为标量;mean为张量，std为张量
torch.randn(*size, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)  #标准正态分布
torch.randn_like(input, *dtype*=None, *layout*=None, *device*=None, *requires_grad*=False)
torch.rand(*size, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)    #[0, 1)
torch.rand_like(input, *dtype*=None, *layout*=None, *device*=None, *requires_grad*=False）
torch.randint(*low*=0, high, size, *, *generator*=None, *out*=None, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False) #[low, high)
torch.randint_like(input, *low*=0, high, *dtype*=None, *layout*=torch.strided, *device*=None, *requires_grad*=False)
torch.randperm(n, *out*=None, *dtype*=torch.int64, *layout*=torch.strided, *device*=None, *requires_grad*=False)#0到n-1的随机排列
torch.bernoulli(input, *, *generator*=None, *out*=None)   #以input为概率，生成伯努力分布

```

# 属性

```python
tensor.shape	 # 维数
tensor.dtype 	 # 数据类型
tensor.device	 # 存储设备
```

#  操作

```python
torch.cat(tensors, *dim*=0, *out*=None)   #按维度dim进行拼接 (3,3)+(3,3)->(6,3)or(3,6)    torch.cat([t, t], dim=0)
torch.stack(tensors, *dim*=0, *out*=None) #新创建的维度dim上进行拼接 (3,3)+(3,3)->(2,3,3)  dim=1 (3,2,3)
torch.chunk(input, chunks, *dim*=0) #按维度dim进行平均切分,torch.chunk(a, dim=1, chunks=3)
torch.split(tensor, split_size_or_sections, *dim*=0)#按维度dim进行切分
torch.index_select(input, dim, index, *out*=None)#按index索引数据
torch.masked_select(input, mask, *out*=None)    #mask中的True进行索引
torch.transpose(input, dim0, dim1)  #根据dim0和dim1对应的维度进行交换  # c*h*w  c*w*h
torch.squeeze(input, *dim*=None, *out*=None)      #压缩/移除长度为1的维度
torch.unsqueeze(input, dim)       #依据dim扩展维度

X.sum()			#对张量中的所有元素进行求和
x.reshape(input, shape)
x.numel()		#张量中元素的总数，即形状的所有元素乘积

```



# 计算

```python
torch.add(input, other, *out*=None)     #text{out} = text{input} + text{other}     x.add(x) ==== torch.add(x,x)
torch.addcdiv(input, tensor1, tensor2, *, *value*=1, *out*=None)  #out = input +value*(tensor1/tensor2)
torch.addcmul(input, tensor1, tensor2, *, *value*=1, *out*=None)  #out = input +value*tensor1*tensor2
sub(input, other, *, alpha=1, out=None)		#input -outher*alpha
torch.div(input, *out*=None)
torch.mul(input, *out*=None)
torch.log(input, *out*=None)
torch.log10(input, *out*=None)
torch.log2(input, *out*=None)
torch.exp(input, *out*=None)
torch.pow(input, *out*=None)
torch.abs(input, *out*=None)
torch.acos(input, *out*=None)
torch.cosh(input, *out*=None)
torch.cos(input, *out*=None)
torch.asin(input, *out*=None)
torch.atan(input, *out*=None)
torch.atan2(input, *out*=None)
```

