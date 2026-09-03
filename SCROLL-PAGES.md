# Scroll Pages — Splash/Intro Screen (Detail)

Dokumen pelengkap `SPEC.md` (§7 Ruang Lingkup Sistem) — berisi detail lengkap Splash/Intro Screen, dipisah ke file ini supaya SPEC.md tidak terlalu panjang. Mengadaptasi dua komponen referensi yang dikirim pengguna (kode React/Framer Motion) sebagai **satu scroll yang menyambung, bukan dua fitur terpisah** — komponen pertama (`SmoothScrollHero`) adalah bagian awal, komponen kedua (`parallax-scroll-feature-section`) adalah kelanjutannya di halaman yang sama. Status semua bagian di bawah: **[v1.1 — dikonfirmasi]**, kecuali ditandai lain.

## 1. Splash/Intro Screen

Layar pembuka **terpisah dari landing page utama**, tampil lebih dulu saat pengunjung membuka situs. Satu scroll menyambung, dua bagian:

**Bagian 1 — Pengenalan organisasi + Visi & Misi** (pola `SmoothScrollHero`): gambar besar yang menyusut/clip mengikuti scroll (logo Muhammadiyah), gambar-gambar pendukung bergerak parallax (logo PM/Aisyiyah/NA/IPM), teks pengenalan organisasi + Visi & Misi muncul dengan fade/slide saat discroll ke posisinya.

**Bagian 2 — Sejarah gerakan** (pola `parallax-scroll-feature-section`, kelanjutan langsung dari Bagian 1 di halaman yang sama): babak-babak sejarah **gerakan Pemuda Muhammadiyah secara keseluruhan** (BUKAN sejarah tiap organisasi/ortonom satu-satu) tersingkap berselang-seling kiri-kanan — foto tersingkap dengan efek clip-path + fade saat bagian itu discroll ke tengah layar, teks bergeser masuk (translate) bersamaan.

Diakhiri tombol **"Lanjutkan"** yang membawa pengunjung masuk ke landing page utama. Nav di Bagian 1 (aslinya link "LAUNCH SCHEDULE" yang scroll ke section berikutnya di komponen referensi) diadaptasi jadi tautan yang men-scroll ke Bagian 2 (mis. "Sejarah Kami ↓"), mengikuti mekanisme asli — bukan berpindah halaman.

**[v1.1 — dikonfirmasi, koreksi 3 September 2026]** "Sejarah" di sini BUKAN topik terpisah per organisasi seperti yang sempat ditulis di revisi sebelumnya — Sejarah hanya tampil sekali sebagai Bagian 2 dari Splash ini, menceritakan gerakan Pemuda Muhammadiyah secara umum. Tidak ada halaman "Sejarah" tersendiri per organisasi di Tentang Kami (§7 `SPEC.md`) — topik per-organisasi yang tersisa untuk dikelola dua-jalur hanyalah **Struktur Organisasi, Program Kerja, Agenda, Galeri**; **Visi & Misi** tetap ada sebagai konten terkunci ke kode di tempat lain (lihat `SPEC.md` §7), terpisah dari Visi & Misi umum yang muncul di Bagian 1 Splash ini.

Konten teks Bagian 1 & 2 (pengenalan organisasi, Visi & Misi umum, babak sejarah gerakan) termasuk kategori **identitas resmi yang dikunci ke kode** (lihat `SPEC.md` §7 catatan Landing Page), konsisten dengan keputusan Hero/Visi Misi sebelumnya.

### Perilaku tampil

**[v1.1 — usulan default, bisa disesuaikan]** Supaya tidak mengganggu pengunjung yang berulang kali membuka situs, splash ini sebaiknya hanya tampil sekali per sesi/perangkat (disimpan lewat penanda lokal di browser), dengan opsi lewati langsung ke landing page kapan saja — belum diminta eksplisit oleh pengguna, jadi masih bisa didiskusikan/diubah.

### Isi foto Bagian 1 — logo organisasi menggantikan foto demo

Isi foto Bagian 1 memakai logo organisasi, menggantikan foto SpaceX demo di komponen referensi. 5 slot foto komponen (1 gambar tengah *sticky* + 4 gambar parallax mosaic) diisi logo dari `assets/logos/` (folder proyek, lihat `SPEC.md` §7 "Struktur Organisasi (detail)") dengan urutan tetap:

