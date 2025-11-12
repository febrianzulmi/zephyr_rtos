# Zephyr RTOS pada ESP32 — Panduan Instalasi di Windows

Panduan ini menjelaskan langkah‑demi‑langkah untuk melakukan instalasi Zephyr RTOS di sistem Windows hingga berhasil membangun dan mengunggah program ke board ESP32.

> Referensi utama: [Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)

---

## Prasyarat

- **OS**: Windows 10 atau lebih baru  
- **Board**: ESP32 / ESP32‑DevKitC / ESP32‑S3  
- **Kabel USB** untuk koneksi ke PC  
- **Driver USB‑Serial** (CP210x / CH340 / FTDI)

---

## 1. Update Sistem & Terminal

1. Jalankan Windows Update.  
2. Instal *Windows Terminal* dari Microsoft Store.

---

## 2. Install Dependensi Host

Buka PowerShell (non‑admin) dan jalankan perintah berikut:

```powershell
winget install Kitware.CMake
winget install Ninja-build.Ninja
winget install oss-winget.gperf
winget install Python.Python.3.12
winget install Git.Git
winget install oss-winget.dtc
winget install wget
winget install 7zip.7zip
```

Verifikasi instalasi:

```powershell
cmake --version
python --version
dtc --version
```

Pastikan:  
- CMake ≥ 3.20.5  
- Python ≥ 3.10  
- DTC ≥ 1.4.6

---

## 3. Setup Workspace Zephyr

```powershell
cd %HOMEPATH%
python -m venv zephyrproject\.venv
zephyrproject\.venv\Scripts\Activate.ps1
pip install west
west init zephyrproject
cd zephyrproject
west update
west zephyr-export
west packages pip --install
```

Jika PowerShell menolak eksekusi script, jalankan:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## 4. Install Zephyr SDK

```powershell
west sdk install
```

Pastikan `ZEPHYR_SDK_INSTALL_DIR` sudah ter-set jika lokasi SDK tidak default.

---

## 5. Build Sample "Blinky" untuk ESP32

```powershell
cd %HOMEPATH%\zephyrproject\zephyr
west build -p always -b esp32_devkitc samples\basic\blinky
```

Output firmware akan muncul di:  
`build\zephyr\zephyr.bin`

---

## 6. Upload ke Board

1. Hubungkan board ESP32 ke PC via USB.  
2. Cek port COM di Device Manager.  
3. Flash firmware:

   ```powershell
   west flash
   ```

4. Setelah selesai, buka serial monitor (PuTTY/Tera Term) dengan baud rate 115200.

LED di board akan berkedip menandakan program berhasil dijalankan.

---

## 7. Langkah Selanjutnya

- Coba contoh lain di folder `samples/`
- Pelajari Devicetree dan Kconfig
- Gunakan `west build -t menuconfig` untuk konfigurasi tambahan

---

## Tips

- Hindari path dengan spasi. Gunakan `C:\zephyrproject`
- Jika board tidak terdeteksi, pastikan driver USB‑Serial sudah benar
- Gunakan PowerShell, bukan CMD klasik, agar path virtual environment terbaca dengan baik

---

Selamat! Anda telah berhasil menginstal Zephyr RTOS dan menjalankan program pertama di ESP32.
