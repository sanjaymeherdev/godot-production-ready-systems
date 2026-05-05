# 🏙️ CityMaker PRO - Procedural City Generator for Godot 4

[![Godot Version](https://img.shields.io/badge/Godot-4.0%2B-blue.svg)](https://godotengine.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**Generate complete, playable cities in seconds. One click. 165+ models included.**

https://github.com/user-attachments/assets/citymaker-demo

---

## 📦 What's Included

| Item | Count | Description |
|------|-------|-------------|
| **City Generator Script** | 1 | `proceduralcity.gd` - The main generator |
| **Models** | 165+ | GLB files (bases, buildings, decors, lights) |
| **Template Scene** | 1 | `world_template.tscn` - Pre-configured model library |
| **Demo Scene** | 1 | Example generated city (40x40 grid) |

---

## 🎮 Features

### 🏗️ Smart Road System
- Lane-based roads (customizable count and width)
- Automatic junctions, corners, and T-splits
- Boundary ring with proper corner pieces
- 4 road variants (straight, corner, T-split, junction)

### 🏢 District-Based Building Placement
- **CIVIC districts**: Hospitals, schools, police stations, courts, libraries, hotels, pharmacies, sport centers
- **COMMERCIAL districts**: Shops, restaurants, gas stations, stadiums, offices
- **RESIDENTIAL districts**: Houses, apartments, towers
- Smart spacing with collision avoidance (1 tile gap)

### 🚦 Complete Lighting System
- Traffic lights at every junction (4 per intersection)
- Street lights along all roads and boundaries
- Proper rotation and positioning

### 🎯 Navigation Ready
- Road markers for junctions, corners, and T-splits
- Landmark markers for quest/mission placement
- Ready for NPC pathfinding

### 💥 Collision System
- Box collisions for simple buildings (performance optimized)
- Trimesh collisions for complex models (gas stations, stadiums)
- Ground collision for player walking

### ⚡ Performance Optimized
- MultiMesh instancing for thousands of objects
- 40x40 grid (1600 tiles) in just **14MB**
- 2,400+ objects, 120+ collision shapes

---

## 🚀 Quick Start

### Installation

1. **Copy the contents** to your Godot 4 project:
   - `proceduralcity.gd` → anywhere (e.g., `res://scripts/`)
   - `world_template.tscn` → `res://world_template.tscn`
   - All models → `res://bases/`, `res://buildings/`, `res://decors/`

2. **Open the script** in Godot Script Editor

3. **Run the script** → `File > Run` (Ctrl+Shift+X)

4. **Your city is generated** and opens automatically!

---

## ⚙️ Configuration

Edit variables at the top of `proceduralcity.gd`:

```gdscript
# Grid Configuration
var grid_size    : Vector2i = Vector2i(40, 40)   # Recommended: 40x40, Max: 100x100
var tile_size    : float    = 2.0                 # World units per tile (DON'T CHANGE)
var lanes_x      : int      = 5                   # Vertical roads (N-S)
var lanes_y      : int      = 3                   # Horizontal roads (E-W)
var lane_width   : int      = 1                   # Tiles wide per lane
var random_seed  : int      = 42                 # 0 = random each run

# Building Placement
var decor_density : float = 0.25                  # 0.0 to 1.0
var street_light_spacing : int = 4               # Every N tiles
var building_spacing : int = 1                   # Empty tiles around buildings
```

### What You Can Change

| Setting | Description | Default |
|---------|-------------|---------|
| `grid_size` | City size (X, Z tiles) | 40x40 |
| `lanes_x` | Number of vertical roads | 5 |
| `lanes_y` | Number of horizontal roads | 3 |
| `lane_width` | Road width in tiles | 1 |
| `random_seed` | City variation seed | 42 |
| `decor_density` | Decor fill percentage | 0.25 |
| `street_light_spacing` | Light interval | 4 |
| `building_spacing` | Gap between buildings | 1 |

> ⚠️ **Performance Warning:** Do not exceed `grid_size` 100x100 or the system may crash.

---

## 🎨 Changing Models / Creating a Theme

### Step 1: Create a New Template Scene

1. Create a new scene in Godot (Node3D as root)
2. Add your models as **MeshInstance3D** children
3. Name each mesh **exactly** as referenced in the script arrays

### Step 2: Update the Arrays in Script

```gdscript
# Example: Adding a new civic building
const CIVIC_BUILDINGS : Array = [
    # ... existing entries ...
    { "name": "MY_HOSPITAL", "fx": 3.5, "fz": 8.0, "type": "hospital", "pivot_y": 0.0 },
]

# Example: Adding a new decor
const DECOR_MODELS : Array = [
    # ... existing entries ...
    { "name": "my_tree", "fx": 1.0, "fz": 1.0 },
]

# Example: Adding to trimesh collision
const TRIMESH_MODELS : Array = [
    "GAS STATION_COLOR_008",
    "MY_COMPLEX_MODEL",  # Add yours here
]
```

### Step 3: Update Template Path

```gdscript
const TEMPLATE_PATH : String = "res://your_custom_template.tscn"
```

> 💡 **Important:** The mesh name in your scene must **exactly match** the `name` in the script array.

---

## 🎯 Model Categories

### Civic Buildings (Landmarks)
- Hospitals, Schools, Police Stations
- Courts, Libraries, Hotels
- Pharmacies, Sport Centers
- *Each appears once, in largest blocks*

### Commercial Buildings
- Shops, Restaurants, Gas Stations
- Stadiums, Offices
- *Fill commercial districts*

### Residential Buildings
- Houses, Apartments, Towers
- *Fill residential districts*

### Decor Models
- Trees, Benches, Cars, Bins
- Streetlights, Traffic Lights
- Rocks, Tables, Chairs

---

## 🛠️ Advanced Configuration

### Adjusting Road Rotations

If road pieces have wrong rotation, modify the `match mask` in `_assign_road_tile()`:

```gdscript
match mask:
    0b1010:  # Example corner
        model = ROAD_CORNER
        rotation = 180.0  # ← Change this value
```

### Adjusting Building Footprints

```gdscript
# Format: { "name": "MODEL_NAME", "fx": WIDTH, "fz": DEPTH, "pivot_y": OFFSET }
{ "name": "My_Building", "fx": 2.0, "fz": 3.0, "pivot_y": 0.0 }
```

---

## 📁 Folder Structure

```
res:/
├── proceduralcity.gd           # Main generator script
├── world_template.tscn         # Model library scene
├── bases/                      # 18 base/road models
│   ├── Ground_001_Material_010.glb
│   ├── road_straight3.glb
│   ├── road_corner3.glb
│   └── ...
├── buildings/                  # 84 building models
│   ├── HOSPITAL_Material_011.glb
│   ├── SCHOOL_COLOR2.glb
│   └── ...
├── decors/                     # 63 decor models
│   ├── bench3.glb
│   ├── streetlight3.glb
│   └── ...
└── cities/                     # Generated cities (auto-created)
    └── generated_city_42.tscn
```

---

## 📊 Performance

| Grid Size | Tiles | File Size | Memory | Build Time |
|-----------|-------|-----------|--------|------------|
| 20x20 | 400 | ~4 MB | Low | ~2 sec |
| **40x40** | **1,600** | **14 MB** | **Medium** | **~5 sec** |
| 60x60 | 3,600 | ~35 MB | High | ~12 sec |
| 80x80 | 6,400 | ~60 MB | Very High | ~25 sec |
| 100x100 | 10,000 | ~90 MB | Extreme | ~45 sec |

> ⚠️ **Maximum: 100x100** - Higher values may crash Godot.

---

## 🔧 Requirements

- **Godot Version:** 4.0 or higher (tested on 4.4)
- **Dependencies:** None (vanilla Godot)
- **Platform:** All platforms (editor tool, not runtime)

---

## 📖 How It Works

1. **Step 1** - Creates base grid of ground tiles
2. **Step 2** - Adds lane roads (evenly spaced)
3. **Step 3** - Builds boundary ring around city
4. **Step 4** - Detects city blocks via flood fill
5. **Step 5** - Places buildings (CIVIC → COMMERCIAL → RESIDENTIAL)
6. **Step 6** - Adds decor on remaining ground tiles
7. **Step 7** - Places traffic lights at junctions
8. **Step 8** - Places street lights along roads
9. **Step 9** - Creates road markers (NPC navigation)
10. **Step 10** - Creates landmark markers (quests)
11. **Step 11** - Generates ground collision

---

## 🆓 Free vs PRO Comparison

| Feature | Free Version | PRO Version |
|---------|--------------|-------------|
| **Models included** |14 Models | **165+** (ready to use) |
| **Grid size** | 20x20 max | 40x40 recommended (100x100 max) |
| **Building placement** | Random 2x2 only | District-based (CIVIC/COMMERCIAL/RESIDENTIAL) |
| **Traffic lights** | ❌ | ✅ |
| **Street lights** | ❌ | ✅ |
| **Road markers (NPC)** | ❌ | ✅ (junctions, corners, T-splits) |
| **Landmark markers (quests)** | ❌ | ✅ |
| **Collision system** | ❌ | ✅ (box + trimesh) |
| **MultiMesh optimization** | ❌ | ✅ (14MB for 40x40) |
| **File size** | <1 MB | 14 MB |
| **Block detection** | ❌ | ✅ |
| **District system** | ❌ | ✅ |

---

## ❓ FAQ

### Q: Can I use this at runtime?
**A:** No. This is an EditorScript designed for level generation in editor, not runtime.

### Q: Can I change the theme/style?
**A:** Yes! Create a new template scene with your models and update the arrays in script.

### Q: Why do I get a crash above 100x100?
**A:** Godot has memory limits. 100x100 = 10,000 tiles × 165 models = ~1.6M instances.

### Q: Can I use my own models?
**A:** Yes. Add them to the template scene and reference them in the script arrays.

### Q: Why aren't my buildings showing collisions?
**A:** Check if model name is in `TRIMESH_MODELS` or `NO_COLLISION_MODELS` arrays.

---

## 📝 Version History

### v2.0 (Current)
- Complete rewrite with MultiMesh optimization
- 165+ models included
- Traffic lights + street lights
- Road markers for NPC navigation
- Collision system (box + trimesh)
- District-based building placement

### v1.0 (Free Version)
- Basic grid generation
- Simple lane roads
- 2x2 building placement only

---

## 📄 License

Standard license - Use in commercial projects, royalty-free.

---

## 🙏 Credits

- Models sourced from [Your Source]
- Godot Engine team

---

## 🔗 Links

- [Free Version](link-to-free)
- [Documentation](link-to-docs)
- [Discord Support](link-to-discord)

---

**Generate your city in seconds. Start building your game today!** 🚀
eholders?
2. Create a separate NPC Navigation System README?
3. Adjust any sections?
