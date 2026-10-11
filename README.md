# 🌙 Kurikulum Ilmu Falak Berbasis Python

Berikut kurikulum yang saya susun secara bertahap, dari dasar hingga aplikasi. Setiap modul memadukan **konsep falak klasik** dengan **praktik komputasi Python** 🐍

---

## 📌 Gambaran Umum

| Aspek | Keterangan |
|---|---|
| **Durasi** | 12 pertemuan (±3 bulan, 1 pertemuan/minggu) |
| **Level** | Pemula → Menengah |
| **Prasyarat** | Dasar Python (variabel, fungsi, perulangan), trigonometri SMA |
| **Pustaka Python** | `math`, `numpy`, `matplotlib`, `datetime`, `skyfield`, `pandas` |
| **Luaran akhir** | Aplikasi kalkulator waktu salat, arah kiblat, dan visibilitas hilal |

---

## 🎯 Capaian Pembelajaran

Setelah menyelesaikan kurikulum ini, peserta mampu:

1. Menjelaskan konsep dasar ilmu falak dan kegunaannya dalam ibadah.
2. Menerapkan trigonometri bola pada perhitungan posisi benda langit.
3. Menghitung waktu salat, arah kiblat, dan awal bulan Hijriah dengan Python.
4. Memahami metode klasik (jaib/sittini, Rubu' Mujayyab) dan memadankannya dengan algoritma modern.
5. Membangun visualisasi dan aplikasi falak sederhana.

---

## 🧱 Bagian I — Fondasi

### Modul 1: Pengantar Ilmu Falak 📖
- Definisi, sejarah, dan ruang lingkup ilmu falak
- Dalil dan urgensi falak dalam ibadah (salat, puasa, haji)
- Tradisi falak di pesantren dan Nusantara
- Istilah dasar: *hisab*, *rukyat*, *ephemeris*, *markaz*

**Praktik Python:** Pengaturan lingkungan (Python, Jupyter, virtual environment).

### Modul 2: Matematika Dasar Falak 📐
- Sistem satuan sudut (derajat, menit, detik) dan konversi
- Trigonometri: sin, cos, tan, dan identitas dasarnya
- Jaib dan jaib mankus (sinus dalam tradisi klasik)
- Tabel *sittini* (tabel sinus 60 bagian)

**Praktik Python:**
```python
import math

def dms_ke_desimal(derajat, menit, detik):
    """Konversi derajat-menit-detik ke derajat desimal."""
    tanda = -1 if derajat < 0 else 1
    return tanda * (abs(derajat) + menit / 60 + detik / 3600)

def desimal_ke_dms(nilai):
    """Konversi derajat desimal ke derajat-menit-detik."""
    tanda = -1 if nilai < 0 else 1
    nilai = abs(nilai)
    d = int(nilai)
    m = int((nilai - d) * 60)
    s = round(((nilai - d) * 60 - m) * 60, 2)
    return tanda * d, m, s

# Tabel jaib sederhana (sinus dengan jari-jari 60)
def jaib(sudut_derajat, jari_jari=60):
    return jari_jari * math.sin(math.radians(sudut_derajat))

for sudut in range(0, 91, 15):
    print(f"Jaib {sudut:>2}° = {jaib(sudut):.4f}")
```

### Modul 3: Interpolasi dan Tabel Klasik 🔢
- Konsep interpolasi linear dan nonlinear
- Cara membaca dan menggunakan tabel sinus klasik
- Ketelitian dan galat perhitungan

**Praktik Python:**
```python
def interpolasi_linear(x, x0, x1, y0, y1):
    """Interpolasi linear antara dua titik tabel."""
    return y0 + (x - x0) * (y1 - y0) / (x1 - x0)

# Contoh: mencari jaib 32° dari tabel 30° dan 35°
y30, y35 = jaib(30), jaib(35)
print(f"Jaib 32° (interpolasi): {interpolasi_linear(32, 30, 35, y30, y35):.4f}")
print(f"Jaib 32° (eksak)      : {jaib(32):.4f}")
```

---

## 🌐 Bagian II — Astronomi Bola

### Modul 4: Bola Langit dan Sistem Koordinat 🔭
- Bola langit, ekuator, ekliptika, zenit, horizon
- Koordinat geografis: lintang (φ) dan bujur (λ)
- Koordinat horizon: azimut dan tinggi
- Koordinat ekuatorial: asensio rekta dan deklinasi
- Sudut waktu (*hour angle*)

**Praktik Python:** Visualisasi bola langit sederhana dengan `matplotlib`.

### Modul 5: Trigonometri Bola ⚪
- Segitiga bola dan unsur-unsurnya
- Rumus cosinus dan sinus pada segitiga bola
- Penerapan pada posisi benda langit

**Praktik Python:**
```python
import math

def jarak_sudut_bola(lat1, lon1, lat2, lon2):
    """Jarak sudut dua titik pada bola (derajat) dengan rumus cosinus bola."""
    p1, p2 = math.radians(lat1), math.radians(lat2)
    dl = math.radians(lon2 - lon1)
    c = math.sin(p1) * math.sin(p2) + math.cos(p1) * math.cos(p2) * math.cos(dl)
    return math.degrees(math.acos(max(-1, min(1, c))))
```

### Modul 6: Matahari — Deklinasi dan Equation of Time ☀️
- Gerak semu tahunan Matahari
- Deklinasi Matahari sepanjang tahun
- Perata waktu (*equation of time*)
- Hubungan dengan musim dan panjang siang

**Praktik Python:**
```python
import math
import numpy as np
import matplotlib.pyplot as plt

def deklinasi_matahari(hari_ke):
    """Aproksimasi deklinasi Matahari (derajat) berdasarkan hari ke-n."""
    return -23.44 * math.cos(math.radians(360 / 365 * (hari_ke + 10)))

hari = np.arange(1, 366)
dekl = [deklinasi_matahari(h) for h in hari]

plt.plot(hari, dekl)
plt.title("Deklinasi Matahari Sepanjang Tahun")
plt.xlabel("Hari ke-")
plt.ylabel("Deklinasi (°)")
plt.grid(True)
plt.show()
```

---

## 🕌 Bagian III — Aplikasi Ibadah

### Modul 7: Arah Kiblat 🧭
- Konsep arah kiblat dan koordinat Ka'bah (21°25′21″ LU, 39°49′34″ BT)
- Metode segitiga bola
- Alat bantu klasik: Rubu' Mujayyab, bayangan kiblat

**Praktik Python:**
```python
import math

KAABAH_LAT, KAABAH_LON = 21.4225, 39.8262

def arah_kiblat(lat, lon):
    """Azimut kiblat (derajat dari Utara searah jarum jam)."""
    phi = math.radians(lat)
    dl = math.radians(KAABAH_LON - lon)
    phi_k = math.radians(KAABAH_LAT)
    y = math.sin(dl)
    x = math.cos(phi) * math.tan(phi_k) - math.sin(phi) * math.cos(dl)
    return (math.degrees(math.atan2(y, x)) + 360) % 360

# Contoh: Sedan, Rembang (perkiraan)
print(f"Arah kiblat: {arah_kiblat(-6.80, 111.58):.2f}° dari Utara")
```

### Modul 8: Waktu Salat ⏰
- Kriteria awal dan akhir tiap waktu salat
- Sudut depresi Subuh dan Isya (kriteria Kemenag RI: Subuh −20°, Isya −18°)
- Ikhtiyat dan koreksi tinggi tempat
- Zona waktu dan koreksi bujur

**Praktik Python:** Kalkulator waktu salat lengkap dengan koreksi *equation of time*.

### Modul 9: Rubu' Mujayyab dan Alat Falak Klasik 🪬
- Struktur dan fungsi Rubu' Mujayyab
- Cara menghitung tinggi, azimut, dan waktu dengan alat klasik
- Perbandingan hasil alat klasik dengan hitungan komputasi

**Praktik Python:** Simulasi Rubu' Mujayyab dan verifikasi hasil dengan fungsi trigonometri.

---

## 🌒 Bagian IV — Kalender dan Hilal

### Modul 10: Kalender Hijriah 📅
- Sistem kalender Hijriah (urfi vs hakiki)
- Algoritma konversi Masehi ↔ Hijriah
- Perbedaan metode penentuan awal bulan

**Praktik Python:** Konverter tanggal Masehi–Hijriah.

### Modul 11: Hilal dan Visibilitas 🌙
- Konjungsi (ijtimak), elongasi, umur bulan
- Iluminasi hilal
- Kriteria visibilitas: Wujudul Hilal, MABIMS (tinggi 3° dan elongasi 6,4°), dan lainnya
- Penggunaan `skyfield` untuk data efemeris Bulan dan Matahari

**Praktik Python:**
```python
import math

def iluminasi_hilal(elongasi_derajat):
    """Fraksi iluminasi Bulan (0–1) dari elongasi geosentris."""
    return (1 - math.cos(math.radians(elongasi_derajat))) / 2

for e in [5, 6.4, 10, 20]:
    print(f"Elongasi {e:>4}° → iluminasi {iluminasi_hilal(e) * 100:.3f}%")
```

### Modul 12: Proyek Akhir 🏆
Peserta membangun salah satu proyek berikut:

1. **Kalkulator falak lengkap** (kiblat + waktu salat + hilal)
2. **Website falak interaktif** dengan visualisasi SVG
3. **Dashboard hilal** per bulan Hijriah dengan `pandas` dan `matplotlib`

---

## 📝 Evaluasi

| Komponen | Bobot |
|---|---|
| Latihan praktik tiap modul | 30% |
| Kuis konsep (Modul 1–9) | 20% |
| Proyek akhir | 40% |
| Keaktifan dan diskusi | 10% |

---

## 📚 Referensi yang Disarankan

- Jean Meeus, *Astronomical Algorithms*
- Kemenag RI, *Ephemeris Hisab Rukyat*
- Kitab falak klasik (misalnya *Sullamun Nayyirain*, *Durusul Falakiyah*)
- Dokumentasi `skyfield`: rhodesmill.org/skyfield

---

Mohon maaf jika ada yang ingin disesuaikan 🙏 Apakah kurikulum ini perlu diubah, misalnya **dipadatkan menjadi 6 pertemuan**, **difokuskan untuk santri pemula**, atau **ditambah modul gerhana**? Jika berkenan, saya juga dapat menyiapkannya dalam bentuk dokumen atau halaman web.