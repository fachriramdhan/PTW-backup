# PTW - Sistem Permit to Work

> **Sistem Permit to Work (PTW) Berbasis Web untuk Industri Manufaktur**

Sistem kontrol izin kerja berisiko yang aman, terkendali, dan terdokumentasi dengan baik untuk lingkungan industri manufaktur.

---

## 🎯 Tentang Proyek

Sistem PTW adalah solusi digital untuk mengelola izin kerja berisiko tinggi di lingkungan manufaktur. Sistem ini menggantikan proses manual berbasis kertas dengan platform web yang terpusat, mudah diaudit, dan dapat digunakan langsung di lapangan.

### Tujuan Sistem

- ✅ Mencegah pekerjaan berisiko tanpa izin
- ✅ Memastikan pelaksanaan JSEA sebelum pekerjaan
- ✅ Menjamin proses approval berjenjang
- ✅ Mengontrol dan memonitor pekerjaan aktif
- ✅ Menyediakan bukti audit K3 yang valid

---

## 🔍 Latar Belakang

### Masalah yang Diselesaikan

Di industri manufaktur, pengelolaan Permit to Work secara manual menimbulkan berbagai permasalahan:

- ❌ **Tidak real-time** - Informasi terlambat sampai ke pihak terkait
- ❌ **Sulit ditelusuri** - Jejak audit tidak jelas
- ❌ **Rentan pelanggaran K3** - Kontrol lemah terhadap prosedur keselamatan
- ❌ **Dokumentasi tidak terpusat** - Data tersebar di kertas dan Excel

### Solusi

Sistem PTW berbasis web yang:

- ✅ Terpusat dan accessible dari mana saja
- ✅ Terdokumentasi dengan baik
- ✅ Mudah diaudit dengan trail lengkap
- ✅ User-friendly untuk penggunaan di lapangan

---

## ⚡ Fitur Utama

### 1. **Permit Management**
- Pengajuan permit kerja digital
- Template berbeda untuk setiap jenis pekerjaan
- Auto-generate nomor permit
- Status tracking real-time

### 2. **JSEA (Job Safety & Environmental Analysis)**
- Form analisis bahaya terstruktur
- Identifikasi risiko per langkah kerja
- Pengendalian risiko yang jelas
- Validasi kelengkapan sebelum submit

### 3. **Approval Workflow**
- Alur persetujuan berjenjang otomatis
- Supervisor → Area Owner → HSE → Authorizer
- Email/notifikasi otomatis
- Reject, approve, atau request revision

### 4. **Safety Checklist**
- Checklist K3 dinamis sesuai jenis pekerjaan
- Validasi APD dan peralatan
- Verifikasi kondisi area kerja

### 5. **Active Permit Monitoring**
- Dashboard permit aktif real-time
- QR Code untuk verifikasi di lapangan
- Stop Work Authority
- Suspend/Resume permit

### 6. **Reporting & Audit Trail**
- Histori lengkap setiap permit
- Export ke PDF dan Excel
- Log aktivitas sistem
- Retensi data minimal 5 tahun

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────┐
│  User Browser   │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  Web PTW System │
│   (ASP.NET MVC) │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│ Approval Engine │
│  (Workflow)     │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│    Database     │
│   (MySQL MAMP)  │
└─────────────────┘
```

### Modul Sistem

1. **User & Role Management** - RBAC dan akses kontrol
2. **Permit Management** - Core permit functionality
3. **JSEA Management** - Risk analysis
4. **Checklist K3** - Safety verification
5. **Approval Workflow** - Multi-level authorization
6. **Monitoring** - Active permit tracking
7. **Closing Permit** - Work completion
8. **Reporting & Audit** - Analytics dan compliance

---

## 🔨 Jenis Pekerjaan

Sistem mendukung berbagai jenis pekerjaan berisiko dengan checklist K3 spesifik:

| Jenis | Deskripsi | Risk Level |
|-------|-----------|------------|
| **Hot Work** | Pengelasan, cutting, grinding | 🔴 High |
| **Electrical Work** | Pekerjaan instalasi listrik | 🔴 High |
| **Working at Height** | Pekerjaan di ketinggian >1.8m | 🔴 High |
| **Confined Space** | Kerja di ruang terbatas | 🔴 High |
| **Maintenance Mesin** | Service mesin produksi | 🟡 Medium |
| **Chemical Handling** | Penanganan bahan kimia | 🔴 High |

---

## 🔄 Alur Kerja

### High-Level Flow

```
START
  ↓