1. **Gambar tengah (`CenterImage`, sticky, mulai kecil lalu membesar saat discroll)** — logo **Muhammadiyah** (`Logo-Muhammadiyah.svg`).
2. **Parallax mosaic slot 1** (kiri-atas, `w-1/3`) — logo **Pemuda Muhammadiyah** (`Logo-PM.svg`).
3. **Parallax mosaic slot 2** (tengah, terbesar, `w-2/3`) — logo **Aisyiyah** (`Logo-Aisyiyah.svg`).
4. **Parallax mosaic slot 3** (kanan, `w-1/3`) — logo **Nasyiatul Aisyiyah** (`Logo-NA.svg`).
5. **Parallax mosaic slot 4** (muncul belakangan saat scroll lanjut, `w-5/12`) — logo **IPM** (`Logo-IPM.svg`).

Sengaja hanya 5 dari 8 organisasi yang tampil di Bagian 1 (mengikuti jumlah slot foto bawaan komponen) — **Tapak Suci, Hizbul Wathan, dan IMM tidak perlu ditambahkan ke splash**, cukup muncul di grid pemilih organisasi landing page (`SPEC.md` §7) yang memang menampung ke-8 organisasi.

### Isi Bagian 2 — babak sejarah gerakan

Array babak sejarah (jumlahnya menyesuaikan sejarah gerakan, tidak harus tepat 3 seperti demo) berisi milestone gerakan Pemuda Muhammadiyah secara umum — bukan sejarah spesifik satu ortonom. Foto tiap babak disiapkan terpisah dari logo (bukan logo organisasi, tapi foto/dokumentasi sejarah), di-hardcode di kode frontend sesuai konten yang di-supply.

## 2. Dependency Frontend Tambahan

Khusus untuk Splash/Intro Screen di atas (kedua bagian), ditambahkan ke stack frontend (lihat `SPEC.md` §18.2):

- **Framer Motion** — animasi scroll-linked (`useScroll`, `useTransform`) untuk kedua bagian.
- **Lenis** (`lenis` / `@studio-freight/lenis`) — smooth-scroll di seluruh Splash.
- **react-icons** — ikon di Bagian 1 (Lucide React dipakai di Bagian 2 untuk indikator "SCROLL ↓", dan tetap ikon utama di tempat lain proyek).

Anime.js tetap dipertahankan untuk animasi lain sesuai `SPEC.md` §18.7 — **bukan diganti**, ini murni tambahan khusus Splash/Intro Screen.

## 3. Kode Referensi (as-received dari pengguna)

Kode di bawah ini **persis seperti yang dikirim pengguna, dua bagian yang menyambung jadi satu halaman** — dipakai sebagai acuan mekanisme animasi/interaksi (scroll parallax, clip-path reveal, sticky, dsb), **bukan untuk dipakai apa adanya**. Saat implementasi: seluruh gambar demo (foto SpaceX/Lorem Picsum) diganti sesuai pemetaan di §1 (logo organisasi untuk Bagian 1, foto sejarah gerakan untuk Bagian 2), seluruh teks placeholder (judul "PARALLAX SCROLL FEATURE SECTION", "Feature 1/2/3", Lorem ipsum, jadwal peluncuran SpaceX) diganti konten Pemuda Muhammadiyah yang sesungguhnya, `Schedule`/`ScheduleItem` (komponen kedua di Bagian 1) **dihapus/diganti** jadi transisi ke Bagian 2 (bukan jadwal peluncuran), dan path import (`@/components/ui/...`) disesuaikan ke struktur folder proyek ini (`src/features/landing/...` atau setara, lihat `SPEC.md` §18.4) — proyek ini React 19 + Vite (bukan Next.js/shadcn default), jadi arahan setup shadcn CLI di prompt asal tidak dipakai mentah, cukup pastikan Tailwind + dependency (`framer-motion`, `lenis`, `react-icons`, `lucide-react`) terpasang sesuai §2.

### 3.1 Bagian 1 — referensi `SmoothScrollHero`

Install dependency: `lenis`, `react-icons`, `framer-motion`

