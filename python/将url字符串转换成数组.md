# 将url字符串转换成数组

使用 parse_qs() 来将查询参数解析成 dict

```

import urllib.parse

def uri_parse

  uri = b'id=123&name=jhon&sex=boy&age=10'

  dist = urllib.parse.parse_qs(uri)

  print(dist)
```