Permit Request
  ↓
JSEA Completed? ──NO──→ Return to Requester
  ↓ YES
Approval Process
  ├─ Supervisor
  ├─ Area Owner
  ├─ HSE
  └─ Authorizer
  ↓
Permit Active
  ↓
Work Execution & Monitoring
  ↓
Permit Close
  ↓
END
```

### Detail Fase

#### **Fase 1: Initiation**
- Requester login dan membuat permit baru
- Mengisi detail pekerjaan (lokasi, waktu, deskripsi)
- Menyimpan sebagai Draft

#### **Fase 2: Risk Analysis**
- Membuat/melengkapi JSEA
- Identifikasi bahaya per langkah kerja
- Menentukan pengendalian risiko
- Submit permit (hanya jika JSEA lengkap)

#### **Fase 3: Authorization**
1. **Supervisor Review** - Validasi metode kerja dan kesiapan tim
2. **Area Owner Review** - Konfirmasi keamanan area produksi
3. **HSE Review** - Validasi aspek K3 dan JSEA
4. **Final Authorization** - Keputusan akhir oleh Authorizer

#### **Fase 4: Execution**
- Permit berstatus ACTIVE
- QR Code ditempel di lokasi kerja
- Monitoring oleh Supervisor
- Stop Work Authority dapat digunakan kapan saja

#### **Fase 5: Closure**
- Konfirmasi pekerjaan selesai
- Verifikasi area aman
- Release LOTO dan peralatan
- Permit status menjadi CLOSED

---

## 👥 Role & Tanggung Jawab

| Role | Tanggung Jawab | Akses |
|------|----------------|-------|
| **Requester** | Mengajukan permit kerja | Create permit, view own permits |
| **Supervisor** | Mengawasi dan approve pekerjaan | Approve permits, monitor execution |
| **Area Owner** | Menjamin keamanan area produksi | Approve permits, coordinate with production |
| **HSE** | Validasi aspek K3 dan JSEA | Approve permits, audit compliance, stop work |
| **Authorizer** | Otoritas akhir pemberian izin | Final approval, full visibility |
| **Executor** | Melaksanakan pekerjaan | View assigned permits (read-only) |
| **Admin** | Mengelola sistem dan master data | Full system access |

---

## 📊 Status Permit

Status permit mengikuti alur yang ketat dan berurutan:

```
DRAFT
  ↓
SUBMITTED
  ↓
SUPERVISOR_APPROVED
  ↓
AREA_OWNER_APPROVED
  ↓
HSE_APPROVED
  ↓
ACTIVE
  ↓
CLOSED
```

**Status Exception:**
- `REJECTED` - Ditolak oleh approver
- `SUSPENDED` - Dihentikan sementara karena kondisi tidak aman
- `CANCELLED` - Dibatalkan karena keadaan darurat

---

## 🗄️ Struktur Database

### Entitas Utama

#### **Users & Authentication**
```sql
users (id, employee_id, name, email, password_hash, is_active)
roles (id, name)
user_roles (user_id, role_id)
```

#### **Permit Core**
```sql
permits (id, permit_no, permit_type_id, title, description, 
         location_id, start_time, end_time, requester_id, status)
permit_types (id, code, name, risk_level)
locations (id, parent_id, name, type)
```

#### **JSEA**
```sql
jsea (id, permit_id, prepared_by, reviewed_by)
jsea_steps (id, jsea_id, step_no, job_step, hazard, 
            risk_level, control)
```

#### **Safety Checklist**
```sql
safety_checklists (id, permit_type_id, name)
checklist_items (id, checklist_id, description)
permit_checklist_results (permit_id, checklist_item_id, 
                          is_checked, remarks)
