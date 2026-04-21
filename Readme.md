# 🏨 The Azure Stay — Channel Profitability Analysis

> **Data Analytics Project | Problem 2: High Distribution Costs**
> Designed by: NFT | Date: 17/01/2569

---

## 📌 Background & Problem Statement

**The Azure Stay** คือโรงแรมขนาดกลางแบบ Independent (ไม่สังกัดเชน) ที่มีทำเลดีและคุณภาพบริการสูง แม้จะมีอัตราการเข้าพัก (Occupancy) ที่น่าพอใจ แต่กำไรกลับอยู่ในภาวะชะงักงัน (Profit Stagnation)

**ปัญหาหลัก:** โรงแรมขายห้องพักได้จำนวนมาก แต่ต้องจ่ายค่าคอมมิชชั่นให้กับบุคคลที่สาม (OTA) สูงเกินไป ซึ่งกัดกร่อนรายได้สุทธิที่ควรตกถึงโรงแรมอย่างต่อเนื่อง

---

## 🎯 SMART Objectives

ลดต้นทุนจากค่าคอมมิชชั่นและเพิ่มรายได้สุทธิ (Net Revenue) **อย่างน้อย 12% ภายใน 6 เดือน** โดยปรับสัดส่วนช่องทางการจอง

| เป้าหมาย | รายละเอียด |
|---|---|
| **S**pecific | เพิ่มรายได้สุทธิจากห้องพัก (Net Room Revenue) |
| **M**easurable | เพิ่มขึ้นอย่างน้อย +12% |
| **A**chievable | ปรับสัดส่วนช่องทาง (ลด OTA / เพิ่ม Direct) |
| **R**elevant | สอดคล้องกับเป้าหมาย "Maximize Net Revenue" |
| **T**ime-bound | ภายใน 6 เดือน |

> 💡 **Value Proposition:** เพิ่มกำไรโดยไม่จำเป็นต้องเพิ่มจำนวนห้องที่ขาย

---

## ❓ Research Questions & Hypotheses

### Hypotheses
1. **Direct channels** มี Net ADR และ Net Margin สูงกว่า OTA channels อย่างมีนัยสำคัญ
2. ช่องทาง **OTA มีอัตราการยกเลิกการจองสูงกว่า** ส่งผลให้รายได้ที่เกิดขึ้นจริงลดลง
3. **Channels ที่มี Commission สูง** จะมี Net Revenue ต่อ Booking ต่ำกว่า

### Key Questions
1. ช่องทางที่มี Net ADR สูง → มี Volume Bookings ต่ำหรือสูง?
2. OTA vs Direct → ต่างกันกี่ % ใน Net Margin?
3. Cancellation ส่งผลต่อ Net Revenue จริง มากแค่ไหน?
4. มีความสัมพันธ์ระหว่าง Commission สูง ↔ Net ADR ต่ำ จริงหรือไม่?
5. ถ้าตัด OTA ออก → รายได้หายไปเท่าไร vs Margin ดีขึ้นเท่าไร?

---

## 📊 Dataset Overview

ข้อมูลครอบคลุมช่วง **1 มกราคม 2024 – 31 ธันวาคม 2024**

| Table | Rows | Description |
|---|---|---|
| `fact_bookings` | 5,000 | ข้อมูลการจองทั้งหมด (Gross Revenue, Commission, Net Revenue, Status) |
| `dim_channels` | 8 | ข้อมูลช่องทางการจอง (OTA, Direct, Corporate, GDS, Offline) |
| `dim_rate_codes` | 7 | ประเภทราคา (BAR, Corporate, Promo, Net, Walk-in ฯลฯ) |
| `fact_marketing_spend` | 1,256 | ข้อมูลงบการตลาดแยกตามช่องทาง |

### Booking Channels

| Channel ID | Channel Name | Type | Commission Rate |
|---|---|---|---|
| CH_01 | Booking.com | OTA | 15% |
| CH_02 | Expedia | OTA | 18% |
| CH_03 | Agoda | OTA | 12% |
| CH_04 | Direct Website | Direct | 0% |
| CH_05 | Walk-in | Direct | 0% |
| CH_06 | Corporate Direct | Corporate | 5% |
| CH_07 | Travel Agency | Offline | 10% |
| CH_08 | GDS (Amadeus) | GDS | 20% |

### Rate Codes

