###1. str与bytes之间的转换
- str -> bytes: bytes(str, encoding = "utf8")，str.encode("utf8")
	
		s = "example"
		sb = bytes(s, encoding="utf8")

- bytes -> str: str(b, encoding = "utf8"), b.decode("utf8")

		b = b"example"
		s = str(b, encoding = "utf8")

### 2. unicode
转换编码只在2进制文件解码编码中必须指定，如果文本文件指定encoding则会报错