# Cloud-Powered College Medical Unit & Dispensary System

A professional, persistent campus health center application with Firebase Firestore cloud storage, branch-wise and year-wise student directories, dedicated Visitors/Others intake, medicine restock facilities, keyboard-editable quantities, and in-app CSV roster importing.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> Based on your answers and requirements:
> 1. **Cloud Database (Firebase Firestore)**: Records will be stored in a permanent cloud database so student data, visits, and medicine stock are never deleted or lost even after closing the app. (Upon approving this plan, the Firebase setup prompt will open for your 1-click confirmation).
> 2. **Branch-wise & Year-wise Student Table**: A structured directory with interactive filter tabs for Branch (Computer Science, Mechanical, Civil, Electrical, etc.) and Year (1st, 2nd, 3rd, 4th Year) alongside instant Enrollment No search (`2024BCSE001`).
> 3. **Keyboard-Editable Quantity Input**: Quantity fields will allow direct keyboard typing and arrow key increments with real-time stock validation.
> 4. **New Section: Visitors / Others**: A dedicated intake tab recording Name, Gender, Age, Visiting Date/Time, and medicine dispensed for campus visitors, parents, contractors, and guests.
> 5. **Medicine Stock Management**: Full facility to add brand new medicines and restock existing medicines (+10, +50, +100 or custom quantity).
> 6. **In-App CSV Importer**: Dedicated uploader and paste box with template download to load your college CSV file directly into the cloud database.

---

## 1. Overview & Core Concept

- **What It Does**: Provides a complete, cloud-synced dispensary management dashboard for the college medical unit. It tracks visiting dates and times, dispenses medicine with live stock decrement, organizes students branch-wise and year-wise, manages staff and campus visitors, and maintains full consultation histories.
- **Target Users**: College medical officers, dispensary attendants, infirmary nurses, and administrative health coordinators.
- **Key Value**: Permanent cloud persistence with zero data loss, instant enrollment lookup (`2024BCSE001`), quick intake with keyboard entry, and bulk CSV onboarding.

---

## 2. User Experience & Visual Design

### Key User Flows

1. **Student Search & Intake (Branch & Year Filter)**:
   - Search by Enrollment No (e.g. `2024BCSE001`) or filter by Branch tab and Year tab.
   - Comprehensive student table shows Enrollment No, Name, Branch, Year, and Total Visits.
   - Clicking a student loads their card with the direct medicine dispense form (medicine dropdown + keyboard-editable quantity).
   - Pressing **"Save & Deduct Stock"** records the current date/time to the cloud database and shows all past consultations in a structured table.
2. **Staff & Guard Intake**:
   - Filter by Branch / Department and search by Name or Employee ID.
   - Quick medicine dispense and full past visit history.
3. **Visitors / Others Intake (New Section)**:
   - Fast registration/intake form: Visitor Name, Gender, Age, Purpose/Phone.
   - Medicine selection with keyboard-editable quantity.
   - Auto-captured Date & Time saved to cloud DB with visitor consultation history table.
4. **Medicine Stock & Restock**:
   - Live inventory table: Medicine Name, Available Stock, Unit, Low Stock status.
   - "+ Add New Medicine" modal (Name, Unit, Initial Stock, Minimum Threshold).
   - Quick Restock buttons (+10, +50, +100, custom) for existing medicines.
5. **CSV Roster Import**:
   - Upload any `.csv` file or paste raw CSV text.
   - Automatic column recognition (`Enrollment`, `Name`, `Branch`, `Year`, `Type`, `Phone`).
   - One-click import into Firebase Firestore cloud database with downloadable sample CSV.

### Visual Identity & Theme
- **Aesthetic Direction**: Professional clinical dashboard, crisp contrast, utilitarian speed.
- **Palette**: Pure white and slate-50 backgrounds, clinical teal accents (`#0f766e`), navy slate headers (`#0f172a`), emerald green for healthy stock, amber for low stock, crimson for out-of-stock.
- **Typography**: `Plus Jakarta Sans` for clean headings and `JetBrains Mono` for tabular enrollment numbers and quantities.

---

## 3. Key Product Decisions & Trade-Offs

- **Decision 1: Firebase Firestore Cloud Database**
  - *Chosen Approach*: Provision Firestore with client offline persistence cache.
  - *Why*: Ensures records are never deleted when the browser closes, works seamlessly across multiple clinic computers, and synchronizes real-time stock deductions.