| Rate Code | Rate Name | Commissionable |
|---|---|---|
| RT_BAR | Best Available Rate | ✅ |
| RT_CORP | Corporate Rate | ✅ |
| RT_PKG | Package Rate | ✅ |
| RT_NET | Net Rate (OTA) | ❌ |
| RT_PROMO | Promotional Rate | ✅ |
| RT_WALK | Walk-in Rate | ❌ |
| RT_GOVT | Government Rate | ❌ |

---

## 📐 Key Metrics (KPIs)

```
Net ADR       = (Gross Room Revenue - Commission) / จำนวนห้องที่ขายได้
Net RevPAR    = (Gross Room Revenue - Commission) / จำนวนห้องพักทั้งหมด
COA %         = (Commission + Marketing Spend) / Gross Revenue
Net Margin %  = Net Room Revenue / Gross Room Revenue × 100
```

---

## 🔍 Preliminary Findings (from Excel Analysis)

### Net Margin by Channel Type

| Channel Type | Net Margin | Avg. Net Room Revenue |
|---|---|---|
| Direct | **100%** | ฿9,632 |
| Corporate | 97.4% | ฿7,415 |
| Offline | 90.0% | ฿7,893 |
| OTA | 90.6% | ฿7,454 |
| GDS | **80.0%** | ฿6,607 |

> ✅ Direct channels มี Net Margin สูงสุด (100%) สอดคล้องกับ Hypothesis ที่ตั้งไว้
> ⚠️ GDS (Amadeus) มี Net Margin ต่ำสุดที่ 80% จาก Commission 20%

### Cancellation Impact (Checked-Out bookings only)

| Channel Type | Checked-Out Bookings | Net Revenue |
|---|---|---|
| OTA | รวม 3 ช่องทาง | สูงสุด (volume มาก) |
| Direct | 714 | ฿6,859,361 |
| Corporate | 439 | ฿3,279,589 |
| GDS | 209 | ฿1,366,196 |

---

## 🗂️ Repository Structure

```
📦 azure-stay-channel-profitability/
├── 📄 README.md
├── 📊 data/
│   └── hotel_channel_profitability_v2.xlsx    # Raw dataset
├── 📓 notebooks/
│   ├── 01_data_exploration.ipynb              # EDA & Data Cleaning
│   ├── 02_channel_net_margin_analysis.ipynb   # H1: Net ADR & Net Margin
│   ├── 03_ota_cancellation_analysis.ipynb     # H2: Cancellation Rate
│   └── 04_commission_impact_analysis.ipynb    # H3: Commission vs Net Revenue
├── 📈 dashboard/
│   └── channel_profitability_dashboard.*      # Visualization output
└── 📋 docs/
    └── Project_Canvas.pptx                    # Project Canvas
```

---

## 🛠️ Tools & Technologies

- **Python** — pandas, matplotlib, seaborn
- **Excel / Power Pivot** — Exploratory analysis & Pivot Tables
- **Jupyter Notebook** — Analysis workflow
- **GitHub** — Version control

---
---
## 📖 Data Dictionary
คำอธิบาย Attribute ทั้งหมดของ Dataset — Hotel Channel Profitability
from IPython.display import display, HTML

# ============================================================
# Data Dictionary — Hotel Channel Profitability Dataset
# สไตล์ตาม Dark Theme เหมือนตัวอย่าง
# ============================================================

