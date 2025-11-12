# Zephyr RTOS pada ESP32 — Panduan Instalasi di Windows (CMD Version)

Panduan ini menjelaskan langkah-demi-langkah untuk instalasi Zephyr RTOS di **Windows (Command Prompt)** hingga berhasil membangun dan mengunggah program ke board ESP32.

> Referensi resmi: [Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)

---

## Prasyarat

- **OS**: Windows 10 atau lebih baru  
- **Board**: ESP32 / ESP32-DevKitC / ESP32-S3  
- **Kabel USB** untuk koneksi ke PC  
- **Driver USB-Serial** (CP210x / CH340 / FTDI)

---

## 1. Update Sistem & Terminal

1. Jalankan Windows Update.  
2. Instal *Windows Terminal* dari Microsoft Store (opsional, agar tampilan lebih baik).

---

## 2. Install Dependensi Host

Buka **Command Prompt (cmd.exe)** dan jalankan perintah berikut satu per satu:

```cmd
winget install Kitware.CMake
winget install Ninja-build.Ninja
winget install oss-winget.gperf
winget install Python.Python.3.12
winget install Git.Git
winget install oss-winget.dtc
winget install wget
winget install 7zip.7zip
```

Verifikasi hasil instalasi:

```cmd
cmake --version
python --version
dtc --version
```

Pastikan versi minimal:
- CMake ≥ 3.20.5  
- Python ≥ 3.10  
- DTC ≥ 1.4.6  

---

## 3. Setup Workspace Zephyr

1. Masuk ke direktori home:

   ```cmd
   cd %HOMEPATH%
   ```

2. Buat virtual environment:

   ```cmd
   python -m venv zephyrproject\.venv
   ```

3. Aktifkan environment:

   ```cmd
   zephyrproject\.venv\Scripts\activate
   ```

4. Instal dan inisialisasi Zephyr:

   ```cmd
   pip install west
   west init zephyrproject
   cd zephyrproject
   west update
   west zephyr-export
   west packages pip --install
   ```

> Catatan: Di `cmd`, gunakan `\` untuk path (bukan `/`).

---

## 4. Install Zephyr SDK

```cmd
west sdk install
```

SDK akan otomatis diunduh dan dipasang.  
Jika lokasi instalasi bukan default, Anda bisa atur environment variable `ZEPHYR_SDK_INSTALL_DIR`.

---

## 5. Build Sample “Blinky” untuk ESP32

Masuk ke folder Zephyr dan jalankan build:

```cmd
cd %HOMEPATH%\zephyrproject\zephyr
west build -p always -b esp32_devkitc samples\basic\blinky
```

Jika build berhasil, file firmware akan muncul di:
```
build\zephyr\zephyr.bin
```

---

## 6. Upload ke Board (Flash)

1. Hubungkan board ESP32 ke PC via USB.  
2. Cek port COM di **Device Manager**.  
3. Jalankan perintah berikut dari direktori build:

   ```cmd
   west flash
   ```

Jika berhasil, LED pada board akan mulai berkedip (sample Blinky).

---

## 7. Cek Output Serial

Gunakan program seperti **PuTTY**, **Tera Term**, atau **Windows Terminal** dengan baud rate **115200**.  
Anda akan melihat log dari aplikasi Zephyr.

---

## 8. Langkah Selanjutnya

- Coba project lain di folder `samples/`  
- Gunakan `west build -t menuconfig` untuk konfigurasi tambahan  
- Pelajari Devicetree, Kconfig, dan subsystem Zephyr  

---

## Tips

- Hindari folder dengan spasi di path, gunakan misalnya `C:\zephyrproject`  
- Pastikan driver USB-serial (CH340, CP210x, atau FTDI) sudah terinstal  
- Jalankan semua perintah di **cmd biasa**, tidak perlu administrator  

---

Selamat! 🎉  
Anda telah berhasil menginstal **Zephyr RTOS** dan menjalankan program pertama di **ESP32** melalui Command Prompt Windows.
