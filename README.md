# Xceon OS — A Reproducible Linux Distribution Project (KDE Edition)

[![CI: PR](https://img.shields.io/badge/CI-PR-blue)](#continuous-integration)
[![CI: Nightly](https://img.shields.io/badge/CI-Nightly-purple)](#release-channels)
[![License](https://img.shields.io/badge/License-See%20LICENSE-green)](#license)
[![Security Policy](https://img.shields.io/badge/Security-Policy-orange)](#security)
[![Build](https://img.shields.io/badge/Build-Reproducible-informational)](#reproducibility--supply-chain)
[![SBOM](https://img.shields.io/badge/SBOM-SPDX-lightgrey)](#sbom-software-bill-of-materials)

> **Xceon OS** is a Linux distribution project focused on **reproducible builds**, **clean defaults**, and **robust QA**.
> This repository contains everything required to build bootable ISO images (Live & Install), including
> profiles, overlays, kernel tooling, CI pipelines, and release artifacts.

---

## Table of Contents

- [What is Xceon OS?](#what-is-xceon-os)
- [Project Goals](#project-goals)
- [Editions & Profiles](#editions--profiles)
- [Features Overview](#features-overview)
- [Download](#download)
- [Verify Downloads](#verify-downloads)
- [Quick Start (Build in Docker)](#quick-start-build-in-docker)
- [Quick Start (Build on Host)](#quick-start-build-on-host)
- [Build System Overview](#build-system-overview)
- [Repository Layout](#repository-layout)
- [Profiles](#profiles)
- [Package Policy](#package-policy)
- [Kernel Policy](#kernel-policy)
- [Security](#security)
- [Reproducibility & Supply Chain](#reproducibility--supply-chain)
- [SBOM (Software Bill of Materials)](#sbom-software-bill-of-materials)
- [Quality Assurance (QA)](#quality-assurance-qa)
- [Continuous Integration](#continuous-integration)
- [Release Process](#release-process)
- [Roadmap](#roadmap)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Community & Support](#community--support)
- [License](#license)
- [Third-Party Notices](#third-party-notices)
- [Acknowledgements](#acknowledgements)

---

## What is Xceon OS?

Xceon OS is a distribution project that aims to be:

- **Reproducible**: Builds should be repeatable with pinned sources and consistent tooling.
- **Maintainable**: A structured repo, clear policies, validated configs, and automation.
- **Practical**: A daily-driver KDE desktop edition with sensible defaults.
- **Tested**: Every ISO must boot in headless QEMU smoke tests before it’s considered “green”.

This repository is designed for:
- Distro builders, maintainers, and contributors.
- Users who want transparent build scripts and verified artifacts.
- People who want a strong starting point for a real Linux distribution.

> NOTE: If you are here only to download ISO images, use **GitHub Releases** (this repo does not store ISO files in Git).

---

## Project Goals

### Primary goals
1. **Boot reliably** on a wide range of x86_64 hardware.
2. **Ship a polished KDE Plasma Live ISO** and an installer ISO.
3. **Minimize breakage** via strict config validation, robust build scripts, and CI smoke tests.
4. **Provide verifiable artifacts** (checksums, optional signatures, SBOM).

### Non-goals (for now)
- Shipping a custom package repository at scale (future possibility).
- Supporting every architecture from day one (start with x86_64, expand later).
- Heavy kernel patching (keep patches minimal and well-justified).

---

## Editions & Profiles

Xceon OS is built from **profiles**. A profile defines:
- Base and architecture
- Kernel choice/version and boot cmdline
- Package sets
- Root filesystem overlay
- Installer configuration (if applicable)
- Hooks executed during build

Example editions:
- **x86_64-kde-live**: Boot into KDE Plasma live environment
- **x86_64-kde-install**: Live environment + Calamares installer
- **x86_64-server-live**: Minimal server live ISO
- **aarch64-kde-live**: Optional multi-arch expansion

See: `docs/PROFILES.md`

---

## Features Overview

### Desktop experience (KDE Edition)
- KDE Plasma (Wayland default, X11 fallback)
- SDDM display manager
- PipeWire audio stack
- NetworkManager
- Curated default apps (configurable via package lists)

### System design
- Clear separation between:
  - **Profiles** (variants)
  - **Configs** (reusable defaults)
  - **Scripts** (build pipeline)
  - **Kernel** (pinned source/tooling)
- Reproducible build metadata and locked manifests
- Automated QEMU smoke tests (headless)

### Release artifacts
- ISO image(s)
- `SHA256SUMS`
- Optional signature: `SHA256SUMS.sig`
- Build metadata (`build-metadata.yml`)
- SBOM (`sbom.spdx.json`)

---

## Download

All official builds are published on the **Releases** page.

- Releases: `https://github.com/<YOUR-ORG-OR-USER>/Xceon-OS/releases`
- Latest stable: see the newest tag (e.g., `v0.3.0`)
- Nightly builds (optional): see CI artifacts or nightly release channel

> This repository does **not** commit `.iso` files. ISO files are stored as Release assets.

---

## Verify Downloads

### Verify checksums (required)
```bash
sha256sum -c SHA256SUMS