data_dict = [
    # (Attribute, Description_TH, Data_Type, Valid_Range_Example)

    # ── fact_bookings ──
    ('booking_id',          'รหัสออเดอร์ที่ใช้ระบุแต่ละการจอง',
                             'Nominal',                'BK10001, BK15000'),
    ('booking_date',        'วันที่มีการสั่งจอง',
                             'Interval (Date)',        '2024-01-01 ถึง 2024-12-31'),
    ('check_in_date',       'วันที่ Check-in เข้าพัก',
                             'Interval (Date)',        '2024-01-01 ถึง 2025-01-30'),
    ('channel_id',          'รหัสช่องทางการจอง (Foreign Key)',
                             'Nominal',                'CH_01 ถึง CH_08'),
    ('rate_code_id',        'รหัสประเภทอัตราค่าห้อง (Foreign Key)',
                             'Nominal',                'RT_BAR, RT_CORP, RT_PROMO, RT_NET, RT_PKG, RT_WALK, RT_GOVT'),
    ('gross_room_revenue',  'รายได้ห้องพักก่อนหักค่า Commission (บาท)',
                             'Ratio (Continuous)',     '[1,787.82, 39,699.77] THB'),
    ('commission_amount',   'ค่า Commission ที่จ่ายให้ช่องทางการจอง (บาท)',
                             'Ratio (Continuous)',     '[0, 6,003.80] THB (0 = Direct/Net Rate)'),
    ('net_room_revenue',    'รายได้ห้องพักหลังหักค่า Commission (บาท)',
                             'Ratio (Continuous)',     '[1,609.77, 39,699.77] THB'),
    ('status',              'สถานะของการจอง',
                             'Nominal',                'Checked-Out, Cancelled, Confirmed'),
    ('net_margin',          'อัตราส่วน Net/Gross Revenue (Gross ÷ Net)',
                             'Ratio (Continuous)',     '[1.000, 1.250] — Direct=1.000, GDS=1.250'),
    ('channel_type',        'ประเภทของช่องทางการจอง (Category)',
                             'Nominal',                'OTA, Direct, Corporate, Offline, GDS'),
    ('channel_name',        'ชื่อของช่องทางการจอง',
                             'Nominal',                'Booking.com, Expedia, Agoda, Direct Website, Walk-in, Corporate Direct, Travel Agency, GDS (Amadeus)'),
    ('Cancelled Flag',      'Flag = 1 หากการจองถูกยกเลิก, 0 หากไม่ใช่',
                             'Nominal (Binary)',       '0 = ไม่ยกเลิก, 1 = ยกเลิก'),
    ('Checked-Out Flag',    'Flag = 1 หากการจอง Checked-Out สำเร็จ, 0 หากไม่ใช่',
                             'Nominal (Binary)',       '0 = ยังไม่ Check-out, 1 = Check-out แล้ว'),
    ('Net_revenue',         'รายได้สุทธิที่เกิดขึ้นจริง (0 หากถูกยกเลิก)',
                             'Ratio (Continuous)',     '0 (Cancelled/Confirmed) หรือ = net_room_revenue'),
    ('Gross Revenue',       'รายได้รวมที่เกิดขึ้นจริง (0 หากถูกยกเลิก)',
                             'Ratio (Continuous)',     '0 (Cancelled/Confirmed) หรือ = gross_room_revenue'),

    # ── dim_channels ──
    ('channel_id',          '[dim_channels] รหัสช่องทางการจอง (Primary Key)',
                             'Nominal',                'CH_01 ถึง CH_08'),
    ('channel_name',        '[dim_channels] ชื่อช่องทางการจอง',
                             'Nominal',                'Booking.com, Expedia, Agoda, Direct Website, Walk-in, Corporate Direct, Travel Agency, GDS (Amadeus)'),
    ('channel_type',        '[dim_channels] ประเภทของช่องทาง',
                             'Nominal',                'OTA, Direct, Corporate, Offline, GDS'),
    ('commission_model',    '[dim_channels] รูปแบบการคิด Commission',
                             'Nominal',                'Percentage, Flat Fee'),
    ('default_commission_rate','[dim_channels] อัตรา Commission มาตรฐาน',
                             'Ratio (Continuous)',     '0.00 (Direct) ถึง 0.20 (GDS)'),
    ('contract_owner',      '[dim_channels] ผู้รับผิดชอบสัญญาช่องทาง',
                             'Nominal',                'Somchai P., Nattaya K., Wichai S.'),

    # ── dim_rate_codes ──
    ('rate_code_id',        '[dim_rate_codes] รหัสประเภทราคา (Primary Key)',
                             'Nominal',                'RT_BAR, RT_CORP, RT_PKG, RT_NET, RT_PROMO, RT_WALK, RT_GOVT'),
    ('rate_name',           '[dim_rate_codes] ชื่อประเภทราคา',
                             'Nominal',                'Best Available Rate, Corporate Rate, Package Rate, Net Rate (OTA), Promotional Rate, Walk-in Rate, Government Rate'),
    ('is_commissionable',   '[dim_rate_codes] ประเภทราคานี้คิด Commission หรือไม่',
                             'Nominal (Boolean)',      'True = คิด Commission, False = ไม่คิด (Net/Walk-in/Govt)'),
]

