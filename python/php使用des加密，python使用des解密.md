# php使用des加密，python使用des解密

### php加密代码

```
function encryptECB($data)
{
   $key = 'wmWnYL4pJj3oxTapwKTdkVNb1j8dCivD';
   $method = 'AES-256-ECB';
   if (in_array($method, openssl_get_cipher_methods())) {
     return openssl_encrypt($data, $method, $key, $options = 0);
   }
}

使用：
   $data = '123';
   encryptECB($data);   //得到结果：JjE+FSUE+D6a5v92hoK4Hw==
```

#### python解密代码

```
def aes_decrypt(encryStr):

  """

    AES 解密

    :param encryStr: 加密后的字符串

    :return: 解密后的字符串

  """

  key = b'wmWnYL4pJj3oxTapwKTdkVNb1j8dCivD'

  mode = AES.MODE_ECB

  cryptos = AES.new(key, mode)

  plain_text = cryptos.decrypt(b64decode(encryStr))

  res = unpad(plain_text, AES.block_size)

  return res.decode('utf8')

if __name__ == '__main__':

  encryptStr = b'JjE+FSUE+D6a5v92hoK4Hw=='

  aes_decrypt(encryptStr)
```