```

#### **Approval & Monitoring**
```sql
permit_approvals (id, permit_id, role, approver_id, 
                  status, remarks, approved_at)
permit_monitoring (id, permit_id, action, reason, 
                   action_by, action_time)
permit_closing (id, permit_id, closed_by, closing_notes, closed_at)
```

#### **Audit**
```sql
audit_logs (id, user_id, action, entity, entity_id, timestamp)
```

### Relasi ERD

```
users
  ↓
permits → permit_approvals
   ↓           
  jsea → jsea_steps
   ↓
permit_checklist_results
   ↓
checklist_items → safety_checklists
```

---

## 💻 Teknologi

### Backend
- **Framework:** ASP.NET Core MVC
- **Language:** C#
- **Database:** MySQL via MAMP
- **ORM:** Entity Framework Core
- **Authentication:** ASP.NET Identity

### Frontend
- **UI Framework:** Tailwind CSS
- **JavaScript:** Vanilla JS / jQuery
- **Template Engine:** Razor Views
- **Icons:** Lucide Icons

### DevOps & Tools
- **Version Control:** Git
- **CI/CD:** (To be defined)
- **Hosting:** (To be defined)

---

### Default Login

- **Admin:** admin@company.com / Admin123!
- **HSE:** hse@company.com / Hse123!

*(Segera ganti password setelah login pertama)*

---

## 🔒 Prinsip Keamanan

### Business Rules (Non-Negotiable)

```
⛔ No Permit – No Work
⛔ No JSEA – No Permit
⛔ No Approval – No Active Permit
⛔ Unsafe Condition – Stop Work
```

### Kontrol Akses

- ✅ Role-Based Access Control (RBAC)
- ✅ Requester tidak boleh approve permit sendiri
- ✅ Approval harus berurutan
- ✅ Satu user tidak boleh multi-role pada permit yang sama
- ✅ Executor hanya memiliki akses read-only

### Audit & Compliance

- ✅ Semua aktivitas tercatat dalam audit log
- ✅ Retensi data minimal 5 tahun
- ✅ Trail lengkap untuk setiap permit
- ✅ Mendukung audit ISO 45001

---

## 📱 User Interface

### Color Coding Status

| Status | Color |
|--------|-------|
| Draft | ![#E5E7EB](https://via.placeholder.com/15/E5E7EB/000000?text=+) Gray |
| Submitted | ![#FBBF24](https://via.placeholder.com/15/FBBF24/000000?text=+) Yellow |
| Supervisor Approved | ![#60A5FA](https://via.placeholder.com/15/60A5FA/000000?text=+) Blue |
| Area Owner Approved | ![#818CF8](https://via.placeholder.com/15/818CF8/000000?text=+) Indigo |
| HSE Approved | ![#A78BFA](https://via.placeholder.com/15/A78BFA/000000?text=+) Purple |
| Active | ![#10B981](https://via.placeholder.com/15/10B981/000000?text=+) Green |
| Suspended | ![#EF4444](https://via.placeholder.com/15/EF4444/000000?text=+) Red |
| Closed | ![#4B5563](https://via.placeholder.com/15/4B5563/000000?text=+) Dark Gray |

### Layout

```
+--------------------------------------------------+
| Header (Logo, User, Notifications, Logout)       |
+----------------------+---------------------------+
| Sidebar Menu         | Main Content               |
| - Dashboard          |                           |
| - Permit             |  [Dynamic Content Area]   |
| - Approval           |                           |
| - Monitoring         |                           |
| - Report             |                           |
| - Master Data        |                           |
+----------------------+---------------------------+
| Footer                                           |
+--------------------------------------------------+
```

---

# Plan Frontend PTW System - UI/UX & Navigasi

Saya akan membuatkan rencana lengkap untuk frontend PTW System yang user-friendly dan sesuai dengan kebutuhan industri manufaktur.

---

## 🎨 Design System

### Color Palette

```
Primary Colors:
- Primary: #2563EB (Blue 600) - Untuk CTA utama
- Secondary: #059669 (Emerald 600) - Untuk status Active/Success
- Danger: #DC2626 (Red 600) - Untuk warning/reject
- Warning: #F59E0B (Amber 500) - Untuk pending status