```tsx
// modern-hero.tsx
import { ReactLenis } from "lenis/dist/lenis-react";
import {
  motion,
  useMotionTemplate,
  useScroll,
  useTransform,
} from "framer-motion";
import { SiSpacex } from "react-icons/si";
import { FiArrowRight, FiMapPin } from "react-icons/fi";
import { useRef } from "react";

export const SmoothScrollHero = () => {
  return (
    <div className="bg-zinc-950">
      <Nav />
      <Hero />
      <Schedule />
    </div>
  );
};

const Nav = () => {
  return (
    <nav className="fixed left-0 right-0 top-0 z-50 flex items-center justify-between px-6 py-3 text-white">
      <SiSpacex className="text-3xl mix-blend-difference" />
      <button
        onClick={() => {
          document.getElementById("launch-schedule")?.scrollIntoView({
            behavior: "smooth",
          });
        }}
        className="flex items-center gap-1 text-xs text-zinc-400"
      >
        LAUNCH SCHEDULE <FiArrowRight />
      </button>
    </nav>
  );
};

const SECTION_HEIGHT = 1500;

const Hero = () => {
  return (
    <div
      style={{ height: `calc(${SECTION_HEIGHT}px + 100vh)` }}
      className="relative w-full"
    >
      <CenterImage />
      <ParallaxImages />
      <div className="absolute bottom-0 left-0 right-0 h-96 bg-gradient-to-b from-zinc-950/0 to-zinc-950" />
    </div>
  );
};

const CenterImage = () => {
  const { scrollY } = useScroll();
  const clip1 = useTransform(scrollY, [0, 1500], [25, 0]);
  const clip2 = useTransform(scrollY, [0, 1500], [75, 100]);
  const clipPath = useMotionTemplate`polygon(${clip1}% ${clip1}%, ${clip2}% ${clip1}%, ${clip2}% ${clip2}%, ${clip1}% ${clip2}%)`;
  const backgroundSize = useTransform(
    scrollY,
    [0, SECTION_HEIGHT + 500],
    ["170%", "100%"]
  );
  const opacity = useTransform(
    scrollY,
    [SECTION_HEIGHT, SECTION_HEIGHT + 500],
    [1, 0]
  );

  return (
    <motion.div
      className="sticky top-0 h-screen w-full"
      style={{
        clipPath,
        backgroundSize,
        opacity,
        backgroundImage:
          "url(https://cdn.21st.dev/assets/mirror/3e/3ea1ee3ce8a8134c194baf880a7afabc7a43bad8d5672f7b0f74fff87344dcc4.jpg)",
        backgroundPosition: "center",
        backgroundRepeat: "no-repeat",
      }}
    />
  );
};

const ParallaxImages = () => {
  return (
    <div className="mx-auto max-w-5xl px-4 pt-[200px]">
      <ParallaxImg
        src="https://cdn.21st.dev/assets/mirror/68/68fd4edf19855762d0020e6ddbf3fdd31b7f768a6c65014d50ec6b36ef305b54.jpg"
        alt="And example of a space launch"
        start={-200}
        end={200}
        className="w-1/3"
      />
      <ParallaxImg
        src="https://cdn.21st.dev/assets/mirror/bc/bca64f76b38b6b3e0f1c2357292903fc428e16d47b49005201be8ba51377ce8c.jpg"
        alt="An example of a space launch"
        start={200}
        end={-250}
        className="mx-auto w-2/3"
      />
      <ParallaxImg
        src="https://cdn.21st.dev/assets/mirror/04/04691b2e29925f30eac3817ea8f65d973484b711822252a13c15248859e464da.jpg"
        alt="Orbiting satellite"
        start={-200}
        end={200}
        className="ml-auto w-1/3"
      />
      <ParallaxImg
        src="https://cdn.21st.dev/assets/mirror/71/711f1a9ccb3786dcc00e8031191dc4d58c9377cefb54bf927806921a4a05a818.jpg"
        alt="Orbiting satellite"
        start={0}
        end={-500}
        className="ml-24 w-5/12"
      />
    </div>
  );
};

const ParallaxImg = ({ className, alt, src, start, end }) => {
  const ref = useRef(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: [`${start}px end`, `end ${end * -1}px`],
  });
  const opacity = useTransform(scrollYProgress, [0.75, 1], [1, 0]);
  const scale = useTransform(scrollYProgress, [0.75, 1], [1, 0.85]);
  const y = useTransform(scrollYProgress, [0, 1], [start, end]);
  const transform = useMotionTemplate`translateY(${y}px) scale(${scale})`;

  return (
    <motion.img
      src={src}
      alt={alt}
      className={className}
      ref={ref}
      style={{ transform, opacity }}
    />
  );
};

const Schedule = () => {
  return (
    <section
      id="launch-schedule"
      className="mx-auto max-w-5xl px-4 py-48 text-white"
    >
      <motion.h1
        initial={{ y: 48, opacity: 0 }}
        whileInView={{ y: 0, opacity: 1 }}
        transition={{ ease: "easeInOut", duration: 0.75 }}
        className="mb-20 text-4xl font-black uppercase text-zinc-50"
      >
        Launch Schedule
      </motion.h1>
      <ScheduleItem title="NG-21" date="Dec 9th" location="Florida" />
      <ScheduleItem title="Starlink" date="Dec 20th" location="Texas" />
      <ScheduleItem title="Starlink" date="Jan 13th" location="Florida" />
      <ScheduleItem title="Turksat 6A" date="Feb 22nd" location="Florida" />
      <ScheduleItem title="NROL-186" date="Mar 1st" location="California" />
      <ScheduleItem title="GOES-U" date="Mar 8th" location="California" />
      <ScheduleItem title="ASTRA 1P" date="Apr 8th" location="Texas" />
    </section>
  );
};

const ScheduleItem = ({ title, date, location }) => {
  return (
    <motion.div
      initial={{ y: 48, opacity: 0 }}
      whileInView={{ y: 0, opacity: 1 }}
      transition={{ ease: "easeInOut", duration: 0.75 }}
      className="mb-9 flex items-center justify-between border-b border-zinc-800 px-3 pb-9"
    >
      <div>
        <p className="mb-1.5 text-xl text-zinc-50">{title}</p>
        <p className="text-sm uppercase text-zinc-500">{date}</p>
      </div>
      <div className="flex items-center gap-1.5 text-end text-sm uppercase text-zinc-500">
        <p>{location}</p>
        <FiMapPin />
      </div>
    </motion.div>
  );
};
```

