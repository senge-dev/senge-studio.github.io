## 获取国内疫情
```python
from pycovid.covid import PyCovid
covid = PyCovid()
data = covid.cn_covid()
```
 - 返回值(数据为虚构数据，具体数据以官方为准)
```json
[
  {
    "provinceName": "A",
    "currentConfirmed": 0,
    "confirmed": 3,
    "dead": 0,
    "cured": 3
  },
  {
    "provinceName": "B",
    "currentConfirmed": 21,
    "confirmed": 1542,
    "dead": 1,
    "cured": 1520
  }
]
```
> 不同字段的含义：
|字段            |含义      |
| -------------- | ------- |
|provinceName    |省份      |
|currentConfirmed|现存确诊  |
|confirmed       |累计确诊  |
|cured           |累计治愈  |
|dead            |累计死亡  |

>使用参数忽略某个返回值
>- current=False(忽略现存确诊人数)
>- confirmed=False(忽略累计确诊人数)
>- cured=False(忽略累计治愈人数)
>- dead=False(忽略累计死亡人数)