Neutral:
- Gray 50-900 untuk background, text, borders

Status Colors (sesuai dokumen):
- Draft: #E5E7EB (Gray 200)
- Submitted: #FBBF24 (Amber 400)
- In Approval: #60A5FA (Blue 400)
- Active: #10B981 (Emerald 500)
- Suspended: #EF4444 (Red 500)
- Closed: #4B5563 (Gray 600)
```

### Typography

```
Font Family: Inter / System UI
Heading 1: 2rem (32px) - Semibold
Heading 2: 1.5rem (24px) - Semibold
Heading 3: 1.25rem (20px) - Medium
Body: 0.875rem (14px) - Regular
Small: 0.75rem (12px) - Regular
```

---

## 📐 Layout Structure

### Main Layout Components

```
┌─────────────────────────────────────────────────────┐
│ Top Navigation Bar (Fixed)                          │
│ Logo | Search | Notifications | User Profile        │
├──────────┬──────────────────────────────────────────┤
│          │                                           │
│ Sidebar  │  Main Content Area                        │
│ (Fixed)  │  ┌─────────────────────────────────────┐ │
│          │  │ Breadcrumb                          │ │
│ Nav Menu │  ├─────────────────────────────────────┤ │
│          │  │ Page Header + Actions               │ │
│          │  ├─────────────────────────────────────┤ │
│          │  │                                     │ │
│          │  │ Dynamic Content                     │ │
│          │  │                                     │ │
│          │  │                                     │ │
│          │  └─────────────────────────────────────┘ │
├──────────┴──────────────────────────────────────────┤
│ Footer (Status bar, version info)                   │
└─────────────────────────────────────────────────────┘
```

---

## 🧭 Navigation Structure

### Sidebar Menu (Role-Based)

```
┌─────────────────────────┐
│ [Logo] PTW System       │
├─────────────────────────┤
│                         │
│ 🏠 Dashboard            │
│                         │
│ 📋 Permit Saya          │
│   ├─ List Permit        │
│   ├─ Buat Permit Baru   │
│   └─ Draft              │
│                         │
│ ✅ Approval (badge: 5)  │
│   ├─ Menunggu Review    │
│   └─ Riwayat Approval   │
│                         │
│ 👁️ Monitoring           │
│   ├─ Permit Aktif       │
│   ├─ QR Scanner         │
│   └─ Stop Work Log      │
│                         │
│ 📊 Laporan              │
│   ├─ Statistik          │
│   ├─ Export Data        │
│   └─ Audit Trail        │
│                         │
│ ⚙️ Master Data (Admin)  │
│   ├─ User Management    │
│   ├─ Permit Types       │
│   ├─ Locations          │
│   └─ Safety Checklist   │
│                         │
└─────────────────────────┘
```

### Top Navigation Bar

```
┌────────────────────────────────────────────────────┐
│ [☰] [Logo] PTW    [🔍 Search]    [🔔3] [👤 User ▼] │
└────────────────────────────────────────────────────┘

Components:
- Toggle Sidebar (Mobile)
- Quick Search (Global search permits)
- Notification Bell (dengan badge counter)
- User Dropdown:
  ├─ Profile
  ├─ Settings
  ├─ Help
  └─ Logout
