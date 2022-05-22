# `*`转载文章示例

> - **原链接**: https://wiki.archlinux.org

## 第一章

- 第一章的内容

### 副标题

- 第一章的副标题

## 第二章

```python
import sys
import hashlib

filename = sys.argv[1]
with open(filename, 'rb') as file:
    data = file.read()
hash = hashlib.md5()
hash.update(data.encode(encoding='utf-8'))
encrypted_data = hash.hexdigest()
print(encrypted_data)
```
