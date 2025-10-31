# 🎯 Multi-Line Aesop Excel Import Guide

## What's New?

The SALEM platform now **automatically detects and handles multi-line store records** from Aesop Excel exports!

## How It Works

### Multi-Line Record Detection

Your Aesop Excel data has each store split across **3-4 lines**:

**Line 1:** Core store info
```
02055	Aesop One New Change	18 Cheapside, One New Change	London	EC2V 6AH	UK	United Kingdom	+44 020 4559 8341
```

**Line 2:** Coordinates, metadata, booleans
```
51,5143 -0,096 VERDADERO	VERDADERO	SignatureStore	Aesop One New Change	['storelocator-content-store-02055', 'storelocator-store-map']
```

**Line 3:** Services, images, email, hours
```
['ClickAndCollect']	images/pages/stores/02055/Aesop_UK_Store_Oe_New_Change_Hero_Bleed_Desktop_2880x1620px.jpg	aesop.onenewchange@loreal.com	Mon: 10:00-18:00 Tue: 10:00-18:00...
```

**Line 4:** Next store starts
```
2002	Aesop Notting Hill	227A Westbourne Grove	London	W11 2SE	UK	United Kingdom	+44 204 559 8321	51,5144 -0,2002
```

## Smart Parsing Features

### 🔍 Automatic Detection
- ✅ Recognizes when Store ID starts a new record
- ✅ Identifies coordinate lines (lat/lng with commas)
- ✅ Detects array values `['item1', 'item2']`
- ✅ Merges 3-4 lines into single store record

### 🧹 Data Cleaning
- ✅ Converts European decimals: `51,5143` → `51.5143`
- ✅ Cleans arrays: `['ClickAndCollect']` → `ClickAndCollect`
- ✅ Extracts emails from anywhere in the data
- ✅ Finds image URLs automatically
- ✅ Detects store hours by pattern matching

### 📍 Field Recognition
- **Line 1 Extraction:**
  - Store ID (position 0)
  - Store Name (position 1)
  - Address (position 2)
  - City (position 3)
  - Postal Code (position 4)
  - Country (position 5 or 6)
  - Phone (position 7 or 8)

- **Line 2 Extraction:**
  - Latitude (first number -90 to 90)
  - Longitude (first number -180 to 180)
  - Page Title (text field after booleans)

- **Line 3 Extraction:**
  - Services (array with Click/Collect)
  - Images (path with images/ or .jpg)
  - Email (contains @)
  - Hours (contains Mon:, Tue:, or very long)

## Usage Instructions

1. **Copy your Aesop Excel data** (select all, Ctrl+C)
2. **Paste into SALEM** import area
3. **Click "Import Data"** button
4. **Watch the magic happen!** ✨

The system will:
- Detect it's multi-line format
- Show: `🔄 Detected multi-line Aesop Excel format`
- Merge the lines: `📦 Merged X lines into Y records`
- Parse each store: `✅ Parsed store: 02055 - Aesop One New Change`

## Console Logging

Open browser console (F12) to see:
```
🔄 Detected multi-line Aesop Excel format
📦 Merged 12 lines into 3 records
✅ Parsed store: 02055 - Aesop One New Change
✅ Parsed store: 2002 - Aesop Notting Hill
✅ Parsed store: 2004 - Aesop Covent Garden
```

## Troubleshooting

### Data Still Wrong?
- Check browser console for parsing errors
- Verify your data matches the 3-4 line pattern
- Ensure tabs are preserved (Excel default)

### Missing Fields?
- Some fields may be in different positions
- The parser looks for patterns, not fixed positions
- Email, phone, coordinates use pattern matching

### European Decimals?
- All comma decimals are automatically converted
- `51,5143` becomes `51.5143`
- Works for both latitude and longitude

## What's Different from Before?

**Before:** Each line treated as separate store → Data misaligned ❌

**Now:** Related lines merged into one store → Perfect alignment ✅

## Technical Details

The system uses three-layer intelligent detection:

1. **Pattern Recognition:** Identifies multi-line format by looking for coordinate/array continuation lines
2. **Record Grouping:** Groups 3-4 lines by detecting Store ID boundaries
3. **Smart Extraction:** Uses pattern matching instead of fixed positions

No Python needed - **pure JavaScript intelligence**! 🚀
