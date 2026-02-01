# PTW - Sistem Permit to Work

> **Sistem Permit to Work (PTW) Berbasis Web untuk Industri Manufaktur**

Sistem kontrol izin kerja berisiko yang aman, terkendali, dan terdokumentasi dengan baik untuk lingkungan industri manufaktur.

---

## 📋 Daftar Isi

- [Tentang Proyek](#-tentang-proyek)
- [Latar Belakang](#-latar-belakang)
- [Fitur Utama](#-fitur-utama)
- [Arsitektur Sistem](#-arsitektur-sistem)
- [Jenis Pekerjaan](#-jenis-pekerjaan)
- [Alur Kerja](#-alur-kerja)
- [Role & Tanggung Jawab](#-role--tanggung-jawab)
- [Status Permit](#-status-permit)
- [Struktur Database](#-struktur-database)
- [Teknologi](#-teknologi)
- [Instalasi](#-instalasi)
- [Prinsip Keamanan](#-prinsip-keamanan)

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

## 🚀 Instalasi

### Prerequisites

- .NET 6.0 atau lebih tinggi
- MySQL via MAMP
- Visual Studio 2022 / VS Code
- Node.js (untuk Tailwind CSS)

### Langkah Instalasi

1. **Clone Repository**
```bash
git clone https://github.com/your-org/ptw-system.git
cd ptw-system
```

2. **Setup Database**
```bash
# Update connection string di appsettings.json
# Jalankan migrations
dotnet ef database update
```

3. **Install Dependencies**
```bash
dotnet restore
npm install
```

4. **Build Tailwind CSS**
```bash
npm run build:css
```

5. **Run Application**
```bash
dotnet run
```

6. **Access Application**
```
https://localhost:5001
```

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

## 📖 Dokumentasi Tambahan

- [User Manual](docs/user-manual.md)
- [API Documentation](docs/api-docs.md)
- [Database Schema](docs/database-schema.md)
- [Development Guide](docs/development-guide.md)

---

## 🤝 Kontribusi

Silakan baca [CONTRIBUTING.md](CONTRIBUTING.md) untuk detail proses pengembangan dan pull request.

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).

---

## 📞 Kontak & Support

- **Name:** fachriramdhan
- **Email:** support@company.com
- **Issue Tracker:** [GitHub Issues](https://github.com/your-org/ptw-system/issues)

---

## 🎯 Roadmap

### Version 1.0 (Current)
- ✅ Core permit management
- ✅ JSEA integration
- ✅ Approval workflow
- ✅ Basic reporting

### Version 2.0 (Planned)
- 📍 Map-based permit monitoring
- 📱 Mobile app for field workers
- 🔔 Real-time notifications (SignalR)
- 📊 Advanced analytics dashboard
- 🔗 Integration with CMMS

### Version 3.0 (Future)
- 🤖 AI-powered risk prediction
- 📸 Photo documentation
- 🎥 Video briefing integration
- 🌐 Multi-language support

---

## ⚠️ Disclaimer

Sistem ini dirancang untuk membantu pengelolaan permit to work. Namun, keputusan keselamatan kerja tetap menjadi tanggung jawab penuh manajemen dan personel yang berwenang. Sistem tidak menggantikan prosedur keselamatan perusahaan yang ada.

---

**© 2026 fachriramdhan. All Rights Reserved.**
