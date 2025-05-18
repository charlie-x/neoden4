# Comprehensive Neoden 4 Pick and Place Machine Guide

## Board Design and Preparation

### Eagle ULP Setup & CSV Generation
- Use the Neoden4 ULP (charliex@nullspacelabs.com version 1.0j) to generate the proper CSV format
- The ULP identifies components by value and package
- Components need specific attributes in Eagle to be recognized correctly:
 - Set attribute "FORCEPNP" to force a part to be added to the list
 - Set attribute "NOPNP" to exclude a part
 - Set value="DNP" to include but skip a part
 - Parts are skipped by default if they have through-hole pads, are test pads, jumpers, or have zero-ohm values
- For fiducials, use "FID" in either the name or value field for automatic detection
- The ULP allows setting board offset, rotation, and movement speed override
- The "+" in Eagle parts MUST represent the centroid of the component for accurate placement
- The ULP combines parts with the same value and package for easier programming

### Fiducials
- Add at least two 1mm round fiducials to boards
- Alphabetically name fiducials to be first in the list (e.g., "AFID1", "AFID2")
- Fiducials should have a large keepout area with no other round shapes nearby
- First components in your list should always be fiducials
- For fiducial detection settings, adjust Min-Max values (0.8-3.0 recommended)
- Light source options: use inner ring (1) for board holes as fiducials, use outer ring for light-spots as fiducials
- If no fiducials exist on the board, choose a component far from the first component to serve as a fiducial
- The ULP automatically detects fiducials based on "FID" in the name or value

### Part Designations
- Set attribute "NOPNP" to exclude a part
- Set value="DNP" to include but skip a part
- Set attribute "FORCEPNP" to force a part to be added to the list
- Parts beginning with "_" will be sorted last, so use "A" or numbers for priority
- The ULP skips certain parts by default: through-hole parts, test pads, jumpers, zero-ohm values
- Reel assignments can be saved in a ".PnP" file for reuse

## Machine Hardware Configuration

### Equipment Preparation
- If machine's monitor doesn't power on, VLC into it and check the notebook switch in Intel drivers
- Holes on work plate are M3 sized
- Clean nozzles regularly
- Tape goes over brass part or small components will bounce during feed
- Tape leader goes in front of brass parts
- Vibratory feeder doesn't turn off unless manually turned off (unloading leaves it running)
- Maintain operating environment: temperature 20-26°C, humidity 45-70%
- After turning on, the machine performs a 1-2 minute self-inspection

### Nozzle Selection
- **CN030** (0.3mm): For 0201, 0402 components
- **CN065** (0.65mm): For 0402, 0603, 0805, diodes
- **CN140** (1.4mm): For 1206, 1210, 1812, 2010, SOT23, 5050
- **CN220** (2.2mm): For SOP series ICs, SOT89, SOT223, SOT252
- **CN400** (4mm): For ICs from 5-12mm
- **CN750** (7.5mm): For ICs larger than 12mm
- In the software, you can assign multiple nozzles to a single feeder for flexibility

### Component Size Reference
- 0201: 0.6×0.3mm
- 0402: 1.0×0.5mm
- 0603: 1.6×0.8mm
- 0805: 2.0×1.2mm
- 1206: 3.2×1.6mm
- 1210: 3.2×2.5mm
- 1812: 4.5×3.2mm
- 2010: 5.0×2.5mm
- SMA: 4.0×2.5mm
- SMB: 4.0×3.3mm
- SMC: 6.6×5.6mm
- SOT23: 3×1.4mm
- SOT89: 4.5×2.5mm
- SOT223: 6.4×3.7mm
- TO252: 6.5×6.3mm

## Software Settings and Feeder Configuration

### Feeder Arrangement
- Left Feeders (Feeder 20-48)
- Right Feeders (Feeder 1-19)
- Special Feeders (Feeder 49-98 for trays)
- Regular feeders have a default pick angle of 90° for right-side feeders and -90° for left-side feeders
- Default X-Y feeder positions are pre-configured in the software

### Feeder Configuration Parameters
- **Pick Height**: Distance from maximum extension (smaller values = greater travel)
- **Place Height**: Sets nozzle extension when component is released into solder paste
- **Pick Delay/Place Delay**: Time to wait after contact (100-300ms recommended)
- **Vacuum Detection**: Enables checking if component is present on nozzle
- **Feed Rate**: Distance in mm to advance feeder (2mm for 0201/0402, 4mm for 0603/0805/1206)
- **Feed Strength/Torque**: Adjusts feed strength (default: 50-60)
- **Peel Strength**: Required strength for peeling (default: 80)
- **Move Speed**: Percentage of maximum speed (default: 100, use lower like 70 for testing)
- **Vision Alignment**: Options include Individual, Jointly, or Large Component
- **Threshold Settings**: Vacuum threshold settings for nozzles (default: -40)

### Special Tray Feeders (49-98)
- Require additional settings:
 - **Columns**: Number of components in each column
 - **Rows**: Number of components in each row
 - **Right Top X/Y**: Position of last component (top right) in tray
 - **Start X/Y**: Position of first component (defaults to X1,Y1)

## Operating Procedures