```

---

## 📱 Responsive Design Strategy

### Breakpoints

```css
/* Mobile First Approach */
sm: 640px   /* Mobile landscape */
md: 768px   /* Tablet */
lg: 1024px  /* Desktop */
xl: 1280px  /* Large desktop */
```

### Layout Behavior

**Desktop (≥1024px)**
- Sidebar visible & fixed
- 3-column grid untuk cards
- Full data table view

**Tablet (768px - 1023px)**
- Collapsible sidebar
- 2-column grid
- Condensed table view

**Mobile (<768px)**
- Hidden sidebar (hamburger menu)
- Single column
- Card-based view (bukan table)
- Bottom navigation untuk quick actions

---

## 🖥️ Key Pages & Wireframes

### 1. Dashboard

```
┌─────────────────────────────────────────────────┐
│ Dashboard                            [Date ▼]   │
├─────────────────────────────────────────────────┤
│                                                 │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌────────┐│
│ │📝 Draft │ │⏳Pending│ │✅Active │ │📊Total ││
│ │   12    │ │   8     │ │   5    │ │  156   ││
│ └─────────┘ └─────────┘ └─────────┘ └────────┘│
│                                                 │
│ ┌──────────────────────────────────────────────┐│
│ │ 📊 Permit Trend (Chart)                      ││
│ │ [Line/Bar Chart - 6 bulan terakhir]          ││
│ └──────────────────────────────────────────────┘│
│                                                 │
│ ┌─────────────────┐  ┌──────────────────────┐  │
│ │ 🔥 Hot Work: 3  │  │ ⚡ Electrical: 2     │  │
│ │ 📏 Height: 2    │  │ 🚧 Confined: 1       │  │
│ └─────────────────┘  └──────────────────────┘  │
│                                                 │
│ Recent Permits                    [View All >]  │
│ ┌──────────────────────────────────────────────┐│
│ │ PTW-2024-001 | Hot Work | Active | Area A   ││
│ │ PTW-2024-002 | Height   | Pending| Area B   ││
│ └──────────────────────────────────────────────┘│
└─────────────────────────────────────────────────┘
```

### 2. List Permit

```
┌─────────────────────────────────────────────────┐
│ Permit Saya                    [+ Buat Baru]    │
├─────────────────────────────────────────────────┤
│                                                 │
│ Filter: [All ▼] [Type ▼] [Status ▼] [🔍Search] │
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ No.Permit    │ Type    │ Status  │ Actions │ │
│ ├─────────────────────────────────────────────┤ │
│ │ PTW-2024-001 │Hot Work │🟢Active │[👁️][📝]│ │
│ │ PTW-2024-002 │Height   │🟡Pending│[👁️][📝]│ │
│ │ PTW-2024-003 │Electric │⚪Draft  │[📝][🗑️]│ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ Showing 1-10 of 156     [< 1 2 3 4 5 >]        │
└─────────────────────────────────────────────────┘
```

### 3. Buat/Edit Permit (Multi-Step Form)

```
┌─────────────────────────────────────────────────┐
│ Buat Permit Baru                    [Save Draft]│
├─────────────────────────────────────────────────┤
│                                                 │
│ Progress: ●━━━○━━━○━━━○                         │
│           1   2   3   4                         │
│         Info JSEA Check Submit                  │
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ STEP 1: Informasi Dasar                     │ │
│ │                                             │ │
│ │ Jenis Pekerjaan: [Hot Work ▼]              │ │
│ │ Judul Pekerjaan: [_____________________]   │ │
│ │ Deskripsi:                                  │ │
│ │ [________________________________]          │ │
│ │                                             │ │
│ │ Lokasi: [Area A - Produksi Line 1 ▼]       │ │
│ │ Tanggal Mulai: [📅 05/02/2026]             │ │
│ │ Waktu: [08:00] - [17:00]                   │ │
│ │                                             │ │
│ │ Executor: [+ Add Executor]                 │ │
│ │ - John Doe (Welder)              [🗑️]      │ │
│ │ - Jane Smith (Supervisor)        [🗑️]      │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│                            [Cancel] [Next >]    │
└─────────────────────────────────────────────────┘
```

### 4. JSEA Form

```
┌─────────────────────────────────────────────────┐
│ STEP 2: Job Safety & Environmental Analysis    │
├─────────────────────────────────────────────────┤
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ Step Analysis              [+ Add Step]     │ │
│ ├─────────────────────────────────────────────┤ │
│ │ Step 1: Persiapan Area Kerja    [Edit][🗑️] │ │
│ │                                             │ │
│ │ Langkah Kerja:                              │ │
│ │ Membersihkan area dan setup peralatan       │ │
│ │                                             │ │
│ │ Bahaya:                                     │ │
│ │ - Tersandung material                       │ │
│ │ - Peralatan jatuh                           │ │
│ │                                             │ │
│ │ Risk Level: [🔴 HIGH ▼]                     │ │
│ │                                             │ │
│ │ Pengendalian:                               │ │
│ │ - Gunakan APD lengkap                       │ │
│ │ - Barricade area kerja                      │ │
│ │ - Pre-job safety briefing                   │ │
│ ├─────────────────────────────────────────────┤ │
│ │ Step 2: [Collapsed]              [Expand]   │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│                        [< Back] [Next >]        │
└─────────────────────────────────────────────────┘
```

### 5. Safety Checklist

```
┌─────────────────────────────────────────────────┐
│ STEP 3: Safety Checklist - Hot Work            │
├─────────────────────────────────────────────────┤
│                                                 │
│ ⚠️ Semua item harus dicentang untuk melanjutkan│
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ A. Persiapan Area                           │ │
│ │ ☑️ Area kerja bebas dari bahan mudah terbakar│ │
│ │ ☑️ Fire extinguisher tersedia dalam jangkauan│ │
│ │ ☑️ Fire watch ditugaskan                     │ │
│ │ ☐ Hot work shield terpasang                 │ │
│ │                                             │ │
│ │ B. Alat Pelindung Diri                      │ │
│ │ ☑️ Welding helmet & safety glasses          │ │
│ │ ☑️ Leather gloves & apron                   │ │
│ │ ☐ Safety shoes                              │ │
│ │                                             │ │
│ │ C. Peralatan                                │ │
│ │ ☑️ Welding machine dalam kondisi baik       │ │
│ │ ☐ Gas cylinder secured properly             │ │
│ │                                             │ │
│ │ Catatan Tambahan:                           │ │
│ │ [______________________________]            │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ Completion: 8/11 items (73%)                    │
│                        [< Back] [Next >]        │
└─────────────────────────────────────────────────┘
```

### 6. Review & Submit

```
┌─────────────────────────────────────────────────┐
│ STEP 4: Review & Submit                         │
├─────────────────────────────────────────────────┤
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ Ringkasan Permit                            │ │
│ │                                             │ │
│ │ No. Permit: PTW-2024-XXX (Auto-generated)   │ │
│ │ Jenis: Hot Work - Welding                   │ │
│ │ Lokasi: Area A - Produksi Line 1            │ │
│ │ Periode: 05 Feb 2026, 08:00 - 17:00        │ │
│ │ Requester: Anda (John Worker)               │ │
│ │                                             │ │
│ │ JSEA: ✅ 3 steps completed                  │ │
│ │ Safety Checklist: ⚠️ 3 items pending        │ │
│ │                                             │ │
│ │ ⚠️ Perhatian:                               │ │
│ │ Setelah submit, permit akan masuk proses    │ │
│ │ approval dan tidak dapat diubah tanpa       │ │
│ │ persetujuan approver.                       │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│         [< Back] [Save Draft] [Submit Permit]   │
└─────────────────────────────────────────────────┘
```

### 7. Approval Dashboard

```
┌─────────────────────────────────────────────────┐
│ Approval Queue               [Refresh] 🔔 5     │
├─────────────────────────────────────────────────┤
│                                                 │
│ Tabs: [Pending (5)] [Reviewed (24)]            │
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ PTW-2024-001 - Hot Work Welding            🔥│ │
│ │ Requester: John Worker | Area A              │ │
│ │ Submitted: 05 Feb 2026, 08:30               │ │
│ │ ⏱️ SLA: 2h 15m remaining                     │ │
│ │                                             │ │
│ │ Quick Info:                                 │ │
│ │ • JSEA: ✅ Completed (3 steps)              │ │
│ │ • Checklist: ✅ All items checked           │ │
│ │ • Risk Level: 🔴 HIGH                       │ │
│ │                                             │ │
│ │          [View Details] [Approve] [Reject]  │ │
│ ├─────────────────────────────────────────────┤ │
│ │ PTW-2024-002 - Working at Height           📏│ │
│ │ ...                                         │ │
│ └─────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────┘
```

### 8. Permit Detail View

```
┌─────────────────────────────────────────────────┐
│ ← Back to List        PTW-2024-001    [⋮ Menu] │
├─────────────────────────────────────────────────┤
│                                                 │
│ 🔥 Hot Work - Welding Main Frame                │
│ Status: 🟢 ACTIVE                               │
│                                                 │
│ ┌────────────────────────────────────┐          │
│ │ Timeline                           │          │
│ │ ✅ Created - 05 Feb, 08:00        │          │
│ │ ✅ Supervisor Approved - 08:30    │          │
│ │ ✅ Area Owner Approved - 09:00    │          │
│ │ ✅ HSE Approved - 09:30           │          │
│ │ ✅ Authorized - 10:00             │          │
│ │ 🟢 Active Since - 10:15           │          │
│ └────────────────────────────────────┘          │
│                                                 │
│ ┌── Tabs ──────────────────────────────────┐    │
│ │[Info][JSEA][Checklist][Approval][Monitoring]│ │
│ │                                           │   │
│ │ Detail Informasi:                         │   │
│ │ Lokasi: Area A - Produksi Line 1          │   │
│ │ Periode: 05 Feb 08:00 - 17:00            │   │
│ │ Requester: John Worker                    │   │
│ │ Supervisor: Jane Smith                    │   │
│ │                                           │   │
│ │ Deskripsi:                                │   │
│ │ Pengelasan frame utama untuk mesin baru   │   │
│ │                                           │   │
│ │ Executor Team:                            │   │
│ │ • John Doe (Welder)                       │   │
│ │ • Mike Brown (Fire Watch)                 │   │
│ └───────────────────────────────────────────┘   │
│                                                 │
│ [📥 Download PDF] [🖨️ Print] [⚠️ Stop Work]    │
└─────────────────────────────────────────────────┘
```

### 9. Active Monitoring

```
┌─────────────────────────────────────────────────┐
│ Permit Aktif - Real-time Monitoring            │
├─────────────────────────────────────────────────┤
│                                                 │
│ [🔴 LIVE] Auto-refresh: ON  Last: 2 sec ago    │
│                                                 │
│ ┌─────────────────────────────────────────────┐ │
│ │ 🗺️ Map View / List View                     │ │
│ ├─────────────────────────────────────────────┤ │
│ │ Area A (3 Active)                           │ │
│ │ ┌─────────────────────────────────────┐     │ │
│ │ │ 🔥 PTW-001 | Hot Work | 6h 30m left │     │ │
│ │ │ 📍 Line 1 | ⏰ Started: 10:15       │     │ │
│ │ │ Status: 🟢 Normal                   │     │ │
│ │ │        [View] [QR Code] [Suspend]   │     │ │
│ │ └─────────────────────────────────────┘     │ │
│ │ ┌─────────────────────────────────────┐     │ │
│ │ │ ⚡ PTW-002 | Electrical              │     │ │
│ │ │ Status: ⏸️ SUSPENDED (15m ago)      │     │ │
│ │ │ Reason: Unsafe condition detected   │     │ │
│ │ │        [View] [Resume] [Details]    │     │ │
│ │ └─────────────────────────────────────┘     │ │
│ └─────────────────────────────────────────────┘ │
│                                                 │
│ Quick Actions:                                  │
│ [📱 Scan QR] [🛑 Emergency Stop All] [📊 Export]│
└─────────────────────────────────────────────────┘
```

### 10. QR Code Display

```
┌─────────────────────────────────────────────────┐
│              PTW-2024-001                       │
│              Hot Work Permit                    │
├─────────────────────────────────────────────────┤
│                                                 │
│              ┌─────────────┐                    │
│              │             │                    │
│              │  QR CODE    │                    │
│              │   IMAGE     │                    │
│              │             │                    │
│              └─────────────┘                    │
│                                                 │
│         Scan untuk verifikasi permit            │
│                                                 │
│ Location: Area A - Line 1                       │
│ Valid: 05 Feb 2026, 08:00 - 17:00              │
│ Status: 🟢 ACTIVE                               │
│                                                 │
│                                                 │
│         [📥 Download] [🖨️ Print A4]            │
│         [📧 Email QR]                           │
└─────────────────────────────────────────────────┘
```

---

## 🎯 UX Principles

### 1. **Progressive Disclosure**
- Multi-step form untuk reduce cognitive load
- Collapsible sections untuk info detail
- Tabs untuk organize complex data

### 2. **Clear Feedback**
```
✅ Success: Toast notification (green, top-right, 3s)
❌ Error: Alert banner (red, top, persistent)
⏳ Loading: Skeleton loader / spinner
💾 Auto-save: "Draft saved at 10:30" (subtle)
```

### 3. **Accessibility**
- Keyboard navigation support
- ARIA labels untuk screen readers
- Color contrast ratio ≥ 4.5:1
- Focus indicators jelas
- Form validation dengan error messages

### 4. **Mobile Optimization**
```
Touch Targets:
- Minimum 44×44px
- Spacing antar button: 8px

