+++
title = "Hands on: File System Forensics"
date = 2023-05-26
[taxonomies]
tags = ["forensics"]
+++

# Hands on: File System Forensics

## Contents

- [Scenario](#scenario)
- [persiapan environment](#persiapan-environment)
  - [barang bukti](#barang-bukti)
- [Evidence Acquisition](#evidence-acquisition)
- [File System Analysis](#file-system-analysis)
- [File Carving](#file-carving)
- [Metadata](#metadata)

## Scenario

Seorang kriminal siber membeli data user tokopedia dari seorang hacker di sebuah forum.
Kriminal tersebut menyimpan data tersebtu di dalam sebuah flashdisk.
Setelah melakukan kejahatan, kriminal tersebut menghapsu data yang telah dibelinya untuk menghapus jejak digital.

## Persiapan Environment

### Barang Bukti

- Dibuat sebuah flashdisk virtual berukuran 1GB

```bash
qemu-img create -f raw fat32.img 1G
mkfs.fat -F 32 fat32.img
```

- Mount flashdisk virtual tersebut
- Copy data user tokopedia yang telah didapatkan dari hacker
- Unzip file tersebut
- Hapus file zip serta csv isi dari zip tersebut

## Evidence Acquisition

Cek lokasi flashdisk virtual tersebut

```bash
lsblk
```

![lsblk](/img/lsblk1.webP)

Evidence acquisition dapat dilakukan dengan menggunakan tool dd atau dcfldd

```bash
sudo dcfldd if=/dev/vdb of=usb_1gb.dd hash=md5
```

Cek image hash dan tulis ke dalam file

```bash
md5sum usb_1gb.dd > usb_1gb.dd.md5
```

![dcfldd](/img/dcfldd.webP)

## File System Analysis

Display file system information

```bash
file -s usb_1gb.dd
```

```bash
img_stat usb_1gb.dd
```

mmls untuk melihat offset

```bash
mmmls usb_1gb.dd
```

Melihat informasi file system

```bash
sudo fsstat -o <offset> usb_1gb.dd
```

List file

```bash
sudo fls -o <offset> usb_1gb.dd
```

## File Carving

```bash
sudo tsk_recover -i raw -e usb_1gb.dd -o output_dir
```

![recovered](/img/recovered.webP)
![isicsv](/img/isicsv.webP)

## Metadata

![metadata1](/img/metadata1.webP)
![metadata2](/img/metadata2.webP)
![exif](/img/exif.webP)
![metadata3](/img/metadata3.webP)
![exif2](/img/exif2.webP)
![metadata4](/img/metadata4.webP)
![exif3](/img/exif3.webP)
