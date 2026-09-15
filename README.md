<div align="center">

<img src="assets/logo/mpl.png" alt="MPL Indonesia" width="180" />

# Basis Data MPL Indonesia

**Sembilan musim MPL Indonesia (S10–S18) dalam satu berkas SQLite.**
680 laga · 1.611 game · 32.219 pick/ban · 320 pemain · 135 hero.

<p>
  <img src="https://img.shields.io/badge/SQLite-3.37%2B-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="sqlite" />
  <img src="https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="python" />
  <img src="https://img.shields.io/badge/STRICT%20tables-enabled-0f9d58?style=for-the-badge" alt="strict" />
  <img src="https://img.shields.io/badge/data-CC--BY--SA%203.0-EF9421?style=for-the-badge&logo=creativecommons&logoColor=white" alt="license" />
</p>

<p>
  <img src="https://img.shields.io/badge/musim-S10%20–%20S18-black?style=flat-square" alt="seasons" />
  <img src="https://img.shields.io/badge/tabel-16-black?style=flat-square" alt="tables" />
  <img src="https://img.shields.io/badge/view-9-black?style=flat-square" alt="views" />
  <img src="https://img.shields.io/badge/ukuran-4.8%20MB-black?style=flat-square" alt="size" />
</p>

</div>

---

## Apa ini

Basis data relasional yang dibangun ulang dari data publik Liquipedia dan id-mpl.com,
bukan sekadar kumpulan CSV yang di-`import`. Setiap tabel memakai mode `STRICT`,
setiap relasi punya kunci asing, dan `build.py` menolak menyatakan berhasil sebelum
jumlah baris di basis data sama persis dengan CSV asalnya.

`mpl.db` adalah **hasil, bukan sumber**. Jangan disunting langsung — ubah `schema.sql`
lalu build ulang, supaya tidak ada perubahan yang cuma hidup di dalam berkas biner.

---

## Angka besar

| | Baris | | Baris |
|---|---:|---|---:|
| `pick_ban` | 32.219 | `laga` | 680 |
| `hero_relasi` | 6.292 | `pemain` | 320 |
| `game` | 1.611 | `hero` | 135 |
| `roster` | 809 | `tim_musim` | 79 |
| `hero_musim` | 779 | `tim` | 11 |
| `award` | 611 | `musim` | 9 |

---

## Tim yang tercakup

<div align="center">

<img src="assets/logo/onic.png" height="52" alt="ONIC" />&nbsp;&nbsp;
<img src="assets/logo/rrq.png" height="52" alt="RRQ Hoshi" />&nbsp;&nbsp;
<img src="assets/logo/btr.png" height="52" alt="Bigetron by VIT" />&nbsp;&nbsp;
<img src="assets/logo/evos.png" height="52" alt="EVOS" />&nbsp;&nbsp;
<img src="assets/logo/ae.png" height="52" alt="Alter Ego" />&nbsp;&nbsp;
<img src="assets/logo/tlid.png" height="52" alt="Team Liquid ID" />&nbsp;&nbsp;
<img src="assets/logo/dewa.png" height="52" alt="Dewa United" />&nbsp;&nbsp;
<img src="assets/logo/navi.png" height="52" alt="NAVI" />

<sub>ONIC · RRQ Hoshi · Bigetron by VIT · EVOS · Alter Ego Esports · Team Liquid ID · Dewa United · NAVI · AURA Fire · Geek Fam ID · Rebellion Esports</sub>

</div>

---

## Mulai cepat

```bash
git clone https://github.com/agaollineed/mpl-id-database.git
cd mpl-id-database

sqlite3 database/mpl.db              # langsung buka
python database/build.py --periksa   # periksa integritas
python database/build.py             # bangun ulang dari dataset/*.csv
```

```python
import sqlite3, pandas as pd

con = sqlite3.connect("database/mpl.db")
pd.read_sql("SELECT * FROM v_klasemen WHERE season = 17", con)
```