- **Decision 2: Direct Keyboard-Enabled Numeric Inputs**
  - *Chosen Approach*: Uncontrolled/controlled numeric `<input type="number">` with standard keydown and change handlers allowing instant keyboard typing.
  - *Why*: Pharmacists and doctors can type "2" or "10" on physical keypads without clicking buttons.
- **Decision 3: Branch & Year Matrix Navigation**
  - *Chosen Approach*: Segmented branch buttons (e.g., Computer Science, Mechanical, Electronics, Civil) paired with a Year pill selector (All, 1st Year, 2nd Year, 3rd Year, 4th Year).
  - *Why*: Allows browsing entire classes or finding students when only their branch and year are known.

---

## 4. Technical Architecture & Data Strategy

### System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       CampusMed Cloud Dashboard                             │
├─────────────────────┬─────────────────────┬───────────────────┬─────────────┤
│  Student Directory  │    Staff & Guards   │  Visitors/Others  │  Medicine   │
│  - Enrollment No    │  - Branch & Name    │  - Name, Gender,  │    Stock    │
│  - Branch & Year    │  - Role (Guards/    │    Age, Contact   │  - Restock  │
│    Filter Tabs      │    Faculty/Tech)    │  - Quick Dispense │  - New Med  │
├─────────────────────┴─────────────────────┴───────────────────┴─────────────┤
│                         Active Intake & History Card                        │
│  - Direct Medicine Select Dropdown                                          │
│  - Keyboard-Editable Quantity Input (<input type="number">)                 │
│  - "Save & Deduct Stock" (Auto-Captured Timestamp: ISO + Readable Format)   │
│  - Comprehensive Past Consultations Table                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│               CSV Roster Importer (File Upload & Text Paste)                │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                 Firebase Firestore Cloud Database                           │
│  ├── /students/{studentId}   (enrollmentNo, name, branch, year, phone)      │
│  ├── /staff/{staffId}         (employeeId, name, branch, staffRole, phone)  │
│  ├── /visitors/{visitorId}   (name, gender, age, contact, createdAt)        │
│  ├── /medicines/{medicineId} (name, stockQuantity, unit, minThreshold)     │
│  └── /visits/{visitId}       (patientId, identifier, dateTime, medicine, qty)│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Data Schema Entities

1. **Student**:
   - `id`: string
   - `enrollmentNo`: string (e.g. `2024BCSE001`)
   - `name`: string
   - `branch`: string
   - `year`: string (`1st Year`, `2nd Year`, `3rd Year`, `4th Year`)
   - `phone`: string
2. **Staff**:
   - `id`: string
   - `employeeId`: string (e.g. `SEC-GD-018`)
   - `name`: string
   - `branch`: string
   - `staffRole`: `'guard' | 'faculty' | 'technical' | 'maintenance'`
   - `phone`: string
3. **Visitor**:
   - `id`: string
   - `name`: string
   - `gender`: `'Male' | 'Female' | 'Other'`
   - `age`: number
   - `phone`: string
   - `purpose`: string
4. **Medicine**:
   - `id`: string
   - `name`: string
   - `stockQuantity`: number
   - `unit`: string
   - `minStockThreshold`: number
5. **VisitRecord**:
   - `id`: string
   - `patientId`: string
   - `patientType`: `'student' | 'staff' | 'visitor'`
   - `patientName`: string
   - `identifier`: string (`2024BCSE001`, `SEC-GD-018`, or visitor badge)
   - `branch`: string
   - `dateTime`: string (ISO)
   - `formattedDateTime`: string
   - `medicineId`: string
   - `medicineName`: string
   - `quantity`: number

### Interactive Component Mapping
- `Navbar`: 4 core tabs (`Students`, `Staff & Guards`, `Visitors / Others`, `Medicine Stock`) + `Import CSV` button.
- `StudentDirectory`: Branch filter pills, Year filter buttons, Enrollment No instant search bar, student table, and quick medicine dispenser.
- `VisitorsIntake`: Form for visitor demographic (Name, Gender, Age) with inline medicine dispensation and visitor history table.
- `PharmacyInventory`: Medicine stock cards, quick restock (+10, +50, +100), and new medicine modal.
- `CsvImportModal`: File drag-and-drop, raw text paste, sample template download, and database batch write.
