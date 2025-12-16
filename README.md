# gm-packages

OpenWrt packages collection for Chinese GM (GuoMi/国密) cryptographic algorithms, mobile network management, and system utilities.

## 📦 Package Categories

### 🔐 GM Cryptographic Packages

#### gm-fm (Fisherman Crypto Card Driver)
- Kernel driver for Fisherman GM crypto cards (SM1/SM3/SM4)
- PCI/PCIe hardware acceleration support
- Integration with Linux Crypto API (`/proc/crypto`)
- Supports multiple card models (FM1208, FM1218, etc.)

**Install:**
```bash
opkg install kmod-gm-fm gm-fm-test
```

#### gm-sansec (Sansec Crypto Card Driver)
- Sansec (三未信安) SKF crypto card driver
- SM2/SM3/SM4 algorithm support
- User-space tools and test utilities
- StrongSwan IPsec integration

**Install:**
```bash
opkg install kmod-gm-sansec gm-sansec-test
```

#### gm-test
Common testing tools for GM algorithm validation.

### 📱 Mobile Network Tools

#### quectel-CM
Connection manager for Quectel 4G/LTE modules.

**Features:**
- QMI/MBIM protocol support
- UBUS status reporting interface
- Automatic reconnection
- QMAP bridge mode
- 4G LED indicator management

**Configuration (`/etc/config/quectel`):**
```
config quectel 'global'
    option enabled '1'
    option device '/dev/cdc-wdm0'
    option apn 'internet'
    option pdp_type 'IP'        # IP/IPV6/IPV4V6
    option auth_type 'NONE'     # NONE/PAP/CHAP
    option username ''
    option password ''
```

**UBUS API:**
```bash
ubus call quectel status
# Returns: network state, signal strength, operator info
```

**Install:**
```bash
opkg install quectel-cm
```

### 🛠️ System Utilities

#### njmon (Nigel's Performance Monitor)
System performance monitoring tool with JSON output format.

**Features:**
- CPU, memory, disk, network metrics
- JSON structured output (easy integration with ELK/Grafana)
- Low resource consumption

**Usage:**
```bash
njmon -s 10 -c 60 > stats.json  # Collect every 10s for 60 samples
```

#### pwclient
Command-line client for Linux kernel Patchwork system.

#### topologyd
Network topology information daemon.

#### openwrt-upx
UPX (Ultimate Packer for eXecutables) for compressing binaries.

#### pcapplusplus
High-performance packet capture and processing library.

## 🚀 Installation

### Add Feed to OpenWrt

Edit `feeds.conf.default`:
```
src-git gmpackages https://github.com/superice119/gm-packages.git
```

Update and install:
```bash
./scripts/feeds update gmpackages
./scripts/feeds install -a -p gmpackages
make menuconfig  # Select packages under "GM Packages"
make package/feeds/gmpackages/<package-name>/compile V=s
```

### Pre-built Packages

Check [Releases](https://github.com/superice119/gm-packages/releases) for pre-compiled IPKs (if available).

## 📋 Requirements

- **OpenWrt 21.02+** (tested on 22.03/23.05)
- **Target Platform:** Primarily x86_64 (some packages support other architectures)
- **Kernel Modules:** CONFIG_CRYPTO_USER_API_* for GM crypto drivers
- **Dependencies:** Automatically resolved by OpenWrt build system

## 🔧 Hardware Support

### GM Crypto Cards
- **Fisherman:** FM1208, FM1218, FM1228 series
- **Sansec:** SKF-compliant cards (PCIe interface)

### Cellular Modules
- **Quectel:** EC20, EC25, EP06, EM05, RM500Q, RG50xQ series
- **Interface:** USB (QMI/MBIM mode required)

## 📖 Documentation

- [GM Algorithm Standards (GM/T)](http://www.gmbz.org.cn/)
- [Quectel Module Documentation](https://www.quectel.com/support/download)
- [OpenWrt Package Development Guide](https://openwrt.org/docs/guide-developer/packages)

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create feature branch (`git checkout -b feature/xxx`)
3. Test on OpenWrt build system
4. Submit Pull Request

## 📜 License

Most packages inherit licenses from upstream projects:
- **GM drivers:** Check individual package Makefiles
- **quectel-CM:** Apache-2.0 (from Quectel official SDK)
- **njmon:** GPL-3.0
- **pwclient:** GPL-2.0

See `LICENSE` or package-specific headers for details.

## ⚠️ Disclaimer

GM cryptographic modules are intended for compliance with Chinese cryptographic regulations. Users are responsible for proper key management and regulatory compliance.

## 📧 Contact

- **Issues:** [GitHub Issues](https://github.com/superice119/gm-packages/issues)
- **Maintainer:** [@superice119](https://github.com/superice119)

---

**Keywords:** OpenWrt, 国密算法, SM2, SM3, SM4, Quectel, 4G LTE, crypto hardware, embedded Linux

