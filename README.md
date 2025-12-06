# ISRO Satellite Link Budget Calculator

A Python tool to analyze satellite communication links for ISRO satellites using physics equations.

## 📚 Physics Behind the Calculations

### 1. Free Space Path Loss (FSPL)
The signal weakens as it travels through space:

$L = 20 \times \log_{10}(d) + 20 \times \log_{10}(f) + 92.45$

Where:
- $d$ = distance in kilometers
- $f$ = frequency in GHz  
- $L$ = loss in decibels (dB)

### 2. Received Power Calculation
Power received at ground station:

$P_r = P_t + G_t + G_r - L - L_{extra}$

Where:
- $P_t$ = transmit power (dBm)
- $G_t$ = satellite antenna gain (dBi)
- $G_r$ = ground station antenna gain (dBi)
- $L_{extra}$ = additional losses (atmospheric, rain, etc.)

### 3. Link Feasibility Check
A link is considered feasible if:

$\text{Margin} = P_r - P_{\text{required}} > 3 \text{ dB}$

Where $P_{\text{required}} = -80 \text{ dBm}$ (typical receiver sensitivity).

## 📊 Analysis Results

### GSAT-30 (Telecommunication Satellite)
**Parameters:**
- Orbit: GEO (36,000 km)
- Frequency: 6.0 GHz (C-band)
- Transmit Power: 40 dBm (10.0 W)
- Antenna Gains: Satellite 30 dBi + Ground 45 dBi = 75 dBi total

**Results:**
- Free Space Loss: 199.1 dB
- Total Loss: 201.1 dB
- Received Power: -86.1 dBm
- Link Margin: -6.1 dB
- **Status: ⚠️ MARGINAL/INFEASIBLE**
- Needed Improvement: 9.1 dB

### IRNSS-1G (Navigation Satellite)
**Parameters:**
- Orbit: GEO (36,000 km)
- Frequency: 1.5 GHz (L-band)
- Transmit Power: 32 dBm (1.6 W)
- Antenna Gains: Satellite 28 dBi + Ground 45 dBi = 73 dBi total

**Results:**
- Free Space Loss: 187.0 dB
- Total Loss: 189.0 dB
- Received Power: -84.0 dBm
- Link Margin: -4.0 dB
- **Status: ⚠️ MARGINAL/INFEASIBLE**
- Needed Improvement: 7.0 dB

### RISAT-2B (Radar Imaging Satellite)
**Parameters:**
- Orbit: LEO (600 km)
- Frequency: 3.1 GHz (S-band)
- Transmit Power: 38 dBm (6.3 W)
- Antenna Gains: Satellite 25 dBi + Ground 45 dBi = 70 dBi total

**Results:**
- Free Space Loss: 157.8 dB
- Total Loss: 159.8 dB
- Received Power: -51.8 dBm
- Link Margin: 28.2 dB
- **Status: ✅ FEASIBLE (Good margin)**

### SCATSAT-1 (Ocean Monitoring Satellite)
**Parameters:**
- Orbit: LEO (720 km)
- Frequency: 13.5 GHz (Ku-band)
- Transmit Power: 42 dBm (15.8 W)
- Antenna Gains: Satellite 32 dBi + Ground 45 dBi = 77 dBi total

**Results:**
- Free Space Loss: 172.2 dB
- Total Loss: 174.2 dB
- Received Power: -55.2 dBm
- Link Margin: 24.8 dB
- **Status: ✅ FEASIBLE (Good margin)**

## 📈 Summary Table

| Satellite | Orbit | Band | Rx Power | Margin | Status |
|-----------|-------|------|----------|--------|--------|
| GSAT-30 | GEO | C | -86.1 dBm | -6.1 dB | ⚠️ Marginal |
| IRNSS-1G | GEO | L | -84.0 dBm | -4.0 dB | ⚠️ Marginal |
| RISAT-2B | LEO | S | -51.8 dBm | 28.2 dB | ✅ Good |
| SCATSAT-1 | LEO | Ku | -55.2 dBm | 24.8 dB | ✅ Good |

## 🔧 Key Engineering Insights

1. **GEO vs LEO Impact**: GEO satellites (36,000 km) have ~40 dB more loss than LEO satellites (~600-700 km)
2. **Frequency Effect**: Higher frequencies (Ku-band) have higher path loss but LEO orbit compensates
3. **Critical Finding**: Both GEO satellites show negative margins, indicating real engineering challenges
4. **LEO Advantage**: LEO satellites provide excellent margins (>24 dB) despite atmospheric losses

## 🛠️ Improvement Recommendations

For marginal GEO satellites (GSAT-30, IRNSS-1G):

1. **Increase Ground Station Gain**
   - Current: 45 dBi
   - Target: 54 dBi for GSAT-30 (+9.1 dB)
   - Target: 52 dBi for IRNSS-1G (+7.0 dB)

2. **Increase Transmit Power**
   - GSAT-30: 40 dBm → 49.1 dBm (82W vs 10W)
   - IRNSS-1G: 32 dBm → 39.0 dBm (8W vs 1.6W)

3. **Improve Receiver Sensitivity**
   - Current requirement: -80 dBm
   - Target: -89 dBm for GSAT-30
   - Better modulation/coding schemes

## 🚀 Usage

```python
# Example: Analyze GSAT-30
from satellite_link import analyze_satellite

result = analyze_satellite(
    name="GSAT-30",
    distance_km=36000,
    frequency_GHz=6.0,
    tx_power_dBm=40,
    tx_gain_dBi=30,
    rx_gain_dBi=45
)

print(f"Link Margin: {result['margin']} dB")
print(f"Feasible: {result['feasible']}")