```tsx
// demo.tsx
import { SmoothScrollHero } from "@/components/ui/modern-hero";

const DemoOne = () => {
  return <SmoothScrollHero />;
};

export { DemoOne };
```

**Catatan adaptasi untuk proyek ini** (lihat §1): `Nav` diganti jadi tautan "Sejarah Kami ↓" yang scroll ke Bagian 2 (bukan link "Launch Schedule"), `CenterImage.backgroundImage` diisi `Logo-Muhammadiyah.svg`, keempat `ParallaxImg` diisi `Logo-PM.svg` / `Logo-Aisyiyah.svg` / `Logo-NA.svg` / `Logo-IPM.svg` sesuai urutan di §1, teks pengenalan organisasi + Visi & Misi ditambahkan di atas/berdekatan dengan `CenterImage`, dan `Schedule`/`ScheduleItem` **dihapus** — digantikan langsung oleh Bagian 2 (§3.2) sebagai kelanjutan scroll, bukan section jadwal peluncuran.

### 3.2 Bagian 2 — referensi `parallax-scroll-feature-section` (kelanjutan Bagian 1)

Install dependency: `lucide-react`, `framer-motion`

```tsx
// parallax-scroll-feature-section.tsx
'use client'
import { useRef } from "react"
import { motion, useScroll, useTransform } from 'framer-motion'
import { ArrowDown } from "lucide-react"
import { cn } from "@/lib/utils";
import { useState } from "react";

export const Component = () => {
    // Array of section data
    const sections = [
        {
            id: 1,
            title: "Feature 1",
            description: "Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ab maxime sequi, pariatur illum, adipisci ullam optio quod tempora necessitatibus consectetur eaque deleniti id totam possimus unde dolorum inventore incidunt. Ea.",
            imageUrl: 'https://cdn.cosmos.so/6c4a7829-d16a-4a58-9ab9-93fbb3bacb9e.?format=jpeg',
            reverse: false
        },
        {
            id: 2,
            title: "Feature 2",
            description: "Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ab maxime sequi, pariatur illum, adipisci ullam optio quod tempora necessitatibus consectetur eaque deleniti id totam possimus unde dolorum inventore incidunt. Ea.",
            imageUrl: 'https://cdn.cosmos.so/f827788c-038d-4257-8167-759e819f846d?format=jpeg',
            reverse: true
        },
        {
            id: 3,
            title: "Feature 3",
            description: "Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ab maxime sequi, pariatur illum, adipisci ullam optio quod tempora necessitatibus consectetur eaque deleniti id totam possimus unde dolorum inventore incidunt. Ea.",
            imageUrl: 'https://cdn.cosmos.so/20351bef-4a9c-4dcc-81d8-e59c84058944?format=jpeg',
            reverse: false
        }
    ]
    // Create refs and animations for each section
    const sectionRefs = sections.map(() => useRef(null));

    const scrollYProgress = sections.map((_, index) => {
        return useScroll({
            target: sectionRefs[index],
            offset: ["start end", "center start"]
        }).scrollYProgress;
    });

    // Create animations for each section
    const opacityContents = scrollYProgress.map(progress =>
        useTransform(progress, [0, 0.7], [0, 1])
    );

    const clipProgresses = scrollYProgress.map(progress =>
        useTransform(progress, [0, 0.7], ["inset(0 100% 0 0)", "inset(0 0% 0 0)"])
    );

    const translateContents = scrollYProgress.map(progress =>
        useTransform(progress, [0, 1], [-50, 0])
    );

  return (
    <div>
      <div className='min-h-screen w-screen flex flex-col items-center justify-center'>
        <h1 className='text-6xl max-w-2xl text-center'>PARALLAX SCROLL FEATURE SECTION</h1>
        <p className='mt-20 flex items-center gap-1.5 text-sm'>SCROLL <ArrowDown size={15} /></p>
      </div>
      {/* <ScrollSection /> */}
       <div className="flex flex-col md:px-0 px-10">
            {sections.map((section, index) => (
                <div
                    key={section.id}
                    ref={sectionRefs[index]}
                    className={`h-screen flex items-center justify-center md:gap-40 gap-20 ${section.reverse ? 'flex-row-reverse' : ''}`}
                >
                    <motion.div style={{ y: translateContents[index] }}>
                        <div className="text-6xl max-w-sm">{section.title}</div>
                        <motion.p
                            style={{ y: translateContents[index] }}
                            className="text-white/70 max-w-sm mt-10"
                        >
                            {section.description}
                        </motion.p>
                    </motion.div>
                    <motion.div
                        style={{
                            opacity: opacityContents[index],
                            clipPath: clipProgresses[index],
                        }}
                        className="relative"
                    >
                        <img
                            src={section.imageUrl}
                            className="size-80 object-cover"
                            alt={`Section ${section.id}` }
                        />
                    </motion.div>
                </div>
            ))}
        </div>
      <div>
      </div>
       <div className='min-h-screen w-screen flex flex-col items-center justify-center'>
        <h1 className='text-8xl'>The End</h1>
      </div>
    </div>
  );
};
```

```tsx
// demo.tsx
import { Component } from "@/components/ui/parallax-scroll-feature-section";

export default function DemoOne() {
  return <Component />;
}
```

**Catatan adaptasi untuk proyek ini** (lihat §1): array `sections` diisi babak-babak Sejarah **gerakan Pemuda Muhammadiyah secara umum** (bukan per organisasi/ortonom, dan bukan "Feature 1/2/3" + Lorem ipsum) — jumlah babak menyesuaikan sejarah gerakan, tidak harus tepat 3; judul pembuka "PARALLAX SCROLL FEATURE SECTION" diganti "Sejarah Kami" atau setara; layar penutup "The End" diganti tombol **"Lanjutkan"** yang menavigasi ke landing page utama (penutup dari keseluruhan Splash, bukan cuma penutup Bagian 2). Struktur reveal (`clipPath`, `opacity`, `translateY` per section, alternating `reverse`) dipakai apa adanya — ini bagian mekanisme yang memang mau ditiru.
