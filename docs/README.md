# Pipe Warehouse Digital Twin

## 📌 Project Overview

This repository contains high-fidelity **3D CAD models and assemblies** representing an industrial pipe warehouse environment. Designed in **SolidWorks**, the project serves as a simulation-ready digital twin for autonomous robotics testing, path planning validation, and warehouse logistics optimization.

The assembly features a modular rack system, high-density pipe inventory (>3,000 components), and a scalable layout optimized for large-assembly performance on standard hardware.

## 🛠️ Tools & Technologies

- **Software:** SolidWorks 2024 (Assembly, Part Modeling, Large Assembly Mode)  
- **Key Techniques:** Linear Component Patterns, Lightweight Mode, Advanced Mates, Pack & Go  
- **Version Control:** Git & GitHub (optimized with `.gitignore` for CAD artifacts)

## 📂 Project Structure
This project was consolidated using SolidWorks **Pack and Go** to ensure zero broken references. All CAD assets are located in the `/CAD` directory as a flat list.

The file naming convention follows the logic: `[Rows]x[Columns]__[PipeLength]`.

```text
/pipe-warehouse-digital-twin        (Repository Root)
│
├── /CAD                            (Flattened Pack & Go Directory)
│   ├── Gen_assembly.SLDASM         (The Master Warehouse Assembly)
│   │
│   ├── [Inventory Components]
│   ├── Pipe_500mm.SLDPRT
│   ├── Pipe_1000mm.SLDPRT
│   ├── Pipe_2000mm.SLDPRT
│   │
│   ├── [Rack Assemblies & Parts]   (Parametric variations)
│   ├── 10x20__0.5assembly.SLDASM
│   ├── 10x20__0.5mRack.SLDPRT
│   ├── 20x10__1massembly.SLDASM
│   ├── 20x10__1mRack.SLDPRT
│   ├── ... (and other dimensional variants)
│
├── README.md                       (Project Documentation)
└── .gitignore                      (Filters SW lock files & temp data)
```

## ⚙️ Design Methodology & Technical Challenges
### 1. High-Density Inventory Simulation (Linear Patterns)
To simulate a realistic warehouse without crashing the system, I avoided manually placing thousands of items.
* **Solution:** Modeled a single `Pipe.SLDPRT` and utilized **Linear Component Patterns** to populate racks with 200+ instances each.
* **Optimization:** Enabled **"Geometry Pattern"** to reduce rebuild time by strictly copying geometry rather than recalculating feature logic for every instance.

### 2. Large Assembly Management (>3,000 Parts)
The full environment contains thousands of pipe instances, which initially caused significant lag.
* **Lightweight Mode:** Configured the Master Assembly to load components as 'Lightweight' (loading precise geometry on-demand) instead of keeping full parametric feature history in RAM. This maintains 100% geometric accuracy for simulation while drastically reducing memory usage.
* **CAM Data Conflict:** Diagnosed and resolved a conflict where SolidWorks CAM attempted to process lightweight parts, causing errors.
    * **Fix:** Disabled the CAM Add-In to prioritize layout performance.

### 3. Layout Architecture & Mating Logic
Precise positioning was critical for robot navigation paths. I moved away from "Daisy Chaining" (mating Rack B to Rack A), which causes instability.
* **Global Reference System:** Anchored the first rack (Fixed state) as the "World Origin."
* **Row Logic:** Utilized **Distance Mates** (e.g., 35m aisle spacing) referenced against the Master Front Plane. This prevents "floating" rows and ensures that deleting one rack does not destroy the entire row's alignment.
* **Flush Alignment:** Applied Coincident Mates to the Front Faces (not edges) of racks to ensure a perfectly straight "Pick Line" for autonomous agents.

### 4. Troubleshooting & Recovery
During the assembly process, several "Over Defined" and "Circular Reference" errors occurred when new mates conflicted with existing floor plan sketches.
* **Resolution:** Adopted a "Float & Fix" strategy—breaking conflicting floor anchors before applying new distance mates, and using the "View Mates" tool to isolate and delete hidden "Ghost Mates."

## 🚀 How to Open
### 1. Clone the Repo
```bash
git clone [https://github.com/abdulmumeen-abdullahi/Pipe-Warehouse-Digital-Twin.git](https://github.com/abdulmumeen-abdullahi/Pipe-Warehouse-Digital-Twin.git)
```

### 2. Open in SolidWorks
1. Launch **SolidWorks 2024** (or compatible version).
2. **Crucial Step:** Go to `System Options > Performance` and ensure **"Automatically load components lightweight"** is CHECKED.
3. Open `General_Assembly.SLDASM`.

### 3. Visualization
* If the view looks "slanted," use `Alt + Left/Right Arrow` to straighten the horizon.
* Use `Ctrl + 7` for standard Isometric view.

---
**Created by Abdullahi Olalekan Abdulmumeen**