### Height Adjustment (CRITICAL)
- Always use the manual height option for both place and pickup operations
- In the menu, use the slider that can be moved up and down
- Slowly move the slider down until it just touches the surface
- Note the exact numbers at this position
- Avoid using other height adjustment methods as they can cause the nozzle to hit too hard
- For pick height, select appropriate nozzle, align camera over component, and drag slider down until the nozzle touches component, then note the value
- For place height, pick up a component, align over PCB, and slowly lower until component barely touches PCB surface, then note value

### Board Import and Alignment
1. Generate CSV from the neoden4 ULP in Eagle
  - The ULP offers settings for board offset, rotation, and speed override
  - Board offset moves all components by the specified amount in mm
  - Board rotation can compensate for boards not perfectly aligned
  - Move speed can be adjusted globally as a percentage (100% default)
2. Place PCBs in machine where you want them positioned
3. Import CSV to Neoden software (it should auto-detect fiducials)
4. Check "Manual Alignment" if no fiducials on PCB (use positioning holes or reference points)
5. Click "align fiducial", move camera to actual location, click "move", then accept

### Panel Creation
- Create panel from the first component location/fiducial
- Set bottom left and bottom right positions, and specify number of panels
- First entry in CSV must stay as first entry unless you update start/panel positions
- Panelize in the machine, not in Eagle
- For panelized PCBs, set row and column numbers according to positioning
- The ULP can generate configuration for multiple boards with specific spacing

### Running Programs
- Select program from P&P Programming page and press "Mount"
- Check "Vibration Feeder" box if needed
- Two run modes: "Step Mode" (one move at a time) and "Continuous Mode" (runs automatically)
- Step Mode helpful for troubleshooting or verifying program integrity
- The ULP sets "PCB,Manual,Lock,100,100,350,150,Front,10,10,0" by default for manual feed with magnetic fixtures

## Component Placement Workflow

### Tape and Reel Component Installation
1. Lift angled tab on feeder, insert tape so sprocket holes rest on gear tooth
2. Pull cover tape through large feeding slot in feeder, over brass block
3. Allow spring to pull feeder cover over tape
4. Lift silver tab on peeler to separate gears
5. Thread cover tape through slot on angled tab and through peeler gears (must extend all the way through)
6. Allow spring in peeler to close plastic feeding gears

### Tube-Fed Component Installation
1. Install tube feeder guide (polished steel) using Allen screws
2. Screw round horizontal tube guides into plate
3. Adjust width of black vibration feeder guides for each tube
4. For small components, use tape to cover front slots between guides

### First-Time Setup and Testing
1. Perform a dry run without components before production
2. Test the program to pick and place a few components
3. Inspect boards for proper component specification, direction, polarity, and position
4. Adjust programming file based on placement results
5. For components off in the same direction, check fiducial issues
6. For individual components off, adjust their coordinates using downward-looking camera

### Inspection Guidelines
- Check component specification, direction, and polarity against design
- Inspect for component damage or pin distortion
- Verify placement accuracy is within tolerance
- Use amplifier, microscope, or AOI equipment for small-pitch components
- Follow IPC Standard and SJ/T10670-1995 SMT General Technical Requirements

## Maintenance and Troubleshooting

### Daily Maintenance
- Check temperature (20-26°C) and humidity (45-70%)
- Maintain fresh air environment without corrosive elements
- Ensure no obstacles on rails or working area
- Keep camera lens clean
- Check nozzles for dirt or deformation
- Verify feeder installation and cleanliness
- Check air hose connections

### Common Issues & Troubleshooting

#### Pick Failure
- Nozzle wear, aging, or cracks causing leakage - replace nozzle
- Nozzle bottom surface uneven or dirty - clean with appropriate tools
- Incorrect nozzle size for component - select proper nozzle
- Component surface uneven - replace with qualified component
- Component sticking to bottom tape - check tape quality
- Burrs on tape holes sticking to component - check tape quality
- Component pins stuck in corner of den - adjust pick position
- Insufficient gap between component and tape package hole - check manually
- Plastic adhesive tape too sticky or flimsy - replace tape
- Pick position offset - reprogram X, Y, Z data

#### Placement Offset
- Programming issue - amend coordinates or modify fiducials if all components offset
- Component thickness setting errors - modify Footprint Library information
- Pick height too high causing drops - reset Pick Height value
- Pick height too low causing sliding - adjust Pick Height
- Speed too high affecting rotation accuracy - lower speed setting

#### Rails System Issues
- Rails belt loose or broken - adjust or replace
- Rails drive motor not working - check connections or repair
- PCB not advancing - check belt tension and motor operation

## Reference Resources
- NullSpaceLabs.com's standard stack of components in the ULP
- Eagle ULP by charliex@nullspacelabs.com (version 1.0j)

## Safety Precautions
- Power off machine before cleaning or repairing
- Ensure machine is properly grounded
- Do not use in noisy environments (e.g., high frequency welding machine)
- Do not use if power supply voltage exceeds rated voltage ±10%
- Disconnect during thunderstorms to prevent electrical component damage
- Keep hands out of working area during operation
- Repairs should be performed by skilled technicians
- Only use parts supplied by NeoDen
- Ensure all bolts and nuts are tightened after maintenance
