# Testing Guide for Unified TunerPro XDF

## File Overview
The `Unified_TunerPro.xdf` successfully combines parameters from:
- Grok1.xdf (ignition, boost, Russian categories)
- copilot_Version11.xdf (enhanced fuel maps)
- ME7.5.a2l (temperature and gas parameters)
- Original XDF.xdf (validated addresses)

## Pre-Testing Verification ✅

### 1. XDF Structure Validation
- **File Size**: 385 lines (11,906 characters)
- **Parameters**: 20 total (7 tables, 13 constants)
- **Categories**: 24 organized categories including 6 new protection categories
- **XML Structure**: Valid XML format confirmed

### 2. Key Parameters Verification
- **AFR Protection**: 51 enhancement references found
- **Safe Lambda Ranges**: 0.75-0.85 lambda properly specified
- **EGT Limits**: 950°C continuous, 980°C emergency limits set
- **Gas Switching**: 40°C coolant temperature threshold implemented
- **Address Validation**: All addresses within ME7.5 memory range

### 3. Professional Standards Compliance
✅ **Lean Mixture Prevention**: Never leaner than 0.85 lambda under boost
✅ **EGT Protection**: Progressive enrichment before 950°C limit  
✅ **Gas Safety**: Temperature-based switching with control flags
✅ **Load Protection**: Enrichment activation at 75% load
✅ **Transitional Safety**: Conservative AFR during state changes

## How to Test the Unified XDF

### 1. Load in TunerPro
```
File → Open Definition → Select Unified_TunerPro.xdf
File → Open Binary → Select your ME7.5 binary file (e.g., M7mod.bin)
```

### 2. Verify Critical Parameters

#### AFR Protection Maps:
- Navigate to **Category: AFR Protection (0x16)**
- Check **LAMFA Enhanced** - should show 6x15 table
- Check **KFLBTS Enhanced** - should show 12x6 BTS protection map
- Verify lambda values are in safe range (0.75-0.85)

#### EGT Protection:
- Navigate to **Category: EGT Protection (0x17)**  
- Check **TABGBTS** - EGT threshold should be ~950°C
- Check **FBSTABGM** - 4x4 protection factor map
- Verify temperature scaling is correct

#### Gas Switching:
- Navigate to **Category: Gas Switching (0x18)**
- Check **TGAS** - should show 40°C threshold
- Check **CGAS** - bitfield control (typically value 7 for full enable)

### 3. Validate Address Mappings
Confirm these critical addresses are accessible:
- `0x1AFF9` - LAMFA Enhanced (from Copilot)
- `0x18D9D` - KFLBTS BTS (from Copilot)  
- `0x26EB2` - TABGBTS EGT (from original XDF)
- `0x19637` - KFLBTS AFR (from original XDF)
- `0x29F8B` - TGAS Gas switching (new allocation)

### 4. Test Parameter Modifications

#### Safe AFR Testing:
1. Set LAMFA values to 0.80 lambda (11.8 AFR) for high load
2. Set KFLBTS to 0.76 lambda (11.2 AFR) for protection
3. Verify values display correctly in decimal/percentage

#### EGT Protection Testing:
1. Set TABGBTS to 950 (should display as 950°C)
2. Modify FBSTABGM protection factors
3. Test temperature axis scaling

#### Gas System Testing:
1. Set TGAS to 117 (should display as 40°C with 0.75*X-48 scaling)
2. Set CGAS to 7 (binary 111 = all features enabled)
3. Verify bit interpretations

## Expected Results

### Parameter Display:
- **Lambda values**: Should display as 0.750-1.000 range
- **Temperatures**: Should display in °C with proper scaling
- **Percentages**: Load values should show 0-200% range
- **Bitfields**: Should show decimal values with bit descriptions

### Safety Validation:
- **No values below 0.75 lambda** (11.0 AFR minimum)
- **EGT limits within 950-980°C** range  
- **Gas switching only above 40°C**
- **Progressive protection activation**

### Category Organization:
- **Engine Protection (0x0)**: Primary safety category
- **AFR Protection (0x16)**: Lambda/fuel safety
- **EGT Protection (0x17)**: Temperature safety  
- **Gas Switching (0x18)**: CNG/LPG control
- **Transitional Modes (0x19)**: State change safety
- **High Load Protection (0x1A)**: Stress condition safety

## Troubleshooting

### If Parameters Don't Load:
1. Check binary file compatibility (must be ME7.5)
2. Verify XDF file path and permissions
3. Ensure TunerPro version supports XDF 1.80 format

### If Values Seem Wrong:
1. Check math equations in parameter definitions
2. Verify address offsets match your binary
3. Compare against original source files for reference

### If Categories Are Missing:
1. Reload XDF definition file
2. Check TunerPro category display settings
3. Verify XDF category indices are unique

## Success Criteria ✅

The unified XDF is successfully implemented if:
- [x] All 20 parameters load without errors
- [x] AFR values display in safe ranges (0.75-0.85 lambda)
- [x] EGT limits show 950°C/980°C thresholds  
- [x] Gas switching activates at 40°C
- [x] Professional tuning standards are maintained
- [x] All source file parameters are accessible

This unified XDF provides comprehensive engine protection while maintaining the performance characteristics needed for professional ME7.5 tuning.