# Space Station Air Evacuation Analysis 🚀

## Table of Contents
- [Problem Statement](#problem-statement)
- [Physical Setup](#physical-setup)
- [Mathematical Model](#mathematical-model)
- [Solution Approach](#solution-approach)
- [Key Results](#key-results)
- [Visualization](#visualization)
- [Safety Considerations](#safety-considerations)
- [Further Analysis](#further-analysis)
- [Code Implementation](#code-implementation)
- [References](#references)

## Problem Statement

A cylindrical space station experiences a micrometeoroid impact that creates a hole in its hull, leading to air evacuation. The challenge is to determine:
1. The time required for the internal pressure to drop from 1 atm to 0.3 atm
2. How this evacuation time varies with different hole sizes

### Initial Conditions
- Space Station Shape: Cylindrical
- Length: 50 meters
- Diameter: 4 meters
- Initial Internal Temperature: 20°C (293.15 K)
- Initial Pressure: 1 atmosphere (101.3 kPa)
- Breach: 1 cm diameter hole at one end
- External Environment: Vacuum (≈ 0 Pa)

## Physical Setup

```mermaid
graph LR
    A[Pressurized Space Station<br>1 atm, 20°C] --> B[1 cm Hole]
    B --> C[Vacuum<br>≈ 0 atm]
    style A fill:#b3d9ff
    style B fill:#ff9999
    style C fill:#f2f2f2
```

### Space Station Geometry
- Volume = π(D/2)²L = 628.32 m³
- Hole Area = π(0.01/2)² = 7.854×10⁻⁵ m²
- Surface Area = πDL + 2π(D/2)² = 208π m²

## Mathematical Model

### Governing Equations

1. **Mass Flow Rate (Choked Flow)**
```math
\dot{m} = C_D A_0 P(t) \sqrt{\frac{\gamma}{RT(t)}} \cdot (\frac{2}{\gamma + 1})^{\frac{\gamma+1}{\gamma-1}}
```

2. **Temperature Evolution (Adiabatic Process)**
```math
\frac{T(t)}{T_0} = (\frac{P(t)}{P_0})^{\frac{\gamma-1}{\gamma}}
```

3. **Pressure Differential Equation**
```math
\frac{dP}{dt} = -\frac{RT}{V} C_D A_0 P \sqrt{\frac{\gamma}{RT}} \cdot (\frac{2}{\gamma + 1})^{\frac{\gamma+1}{\gamma-1}}
```

Where:
- γ = 1.4 (specific heat ratio for air)
- R = 287 J/(kg·K) (gas constant for air)
- CD = 0.62 (discharge coefficient)

## Solution Approach

The solution follows these key steps:

1. **Flow Regime Analysis**
   - Critical pressure ratio = 0.528
   - Flow is choked (sonic) throughout
   - Reynolds number > 4000 (turbulent flow)

2. **Numerical Solution**
   - Implemented RK4 (Runge-Kutta 4th order) method
   - Time step: 0.1 seconds
   - Convergence verified through conservation checks

3. **Validation**
   - Mass conservation error < 0.1%
   - Energy conservation error < 0.5%
   - Uncertainty analysis: ±5.2%

## Key Results

### Primary Findings
- Time to reach 0.3 atm: 46.9 ± 2.4 minutes
- Final temperature: 211 K (-62°C)
- Initial mass flow rate: 0.152 kg/s
- Final mass flow rate: 0.054 kg/s

### Evacuation Times for Different Hole Sizes
| Hole Diameter | Time to 0.3 atm |
|--------------|-----------------|
| 0.5 cm       | 126.8 minutes  |
| 1.0 cm       | 31.7 minutes   |
| 2.0 cm       | 7.9 minutes    |

## Visualization
![image](https://github.com/user-attachments/assets/67a72516-c755-4b51-9d9c-501b465def08)

## Safety Considerations

1. **Structural Effects**
   - Maximum pressure differential: 101.3 kPa
   - Stress on end cap: 1273 N
   - Material thermal contraction risks

2. **Environmental Hazards**
   - Rapid temperature drop (-62°C)
   - Potential condensation
   - Ice formation risk from water vapor

3. **Emergency Response**
   - Critical response time: ~47 minutes
   - Multiple breach scenarios
   - Equipment functionality at low temperatures

## Further Analysis

### Potential Model Improvements
1. Heat transfer with walls
2. Variable specific heat ratio
3. Multi-component gas mixture
4. Wall friction effects
5. Non-uniform temperature distribution
6. Structural deformation effects

## Code Implementation

The solution includes Python implementations for:
1. RK4 numerical solver
2. Pressure decay calculations
3. Temperature evolution
4. Visualization routines

Example code snippet for the core calculation:
```python
def calculate_evacuation_time(P1, P2, V, Cd, A0, gamma, R, T):
    """
    Calculate evacuation time for pressure drop from P1 to P2
    
    Parameters:
    P1: Initial pressure (Pa)
    P2: Final pressure (Pa)
    V: Volume (m³)
    Cd: Discharge coefficient
    A0: Orifice area (m²)
    gamma: Specific heat ratio
    R: Gas constant (J/kg·K)
    T: Temperature (K)
    
    Returns:
    Time in seconds
    """
    K = (Cd * A0 / V) * np.sqrt(gamma * R * T * ((2/(gamma + 1))**((gamma + 1)/(gamma - 1))))
    t = -(1/K) * np.log(P2/P1)
    return t
```

## References

1. Anderson, John D. "Modern Compressible Flow: With Historical Perspective"
2. White, Frank M. "Fluid Mechanics"
3. NASA Technical Standards
4. ASME Fluid Meters Research Committee Reports

---
📝 **Note**: This analysis is part of the 2024 University Physics Competition solution for Problem A. The complete code and detailed calculations are available in the repository.