Tidak perlu server, tidak perlu pemasangan. SQLite sudah ada di dalam Python, R,
DB Browser, dan DBeaver.

---

## Contoh hasil

**Hero paling dihormati tiap musim** (`queries/01_evolusi_meta_hero.sql`) — presence,
yaitu persentase game di mana hero itu di-pick **atau** di-ban, lebih jujur daripada
jumlah pick: hero yang terlalu kuat justru jarang ter-pick karena selalu dibuang lebih dulu.

| Musim | Hero | Pick | Ban | Presence | Win rate |
|---:|---|---:|---:|---:|---:|
| S10 | Wanwan | 46 | 126 | 100,0% | 60,87 |
| S11 | Joy | 11 | 163 | 100,0% | 45,45 |
| S14 | Chip | 7 | 205 | 100,0% | 42,86 |
| S17 | Freya | 38 | 167 | 100,0% | 60,53 |

**Dominasi tim lintas musim** (`queries/03_dominasi_tim.sql`):

| Tim | Musim | M–K | Win rate | Lolos playoff | Gelar |
|---|---:|---|---:|---|---:|
| ONIC | 9 | 98–35 | 73,7% | 8 / 8 | 6 |
| Bigetron by VIT | 9 | 81–51 | 61,4% | 8 / 8 | 1 |
| Team Liquid ID | 5 | 43–30 | 58,9% | 3 / 4 | 1 |
| RRQ Hoshi | 9 | 71–61 | 53,8% | 6 / 8 | 0 |

Empat kueri contoh lainnya ada di [`database/queries/`](database/queries).

---

## Isi repo

```
.
├── database/
│   ├── mpl.db        # hasil build (4,8 MB)
│   ├── schema.sql    # DDL: 16 tabel, kunci, CHECK, indeks
│   ├── views.sql     # 9 view analitik
│   ├── build.py      # pembangun + pemeriksa
│   ├── queries/      # 6 kueri riset contoh
│   └── README.md     # dokumentasi lengkap
├── dataset/          # CSV sumber
└── assets/logo/      # logo tim
```

📘 **[Dokumentasi lengkap ada di `database/README.md`](database/README.md)** — kamus data
per tabel, bagan keterkaitan, kesepakatan `NULL`, aturan simetri `hero_relasi`, dan
daftar batasan. Halaman ini sengaja tidak mengulangnya supaya tidak ada dua sumber
kebenaran yang bisa saling menyimpang.

---

## Yang belum ada

Baca sebelum menganalisis apa pun:

- **Tidak ada hasil per game.** 32.219 baris `pick_ban` tidak punya label menang/kalah,
  jadi "draft seperti apa yang menang" belum bisa dijawab dari baris mentah. Datanya ada
  di HTML sumber — ini lubang terbesar dalam dataset ini.
- **Tidak ada penghubung pemain ↔ hero.** `pick_ban` mencatat pick milik *tim*, bukan *pemain*.
- **Cakupan kolom tidak rata antar musim.** KDA pemain hanya S18; pick/ban hanya S10–S17.
  Jalankan `queries/06_cakupan_data.sql` lebih dulu.
- **Musim 1–9 belum ada.**

---

## Lisensi dan atribusi

| Bagian | Lisensi |
|---|---|
| Data (`database/mpl.db`, `dataset/`) | [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) — mengikuti [Liquipedia](https://liquipedia.net/mobilelegends/) |
| Kode (`build.py`, `schema.sql`, `views.sql`, `queries/`) | MIT |
| Logo tim | Merek dagang milik masing-masing organisasi, dipakai untuk identifikasi |

Data S10–S17 dari Liquipedia, musim berjalan dari [id-mpl.com](https://id-mpl.com).
**Karya turunan dari basis data ini wajib mempertahankan atribusi yang sama** — itu
syarat share-alike, bukan anjuran.

Repositori ini tidak berafiliasi dengan Moonton, MPL Indonesia, maupun tim mana pun.
