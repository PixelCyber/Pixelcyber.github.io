---
layout: raw
---

# Anubis URL Scheme

### 1. Basic format (x-callback-url)

*Fields in query string are not encoded with `Encode URL Compoent` for readable purpose, as below.*

```js
anubis://x-callback-url/$module
?input=$input
&gui=1
&x-source=sourceApp
&x-cancel=xx://other.app.url
&x-success=xx://other.app.url/path?input=$output
&x-error=xx://other.app.url/path?errorCode=code&errorMessage=message
```

**`$module`**: feature path, e.g. `tools/regex`

**`$input`**: the income data for Anubis to process.
```json
{"text": "some text", "pbFileName": "xx.txt", "pbName": ""}
```
- `text`: text/string passed through URL. The following two fields should be ignored, if `text` is not empty.
- `pbFileName`: file name for the file passed with Pasteboard.
- `pbName`: name of the Pasteboard. Empty name with `""` is for the system general Pasteboard. Passed a `pbName` with `""` When you passed a string with Pasteboard, like `{"pbName": ""}`.

**`$output`**: the result processed by Anubis, will be appended as `input=$output` to query string of `x-success`. (`$output` has same structure as `$input`).

**`gui`**: optional, 1 for GUI mode, 0 for process in background, then callback with `x-success` or `x-error`, default: 0.


### 2. Utilities

#### tool list
```js
anubis://x-callback-url/tools/?input=$input
```

#### RegEx Validator
```js
anubis://x-callback-url/tools/regex?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "reg": "^abc", "case": true, "global": false, "replace": "00"}
```

**$input**: has extra fields than basic format as below:
- `reg`: regular expression
- `replace`: replacement
- `case`: optional, true is case insensitive，default: false
- `global`: optional, global search, default: true


#### SSL Certificate Inspector
```js
anubis://x-callback-url/tools/ssl?input=$input
```

#### Date Formmatter
```js
anubis://x-callback-url/tools/date?input=$input
```

#### UUID Generator
```js
anubis://x-callback-url/tools/uuid
```

#### Base64
```js
anubis://x-callback-url/tools/base64?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false}
```
- `decode`: optional, true for decode, false for encode, default: false
  

#### URL Encode
```js
anubis://x-callback-url/tools/urlencode?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false, "component": false, "multiLine": false}
```
- `decode`: optional, true for decode, false for encode, default: false
- `component`: optional, encode only, true for `Encode URL Compoent`, default: false
- `multiLine`: optional, encode/decode line by line, default: false


#### String Encoding: e.g. gbk -> utf-8
```js
anubis://x-callback-url/tools/stringEncode?input=$input
```

#### Native to ASCII
```js
anubis://x-callback-url/tools/native2ascii?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false, "ignoreASC": false}
```
- `decode`: optional, true for decode, false for encode, default: false
- `ignoreASC`: optional, encode only, true: ignore ascii characters, default: false


#### Data to Hex
```js
anubis://x-callback-url/tools/date2hex?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false}
```
- `decode`: optional, true for decode, false for encode, default: false
  

#### String Escaping
```js
anubis://x-callback-url/tools/stringEscape?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false, "regex": false}
```
- `decode`: optional, true for decode, false for encode, default: false
- `regex`: optional, true for regular expression，false for string, default: false
  

#### HTML Escaping
```js
anubis://x-callback-url/tools/htmlEscape?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false, "ascii": false}
```
- `decode`: optional, true for decode, false for encode, default: false
- `ascii`: optional, encode only，true: ascii characters will be encoded, default: false
  

#### IDN (Punnycode)
```js
anubis://x-callback-url/tools/punnycode?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false}
```
- `decode`: optional, true for decode, false for encode, default: false
  

#### Quoted-printable
```js
anubis://x-callback-url/tools/quotedPrintable?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "decode": false}
```
- `decode`: optional, true for decode, false for encode, default: false
  

#### MD5, SHA, CRC
```js
anubis://x-callback-url/tools/hash?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "alg": "MD5", "multiLine": true, "upper": true}
```
- `alg`: `MD5, MD4, MD3, SHA1, SHA224, SHA256, SHA384, SHA512, CRC32, CRC32B, Adler32, NSString.hash`
- `upper`: optional, true for output as upper，false for output as lower, default: false
- `multiLine`: optional, hash line by line, default: false
  

#### Hmac
```js
anubis://x-callback-url/tools/hashSalt?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "alg": "HmacMD5", "salt": "xxx", "multiLine": true, "upper": true}
```
- `alg`: `HmacMD5, HmacSHA1, HmacSHA224, HmacSHA256, HmacSHA384, HmacSHA512`
- `salt`: optional, the input key of Hmac
- `upper`: optional, true for output as upper，false for output as lower, default: false
- `multiLine`: optional, hash line by line, default: false


#### Basic Cryptology
```js
anubis://x-callback-url/tools/crypto?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "alg": "AES128", "key": "xxx", "ivHex": "", "mode": "CBC", "decrypt": false}
```
- `alg`: `AES128, AES192, AES256, DES, 3DES, RC4, RC2, Blowfish, Cast-128`
- `key`: key text
- `keyHex`: hex string for key
- `ivHex`: hex string for iv
- `mode`: `CBC, ECB, CTR, CFB, CFB8, NOFB`. (`ECB` has no `ivHex`)
- `decrypt`: optional, true for decrypt，false for encrypt, default: false

| Algorithm / length (byte) | key | iv | block |
|:-------|:----|:----|:----|
| AES128 | 16 | 16 | 16 |
| AES192 | 24 | 16 | 16 |
| AES256 | 32 | 16 | 16 |
| DES | 8 | 8 | 8 |
| 3DES | 24 | 8 | 8 |
| RC4 | 1~512 | - | - |
| RC2 | 1~128 | 8 | 8 |
| Blowfish | 8~56 | 8 | 8 |
| Cast-128 | 5~16 | 8 | 8 |


#### Crypto.js
```js
anubis://x-callback-url/tools/crypto.js?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "alg": "AES128", "pwd": "xxx", "decrypt": false}
```
- `alg`: `AES256, DES, 3DES, RC4`
- `pwd`: password
- `decrypt`: optional, true for decrypt，false for encrypt, default: false


#### gzip, br, deflate
```js
anubis://x-callback-url/tools/streamCompress?input={"text": "some text", "pbFileName": "xx.txt", "pbName": "", "alg": "gzip", "extract": false}
```
- `alg`: `gzip, br, deflate`
- `extract`:  optional, true for extract, false for compress, default: false


#### HTTP chunked extract
```js
anubis://x-callback-url/tools/chunked?input=$input
```


#### JSMin
```js
anubis://x-callback-url/tools/jsmin?input=$input
```