# 📘 government fund distribution system

## 🏛 Problem Statement

Government launches a welfare scheme **“JanDhan Money Scheme”** to provide financial assistance to economically weaker individuals (yearly income < ₹90,000).

The fund distribution follows a hierarchical structure:

```
Central Government
        ↓
State Government
        ↓
District
        ↓
Panchayat
        ↓
Individual Person
        ↓
Bank (Final Delivery)
```

Each level:

* Allocates money to the lower level
* Tracks allocation & delivery
* Maintains records

The **Bank** is responsible for final transfer to eligible individuals.

---

# 🎯 Objective

Build a React + Redux Toolkit application to:

* Manage hierarchical government bodies
* Allocate funds across levels
* Track delivery status
* Deliver funds via Bank system
* Show dashboard reports
* Maintain complete audit trail

---

# 🏗 System Architecture Overview

```
                         ┌──────────────────┐
                         │ Central Gov      │
                         │ Allocates Fund   │
                         └─────────┬────────┘
                                   ↓
                         ┌──────────────────┐
                         │ State Gov        │
                         │ Allocates Fund   │
                         └─────────┬────────┘
                                   ↓
                         ┌──────────────────┐
                         │ District         │
                         │ Allocates Fund   │
                         └─────────┬────────┘
                                   ↓
                         ┌──────────────────┐
                         │ Panchayat        │
                         │ Allocates Fund   │
                         └─────────┬────────┘
                                   ↓
                         ┌──────────────────┐
                         │ Person           │
                         │ (Income < 90k)   │
                         └─────────┬────────┘
                                   ↓
                         ┌──────────────────┐
                         │ Bank             │
                         │ Transfers Money  │
                         └──────────────────┘
```

---

# 🧠 Core Modules

## 1️⃣ Government Hierarchy Module

Handles:

* Central
* State
* District
* Panchayat

Tracks:

* totalMoneyAllocated
* totalMoneyDelivered

---

## 2️⃣ Person Module

Stores:

```
personUniqueId
name
age
occupation
yearlyIncome
fatherName
village
state
panchayatId
districtId
```

Eligibility Rule:

```
yearlyIncome < 90000
```

---

## 3️⃣ Bank Module

Stores:

```
uniqueId
bankName
accountNumber
ifscCode
personId
moneyDeliveryStatus (PENDING | SUCCESS | FAILED)
```

---

## 4️⃣ Money Allocation Module

Tracks:

```
allocatedBy: 
    CENTER_GOV 
    STATE_GOV 
    DISTRICT_LEVEL 
    PANCHAYAT_LEVEL

allocatedTo:
    STATE_GOV 
    DISTRICT_LEVEL 
    PANCHAYAT_LEVEL 
    PERSON

govBodyName
govBodyId
totalMoneyAllocation
totalMoneyDelivered
```

---

# 🔄 Interaction Flow

## 🟢 Step 1: Central Gov Allocation

* Allocates ₹X to State
* State record updated

## 🟢 Step 2: State Allocation

* Distributes to District

## 🟢 Step 3: District Allocation

* Distributes to Panchayat

## 🟢 Step 4: Panchayat Allocation

* Assigns to eligible Persons

## 🟢 Step 5: Bank Delivery

* Checks eligibility
* Transfers amount
* Updates delivery status

---

# 🧩 Application Architecture

## Frontend

* React (JSX mode)
* Tailwind CSS
* Redux Toolkit
* React Router

## Backend (Mock Server)

* JSON Server

---

# 📁 Proposed Folder Structure

```
mygov-jandhan-money-scheme/
│
├── public/
│
├── src/
│   ├── app/
│   │   └── store.js
│   │
│   ├── features/
│   │   ├── government/
│   │   │   ├── governmentSlice.js
│   │   │   ├── GovernmentList.jsx
│   │   │   └── GovernmentForm.jsx
│   │   │
│   │   ├── person/
│   │   │   ├── personSlice.js
│   │   │   ├── PersonList.jsx
│   │   │   └── PersonForm.jsx
│   │   │
│   │   ├── bank/
│   │   │   ├── bankSlice.js
│   │   │   ├── BankList.jsx
│   │   │   └── BankTransfer.jsx
│   │   │
│   │   └── allocation/
│   │       ├── allocationSlice.js
│   │       ├── AllocationForm.jsx
│   │       └── AllocationReport.jsx
│   │
│   ├── pages/
│   │   ├── Dashboard.jsx
│   │   ├── GovernmentPage.jsx
│   │   ├── PersonPage.jsx
│   │   ├── BankPage.jsx
│   │   └── AllocationPage.jsx
│   │
│   ├── routes/
│   │   └── AppRoutes.jsx
│   │
│   ├── hooks/
│   │   └── useEligibility.js
│   │
│   ├── services/
│   │   └── api.js
│   │
│   ├── App.jsx
│   └── main.jsx
│
├── db.json
├── package.json
└── README.md
```

---

# 🏛 Central Hub Concept

The Redux Store acts as:

```
                Redux Store (Central Hub)
                       ↓
    ┌─────────────┬─────────────┬─────────────┬─────────────┐
 Government    Person        Bank       Allocation
   Slice        Slice        Slice          Slice
```

All modules:

* Read from Store
* Dispatch actions
* Sync with JSON server

---

# 🧮 Business Rules

1. Person eligible only if income < 90000
2. Allocation cannot exceed available balance
3. Delivery status updates only via Bank module
4. Total delivered ≤ total allocated
5. Each gov body tracks:

   * Allocated amount
   * Delivered amount
   * Remaining balance

---

# 📊 Future Enhancements

* Role-based login (Central/State/District)
* Dashboard charts
* Audit logs
* Error handling
* Pagination
* RTK Query integration
* Production backend (Spring Boot)

---