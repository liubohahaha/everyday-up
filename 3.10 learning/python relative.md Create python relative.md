### python 字符串  
  [头下标:尾下标] 获取的子字符串包含头下标的字符，但不包含尾下标的字符。
```
  >>>s = 'abcdef'
  >>>s[1:5]
  'bcde'
```
### python 字典  
字典用"{ }"标识。字典由索引(key)和它对应的值value组成，左边为键，右边为值
```
dict = {}
dict['one'] = "This is one"
dict[2] = "This is two"
tinydict = {'name': 'runoob','code':6734, 'dept': 'sales'}
print dict['one']          # 输出键为'one' 的值
print dict[2]              # 输出键为 2 的值
print tinydict             # 输出完整的字典
print tinydict.keys()      # 输出所有键
print tinydict.values()    # 输出所有值

输出
This is one
This is two
{'dept': 'sales', 'code': 6734, 'name': 'runoob'}
['dept', 'code', 'name']
['sales', 6734, 'runoob']
```  
