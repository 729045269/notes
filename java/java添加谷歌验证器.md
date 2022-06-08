#### java使用谷歌验证器二次验证

##### 导入依赖包

```java
<dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>javase</artifactId>
    <version>3.1.0</version>
</dependency>
```

##### 创建类 GoogleAuthenticator.java

```
package com.ruoyi.common.google;


import jodd.util.Base32;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;

public class GoogleAuthenticator {
    private static int windowSize = 1; // default 3 - max 17 (from google docs)最多可偏移的时间(允许有几秒的误差)

    public static void setWindowSize(int s) {
        if (s >= 1 && s <= 17)
            windowSize = s;
    }

    /**
     * 生成谷歌密钥
     *
     * @return
     */
    public static String getRandomSecretKey() {
        SecureRandom random = new SecureRandom();
        byte[] bytes = new byte[20];
        random.nextBytes(bytes);
        String secretKey = Base32.encode(bytes);
        return secretKey.toUpperCase();
    }

    /**
     * 校验谷歌验证码
     *
     * @param secret
     * @param code
     * @return
     */
    public static boolean checkCode(String secret, long code) {
        long timeMsec = System.currentTimeMillis();
        byte[] decodedKey = Base32.decode(secret);
        long t = (timeMsec / 1000L) / 30L;
        System.out.println(secret);
        for (int i = -windowSize; i <= windowSize; ++i) {
            long hash;
            try {
                hash = verifyCode(decodedKey, t + i);
            } catch (Exception e) {
                throw new RuntimeException(e.getMessage());
            }
            if (hash == code) {
                return true;
            }
        }
        return false;
    }

    private static int verifyCode(byte[] key, long t) throws NoSuchAlgorithmException, InvalidKeyException {
        byte[] data = new byte[8];
        long value = t;
        for (int i = 8; i-- > 0; value >>>= 8) {
            data[i] = (byte) value;
        }
        SecretKeySpec signKey = new SecretKeySpec(key, "HmacSHA1");
        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(signKey);
        byte[] hash = mac.doFinal(data);
        int offset = hash[20 - 1] & 0xF;
        // We're using a long because Java hasn't got unsigned int.
        long truncatedHash = 0;
        for (int i = 0; i < 4; ++i) {
            truncatedHash <<= 8;
            truncatedHash |= (hash[offset + i] & 0xFF);
        }
        truncatedHash &= 0x7FFFFFFF;
        truncatedHash %= 1000000;
        return (int) truncatedHash;
    }
}

```



##### 使用方法

```
 /**
 * 生成谷歌验证码密钥
 * @return
 */
 public String generateGoogleSecret() {
     String secret = GoogleAuthenticator.getRandomSecretKey();
	 return secret;
 }
 /**
 * 校验谷歌验证码
 *
 * @param secret
 * @param code
 * @return
 */
 public Boolean checkGoogleCode(String secret, long code) {
 	return GoogleAuthenticator.checkCode(secret, code);
 }

```