Mobile Interactions:
- Swipe untuk navigate tabs
- Pull-to-refresh untuk data update
- Bottom sheet untuk actions
- Sticky header untuk context
```

### 5. **Performance**
- Lazy loading untuk tables
- Pagination (max 20 items/page)
- Infinite scroll untuk mobile
- Image optimization
- Minimal JavaScript bundle

---

## 🔔 Notification System

### Types

```
┌─────────────────────────────────────┐
│ 🔔 Notifications (3)                │
├─────────────────────────────────────┤
│ 🟢 PTW-001 Approved                 │
│    Your permit has been approved    │
│    2 minutes ago                    │
├─────────────────────────────────────┤
│ 🔴 PTW-002 Rejected                 │
│    Reason: Incomplete JSEA          │
│    15 minutes ago                   │
├─────────────────────────────────────┤
│ ⏰ PTW-003 Expiring Soon            │
│    Expires in 1 hour                │
│    1 hour ago                       │
├─────────────────────────────────────┤
│           [Mark All Read]           │
└─────────────────────────────────────┘
```

### Channels
- 🔔 In-app notification bell
- 📧 Email notification
- 📱 SMS (untuk critical alerts)
- 🖥️ Browser push notification

---

## 🎨 Component Library

### Reusable Components

1. **Status Badge**
```html
<span class="badge badge-active">Active</span>
<span class="badge badge-pending">Pending</span>
<span class="badge badge-draft">Draft</span>
```

2. **Action Buttons**
```html
<!-- Primary -->
<button class="btn btn-primary">Submit</button>

