---
title: "Nvs"
date: 2023-05-22T11:13:38+04:00
draft: true
tags:
  - "esp"
  - "idf"
---
# **NVS** - Non-volatile Storage

## Keys and values

NVS contain key-value pairs.
Max key length is _15_ char characters.

Values can have one of below types:

1. Integer `int8_t, uint16_t, int32_t, etc.`
2. Zero-terminated strings(max length is 4000 chars)
3. Blobs

But storing blobs and big string in NVS is a bad idea because for this purpose better use FAT fs.
NVS is more suitable for storing many small values.

## Namespaces

NVS also can contain namespaces(max name length also 15 chars) to mitigate conflicts between keys, because they are unique.
Namespaces with the same name on different partitions are considered as separate namespaces.
One NVS partition can has no more than 254 namespaces.

## Type safety

If you write a value to key that isn't the same type as the old value you will get error returned.
Also you will get error returned if you the data type of read operation does not match the type of the value.
