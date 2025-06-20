# tools

# Chinese Stroke Count (繁體中文字筆劃數查詢)

此專案提供 Java 程式碼，用於根據 Big5 編碼來查詢**繁體中文字**的筆劃數。若非 Big5 編碼範圍內的繁體字，將回傳 -1。

## 特色

- 使用 Big5 編碼轉換字元為 16 進位字串。
- 根據筆劃對應表，快速查出字的筆劃數。
- 支援所有常見繁體中文字符。

## 使用說明

### 方法：`GetTraditionStrokeCount(String txt)`

```java
/**
 * 取得繁體字筆畫，非繁體字回傳 -1
 *
 * @param txt 欲查詢的繁體中文字元（僅取第一個字元）
 * @return 筆劃數（1~29），非繁體字回傳 -1
 */
public static int GetTraditionStrokeCount(String txt) throws UnsupportedEncodingException {
    byte[] bytes = txt.getBytes("BIG5");
    String hex = byte2hex(bytes);
    for (int i = 0; i < TraditionStrokesData.length; ++i) {
        for (int j = 0; j < TraditionStrokesData[i].length; j += 2) {
            if (hex.compareTo(TraditionStrokesData[i][j]) >= 0 && hex.compareTo(TraditionStrokesData[i][j + 1]) <= 0)
                return i + 1;
        }
    }
    return -1;
}
```
### 輔助方法：byte2hex
```java
/**
 * 將 byte 陣列轉為大寫 16 進位字串
 */
public static String byte2hex(byte[] b) {
    String hs = "";
    for (int n = 0; n < b.length; n++) {
        String stmp = Integer.toHexString(b[n] & 0XFF);
        hs += (stmp.length() == 1 ? "0" : "") + stmp;
    }
    return hs.toUpperCase();
}
```

### 筆劃資料表 TraditionStrokesData 此資料表為一個二維字串陣列，用來對照 Big5 編碼區段與筆劃數對應關係：
```java
private static String[][] TraditionStrokesData = {
    new String[] { "A440", "A441" }, // 1
    new String[] { "A442", "A453", "C940", "C944" }, // 2
    new String[] { "A454", "A47E", "C945", "C94C" }, // 3
    new String[] { "A4A1", "A4FD", "C94D", "C95C" }, // 4
    new String[] { "A4FE", "A5DF", "C95D", "C9AA" }, // 5
    new String[] { "A5E0", "A6E9", "C9AB", "C959" }, // 6
    new String[] { "A6EA", "A8C2", "CA5A", "CBB0" }, // 7
    new String[] { "A8C3", "AB44", "CBB1", "CDDC" }, // 8
    new String[] { "AB45", "ADBB", "CDDD", "D0C7", "F9DA", "F9DA" }, // 9
    new String[] { "ADBC", "B0AD", "D0C8", "D44A" }, // 10
    new String[] { "B0AE", "B3C2", "D44B", "D850" }, // 11
    new String[] { "B3C3", "B6C3", "D851", "DCB0", "F9DB", "F9DB" }, // 12
    new String[] { "B6C4", "B9AB", "DCB1", "E0EF", "F9D6", "F9D8" }, // 13
    new String[] { "B9AC", "BBF4", "E0F0", "E4E5" }, // 14
    new String[] { "BBF5", "BEA6", "E4E6", "E8F3", "F9DC", "F9DC" }, // 15
    new String[] { "BEA7", "C074", "E8F4", "ECB8", "F9D9", "F9D9" }, // 16
    new String[] { "C075", "C24E", "ECB9", "EFB6" }, // 17
    new String[] { "C24F", "C35E", "EFB7", "F1EA" }, // 18
    new String[] { "C35F", "C454", "F1EB", "F3FC" }, // 19
    new String[] { "C455", "C4D6", "F3FD", "F5BF" }, // 20
    new String[] { "C3D7", "C56A", "F5C0", "F6D5" }, // 21
    new String[] { "C56B", "C5C7", "F6D6", "F7CF" }, // 22
    new String[] { "C5C8", "C5F0", "F6D6", "F7CF" }, // 23
    new String[] { "C5F1", "C654", "F8A5", "F8ED" }, // 24
    new String[] { "C655", "C664", "F8E9", "F96A" }, // 25
    new String[] { "C665", "C66B", "F96B", "F9A1" }, // 26
    new String[] { "C66C", "C675", "F9A2", "F9B9" }, // 27
    new String[] { "C676", "C67A", "F9BA", "F9C5" }, // 28
    new String[] { "C67B", "C67E", "F9C6", "F9DC" }, // 29
};
```

### 範例/用法
```java
System.out.println(GetTraditionStrokeCount("一")); // 輸出：1
System.out.println(GetTraditionStrokeCount("繁")); // 輸出：18
System.out.println(GetTraditionStrokeCount("A")); // 輸出：-1（非繁體中文字）
```


### 特殊情況：注音符號的筆畫用以下陣列進行例外處理。
### 例如：ㄅ = 1, ㄆ = 2, ㄓ = 3
```java
private static String[] Phonetic_stroke = { "ㄅㄋㄑㄟㄣㄥㄧ˙ˊˇˋ", "ㄆㄇㄈㄉㄌㄍㄎㄏㄐㄒㄗㄘㄙㄛㄜㄡㄢㄦㄨㄩ", "ㄊㄔㄕㄚㄝㄞㄠㄤ", "ㄓㄖ" };
```
