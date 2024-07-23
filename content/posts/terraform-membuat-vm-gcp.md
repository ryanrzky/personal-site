---
title: "Terraform: Membuat VM di GCP"
date: 2023-04-24
author: "Ryan Rizky Diantoro"
description: "Cara membuat VM di Google Cloud Provider menggunakan Terraform"
draft: false
categories: [DevOps, IaC]
tags: [Terraform]
---

Membuat VM sederhana di GCP.

## Persiapan
Sebelum membuat sebuah VM/Compute di GCP, kita perlu mempersiapkan beberapa resource terlebih dahulu.
1. VPC
2. Subnet
3. Firewall

## Membuat VPC, Subnet dan Firewall
Buat file main.tf , kemudian isi file main.tf seperti dibawah. Sesuaikan value di block `provider`
```hcl
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.61.0"
    }
  }
}

provider "google" {
  # Configuration options
  project = "project_id_gcp" # project id GCP
  region = "us-central1" # region
  zone = "us-central1-a" # zone
  credentials = "credentials.json" # file credential GCP
}

# VPC
resource "google_compute_network" "vpc_network" {
  name                    = "vpc-test"
  auto_create_subnetworks = false
}

# Subnet
resource "google_compute_subnetwork" "subnet" {
  name = "subnet-1"
  ip_cidr_range = "172.10.0.0/24"
  network = google_compute_network.vpc_network.id
  region = "us-central1"
}

# Firewall
resource "google_compute_firewall" "firewall" {
  name    = "firewall"
  network = google_compute_network.vpc_network.id

  allow {
    protocol = "icmp"
  }

  allow {
    protocol = "tcp"
    ports    = ["80", "22"]
  }

  source_tags = ["ssh-http"]
  source_ranges = ["0.0.0.0/0"]
}
```

## Membuat VM
Tambahkan code dibawah ini di file main.tf yang sebelumnya kita buat.
```hcl
# VM
resource "google_compute_instance" "default" {
  name         = "vm-test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["ssh-http"]

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
      }
    }

  network_interface {
    network = "vpc-test"
    subnetwork = "subnet-1"

    access_config {
      // Ephemeral public IP
    }
  }
}
```

Kemudian hasil akhir akan seperti ini
```hcl
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.61.0"
    }
  }
}

provider "google" {
  # Configuration options
  project = "project_id_gcp" # project id GCP
  region = "us-central1" # region
  zone = "us-central1-a" # zone
  credentials = "credentials.json" # file credential GCP
}

# VPC
resource "google_compute_network" "vpc_network" {
  name                    = "vpc-test"
  auto_create_subnetworks = false
}

# Subnet
resource "google_compute_subnetwork" "subnet" {
  name = "subnet-1"
  ip_cidr_range = "172.10.0.0/24"
  network = google_compute_network.vpc_network.id
  region = "us-central1"
}

# Firewall
resource "google_compute_firewall" "firewall" {
  name    = "firewall"
  network = google_compute_network.vpc_network.id

  allow {
    protocol = "icmp"
  }

  allow {
    protocol = "tcp"
    ports    = ["80", "22"]
  }

  source_tags = ["ssh-http"]
  source_ranges = ["0.0.0.0/0"]
}

# VM
resource "google_compute_instance" "default" {
  name         = "vm-test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["ssh-http"]

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
      }
    }

  network_interface {
    network = "vpc-test"
    subnetwork = "subnet-1"

    access_config {
      // Ephemeral public IP
    }
  }
}
```

Kemudian jalankan perintah `terraform init` untuk inisialisasi provider yang diperlukan. Jika sudah kemudian cek script diatas menggunakan perintah `terraform plan`. Jika dirasa sudah sesuai kemudian apply menggunakan perintah `terrafrom apply`

Terimakasih.