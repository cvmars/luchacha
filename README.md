# 宝莲灯
宝莲灯对接说明
 在APP的WebView的header里添加参数bldSign,用来区分是从某个APP过来。

## 安卓对接流程

```
String BLD_APP_KEY= "4bc991f7-0600-4c0d-bfab-52c65a3f0433"' //管理员分配的AppKey
long curtTime = System.currentTimeMillis() ; //时间戳 单位毫秒
String strAppSecrt = curtTime + "&" + BLD_APP_KEY;                 
String result =curtTime +"&"+ MD5Utils.toMD5(strAppSecrt) ； //MD5加密
String encodeStr = "LCCS" + Base64Utils .encode(result .getBytes());  //Base64加密

Map<String, String> extraHeaders = new HashMap<String, String>();
extraHeaders .put("bldSign",encodeStr) 
webview.loadUrl("url",extraHeaders );   //webview的header里添加参数 bldSign

```
工具类 MD5Utils 

```
public class MD5Utils {
    public static String toMD5(String plainText) {
        byte[] secretBytes = null;
        try {
            secretBytes = MessageDigest.getInstance("md5").digest(
                    plainText.getBytes());
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("没有md5这个算法！");
        }
        String md5code = new BigInteger(1, secretBytes).toString(16);// 16进制数字
        // 如果生成数字未满32位，需要前面补0
        for (int i = 0; i < 32 - md5code.length(); i++) {
            md5code = "0" + md5code;
        }
        return md5code;
    }
}

```
工具类 Base64Utils 

```
public class Base64Utils {

    private static char[] base64EncodeChars = { 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '+', '/' };
    private static byte[] base64DecodeChars = { -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 62, -1, -1, -1, 63, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, -1, -1, -1, -1, -1, -1, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, -1, -1, -1, -1, -1, -1, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, -1, -1, -1, -1, -1 };

    public static String encode(byte[] paramArrayOfByte)
    {
        StringBuffer localStringBuffer = new StringBuffer();
        int i = paramArrayOfByte.length;
        int j = 0;
        while (j < i)
        {
            int k = paramArrayOfByte[(j++)] & 0xFF;
            if (j == i)
            {
                localStringBuffer.append(base64EncodeChars[(k >>> 2)]);
                localStringBuffer.append(base64EncodeChars[((k & 0x3) << 4)]);
                localStringBuffer.append("==");
                break;
            }
            int m = paramArrayOfByte[(j++)] & 0xFF;
            if (j == i)
            {
                localStringBuffer.append(base64EncodeChars[(k >>> 2)]);
                localStringBuffer.append(base64EncodeChars[((k & 0x3) << 4 | (m & 0xF0) >>> 4)]);
                localStringBuffer.append(base64EncodeChars[((m & 0xF) << 2)]);
                localStringBuffer.append("=");
                break;
            }
            int n = paramArrayOfByte[(j++)] & 0xFF;
            localStringBuffer.append(base64EncodeChars[(k >>> 2)]);
            localStringBuffer.append(base64EncodeChars[((k & 0x3) << 4 | (m & 0xF0) >>> 4)]);
            localStringBuffer.append(base64EncodeChars[((m & 0xF) << 2 | (n & 0xC0) >>> 6)]);
            localStringBuffer.append(base64EncodeChars[(n & 0x3F)]);
        }
        return localStringBuffer.toString();
    }

}
```




IOS对接流程