<!-- Secondary -->
<button class="btn btn-secondary">Cancel</button>

<!-- Danger -->
<button class="btn btn-danger">Reject</button>

<!-- Icon Button -->
<button class="btn btn-icon">
  <svg>...</svg>
</button>
```

3. **Cards**
```html
<div class="card">
  <div class="card-header">Title</div>
  <div class="card-body">Content</div>
  <div class="card-footer">Actions</div>
</div>
```

4. **Data Table**
```html
<table class="table table-striped">
  <thead>...</thead>
  <tbody>...</tbody>
</table>
```

5. **Form Controls**
```html
<div class="form-group">
  <label>Label</label>
  <input type="text" class="form-control">
  <span class="form-hint">Helper text</span>
  <span class="form-error">Error message</span>
</div>
```

---

## 📊 Interaction Patterns

### 1. **Approval Flow**
```
User clicks "Approve"
  ↓
Modal confirmation appears
  ↓
User adds optional remarks
  ↓
Clicks "Confirm Approval"
  ↓
Loading spinner
  ↓
Success toast + status update
  ↓
Auto-redirect atau stay with refresh
```

### 2. **Form Validation**
```
Real-time Validation:
- On blur (setelah user keluar dari field)
- Show error icon + message
- Prevent submit jika ada error

Server Validation:
- Submit form
- Loading state
- Show server errors di top form
- Scroll ke first error
```

### 3. **Search & Filter**
```
User types in search (debounce 300ms)
  ↓
Loading skeleton appears
  ↓
Results render
  ↓
"Showing X of Y results"
  ↓
[Clear filters] option available
```

---
