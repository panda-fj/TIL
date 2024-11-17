# apparent_encoding

レスポンスデータから、適切な文字エンコーディングを自動的に推測する。

```py
import requests

url = "https://example.com"
response = requests.get(url)

# apparent_encodingを使ってデコード
print(response.apparent_encoding)  # 推測されたエンコーディングを表示
print(response.content.decode(response.apparent_encoding))  # 推測したエンコーディングでデコード
```

## エンコーディングの推測

`chardet.detect` または `charset_normalizer.detect` を用いて行われる。

```py
# Lib\site-packages\requests\models.py
@property
    def apparent_encoding(self):
        """The apparent encoding, provided by the charset_normalizer or chardet libraries."""
        if chardet is not None:
            return chardet.detect(self.content)["encoding"]
        else:
            # If no character detection library is available, we'll fall back
            # to a standard Python utf-8 str.
            return "utf-8"
```

```py
# Lib\site-packages\requests\compat.py
def _resolve_char_detection():
    """Find supported character detection libraries."""
    chardet = None
    for lib in ("chardet", "charset_normalizer"): # "chardet" か "charset_normalizer" 
        if chardet is None:
            try:
                chardet = importlib.import_module(lib)
            except ImportError as e:
                pass
    return chardet


chardet = _resolve_char_detection()
```

## charset_normalize.detect

```py
# Lib\site-packages\charset_normalizer\legacy.py
def detect(
    byte_str: bytes, should_rename_legacy: bool = False, **kwargs: Any
) -> Dict[str, Optional[Union[str, float]]]:
    """
    chardet legacy method
    Detect the encoding of the given byte string. It should be mostly backward-compatible.
    Encoding name will match Chardet own writing whenever possible. (Not on encoding name unsupported by it)
    This function is deprecated and should be used to migrate your project easily, consult the documentation for
    further information. Not planned for removal.

    :param byte_str:     The byte sequence to examine.
    :param should_rename_legacy:  Should we rename legacy encodings
                                  to their more modern equivalents?
    """
    # kwargs に追加引数が渡された場合、無視する旨を警告する
    if len(kwargs):
        warn(
            f"charset-normalizer disregard arguments '{','.join(list(kwargs.keys()))}' in legacy function detect()"
        )

    # byte_str が bytes または bytearray 型でない場合、例外をスローし、型の安全性を確保
    if not isinstance(byte_str, (bytearray, bytes)):
        raise TypeError(  # pragma: nocover
            "Expected object of type bytes or bytearray, got: "
            "{0}".format(type(byte_str))
        )

    # 内部処理で統一的に扱うため、 bytearray が渡された場合は bytes に変換。
    if isinstance(byte_str, bytearray):
        byte_str = bytes(byte_str)

    # from_bytes は CharsetNormalizer の関数で、バイト列を解析する。
    # best() は最適な候補（最も信頼度の高い結果）を返す。
    r = from_bytes(byte_str).best()

    # 結果オブジェクト r の値を取り出し、必要に応じて値を設定。
    encoding = r.encoding if r is not None else None
    language = r.language if r is not None and r.language != "Unknown" else ""
    confidence = 1.0 - r.chaos if r is not None else None

    # utf_8 に BOM（Byte Order Mark）が含まれる場合、"utf_8_sig" として処理。
    if r is not None and encoding == "utf_8" and r.bom:
        encoding += "_sig"

    # should_rename_legacy が False かつ CHARDET_CORRESPONDENCE にマッチする場合、エンコーディング名を変換。
    if should_rename_legacy is False and encoding in CHARDET_CORRESPONDENCE:
        encoding = CHARDET_CORRESPONDENCE[encoding]

    return {
        "encoding": encoding,
        "language": language,
        "confidence": confidence,
    }
```