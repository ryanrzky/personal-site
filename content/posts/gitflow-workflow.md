---
title: "Gitflow Workflow"
date: 2024-05-01
author: "Ryan Rizky Diantoro"
description: "Apa itu Gitflow Workflow"
draft: false
categories: [DevOps]
---

## Apa itu Gitflow

Gitflow adalah alternatif Git branching yang melibatkan penggunaan feature branch dan beberapa branch utama. Jika dibandingkan dengan trunk-based development, Gitflow memiliki banyak branch dan banyak commit.
Dalam Gitflow, developer bisa membuat feature branch dan bisa menunda merging ke main branch sampai feature tersebut selesai.

Gitflow juga dapat digunakan untuk project yang mempuanyai jadwal perilisan.

## Cara Kerja

### Develop dan Main Branch
![gitflow-1](/images/gitflow-1.svg)
Gitflow menggunakan 2 branch untuk merekam history dari sebuah project. Main branch menyimpan official release history dan Develop berfungis sebagai integrasi untuk feature branch. Dengan Gitflow, kita juga bisa membuat tag untuk setiap commit yang ada di main branch.

### Feature Branch
![gitflow-2](/images/gitflow-2.svg)
Setiap Feature harus berada di branchnya sendiri dan menggunakan develop branch sebagai parent nya. Setelah feature sudah selesai akan di merge kembali ke develop branch. Yang perlu di ingat, feature branch tidak boleh berinteraksi langsung dengan main branch.

### Release Branch
![gitflow-3](/images/gitflow-3.svg)
ketika develop branch sudah siap untuk di release atau tanggal release sudah di tentukan, buat release branch dari develop branch. Pembuatan release branch dilakukan di next release cycle, jadi tidak akan ada feature baru yg di tambahkan dan hanya bug fix yang bisa ditambahkan di release branch.

Menggunakan dedicated release branch memungkinkan untuk menyempurnakan release saat ini dan sementara tim lain bisa mengerjakan feature untuk release selanjutnya.

Ketika release siap untuk dibawa ke production, release branch akan di merge ke main dan develop branch, dan release branch akan dihapus.

### Hotfix Branch
![gitflow-4](/images/gitflow-4.svg)
Tidak selamanya code yang sudah dibawa ke production sempurna, terkadang masih ada bug yang perlu di fix secepat mungkin.
Hotfix branch digunakan untuk fix dengan cepat di production. Hotfix branch hampir mirip dengan feature branch, tapi hotfix branch menggunakan main sebagai parentnya.

Setelah hotfix selesai, hotfix branch harus di merge kemabali ke main, develop dan ke release branch saat ini, dan main branch harus di tag dengan versi yang terbaru.

## Cara Memulai

Git-flow merupakan wrapper dari Git, dan instalasinya cukup mudah.

```bash
# MAC
brew install git-flow

# Ubuntu
sudo apt -y install git-flow
```

Inisialisasi Project
```bash
git flow init
```

Feature Branch
```bash
# Membuat feature branch
git flow feature start feature_branch

# Menyelesaikan feature branch
git flow feature finish feature_branch
```

Release Branch
```bash
# Membuat release branch
git flow release start 0.1.0

# Menyelesaikan release branch
git flow release finish '0.1.0'
```

Hotfix Branch
```bash
# Membuat hotfix branch
git flow hotfix start hotfix_branch

# Menyelesaikan hotfix branch
git flow hotfix finish hotfix_branch
```

## Kesimpulan
Beberapa hal yang perlu diketahui tentang Gitflow:

1. Gitflow bagus untuk release-based workflow.
2. Gitflow menawarkan alur khusus untuk hotfix ke production.
3. Tidak harus memakai Gitflow, jika ingin cepat dalam proses release bisa menggunakan trunk-based development.

referensi: https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow