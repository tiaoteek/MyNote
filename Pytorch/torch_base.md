# 使用gpu

```python
# 判断当前环境GPU是否可用, 然后将tensor导入GPU内运行
if torch.cuda.is_available():
  tensor = tensor.to('cuda')
```