# ──────────── Build HTML ────────────
rows_html = ''
section_headers = {
    0  : 'fact_bookings — ตารางข้อมูลการจองหลัก (5,000 rows)',
    16 : 'dim_channels — ตารางข้อมูลช่องทาง (8 rows)',
    22 : 'dim_rate_codes — ตารางรหัสประเภทราคา (7 rows)',
}

for i, (attr, desc, dtype, example) in enumerate(data_dict):
    # Section header row
    if i in section_headers:
        rows_html += f'''
        <tr>
          <td colspan="4" style="
            background:#1A6B4A; color:white; font-weight:bold;
            font-size:13px; padding:10px 16px; letter-spacing:0.5px;
          ">📁 {section_headers[i]}</td>
        </tr>
        '''

    bg = '#2C3E50' if i % 2 == 0 else '#34495E'
    tag_color = '#27AE60'
    if 'dim_channels' in desc:   tag_color = '#2980B9'
    if 'dim_rate_codes' in desc: tag_color = '#8E44AD'

    # Data type badge color
    if 'Nominal'   in dtype: badge_c = '#2980B9'
    elif 'Ordinal' in dtype: badge_c = '#8E44AD'
    elif 'Ratio'   in dtype: badge_c = '#16A085'
    elif 'Interval'in dtype: badge_c = '#E67E22'
    elif 'Boolean' in dtype: badge_c = '#C0392B'
    elif 'Binary'  in dtype: badge_c = '#C0392B'
    else:                    badge_c = '#7F8C8D'

    rows_html += f'''
    <tr style="background:{bg}; color:#ECF0F1;">
      <td style="padding:10px 14px; font-weight:bold; font-size:13px;
                 border-right:1px solid #1A252F; white-space:nowrap;
                 color:#F8C471;">{attr}</td>
      <td style="padding:10px 14px; font-size:13px; border-right:1px solid #1A252F;">{desc}</td>
      <td style="padding:10px 14px; text-align:center; border-right:1px solid #1A252F; white-space:nowrap;">
        <span style="background:{badge_c}; color:white; padding:3px 9px;
                     border-radius:12px; font-size:11.5px; font-weight:bold;">{dtype}</span>
      </td>
      <td style="padding:10px 14px; font-size:12px; color:#BDC3C7;">{example}</td>
    </tr>
    '''

html = f'''
<style>
  .dd-table {{ border-collapse:collapse; width:100%; font-family:'Segoe UI',Arial,sans-serif; }}
  .dd-table td {{ border-bottom:1px solid #1A252F; vertical-align:middle; }}
</style>
<div style="background:#1A252F; border-radius:10px; padding:20px 20px 8px 20px;
            box-shadow:0 4px 15px rgba(0,0,0,0.4); margin-bottom:10px;">
  <h2 style="color:white; font-family:'Segoe UI',Arial,sans-serif;
             margin:0 0 16px 0; font-size:20px; letter-spacing:0.5px;">
    📖 Data Dictionary &nbsp;
    <span style="background:#F39C12; padding:3px 10px; border-radius:6px;
                 font-size:13px; font-weight:bold; color:#1A252F;">Hotel Channel Profitability</span>
  </h2>
  <table class="dd-table">
    <thead>
      <tr style="background:#0D1B2A;">
        <th style="padding:12px 14px; text-align:left; color:#F39C12;
                   font-size:14px; font-weight:bold; white-space:nowrap;">Attribute</th>
        <th style="padding:12px 14px; text-align:left; color:#F39C12; font-size:14px; font-weight:bold;">Description (คำอธิบาย)</th>
        <th style="padding:12px 14px; text-align:center; color:#F39C12; font-size:14px; font-weight:bold; white-space:nowrap;">Data Type</th>
        <th style="padding:12px 14px; text-align:left; color:#F39C12; font-size:14px; font-weight:bold;">Valid Range / Example</th>
      </tr>
    </thead>
    <tbody>
      {rows_html}
    </tbody>
  </table>
  <p style="color:#7F8C8D; font-size:11px; margin:10px 0 4px 4px;">
    Dataset: 5,000 bookings | 8 channels | Period: 2024-01-01 to 2024-12-31 | Currency: THB
  </p>
</div>
'''

## 👤 Author

**NFT** | Data Analytics Project | SWU
*Modified from Bill Schmarzo's Machine Learning Canvas and Jasmine Vasandani's Data Science Workflow Canvas*