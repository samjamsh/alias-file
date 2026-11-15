# `.alias` File Format Specification

## 1. Overview

The `.alias` file format is a lightweight binary container specification designed to store direct reference paths to local file targets. The format consists of a fixed-size header section followed by a variable length payload section containing the hexadecimal-encoded file path

---

## 2. Binary Layout Specification

All `.alias` files strictly adhere to a 5-byte fixed header followed immediately by the file path payload

```text
 0x00       0x01       0x03       0x05                                0x05 + N
+----------+----------+----------+------------------------------------+
|   Type   | Version  |   Size   |            Hex Payload             |
| (1 Byte) | (2 Bytes)| (2 Bytes)|             (N Bytes)              |
+----------+----------+----------+------------------------------------+
```

---

## 3. Header Field Definitions

| Offset | Field Name | Data Type | Endianness | Value Range / Definition |
| :--- | :--- | :--- | :--- | :--- |
| `0x00` | **Type** | `uint8` | **Big-Endian** | Format identifier magic byte. Fixed constant: `0x01` |
| `0x01 - 0x02` | **Version** | `uint16` | **Big-Endian** | File format version number. Format v1.0 standard: `0x0001` |
| `0x03 - 0x04` | **Size** | `uint16` | **Big-Endian** | Exact byte/character length ($N$) of the hexadecimal payload section |
| `0x05 - End` | **Hex Payload** | `ASCII String` | N/A | Hexadecimal-encoded ASCII representation of the target file path |

---

## 4. Format Rules & Data Standards

 **Strict Big-Endian Encoding:** All multi-byte and single-byte fields within the header structure (`Type`, `Version`, and `Size`) are explicitly stored using Big-Endian byte ordering
 **Hexadecimal String Payload:** The target file path is stored exclusively as an ASCII string composed of hexadecimal representations of the underlying path characters
 **Payload Boundaries:** The `Size` field defines the exact number of bytes $N$ that follow the 5-byte header. The maximum allowable payload size for version 1.0 is $65,535$ bytes (`0xFFFF`)
 **Encoding Integrity:** The payload must represent valid hex pairs corresponding to an absolute target path string
