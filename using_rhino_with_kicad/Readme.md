## Using Rhino and KiCad

See [KiCad Forum Post](https://forum.kicad.info/t/kicad-latest-nightly-doesnt-recognize-colors-of-step-file/4703) 

KiCad does not seem to be as flexible as CircuitMaker about how STEP files are brought in, specifically it likes making a lot of files monochrome. 

This method was tested with Rhino 7 SR5 and KiCad 5.1.9-1

# 3d Models with KiCad

## TL;DR
KiCad really likes closed, watertight shapes in it's step files if you want the colors to apply.  
You also **must** use the `AP214` or newer export formats.

### Opening STEP Files from Suppliers
When opening a STEP file with the intention to send it to KiCad:
- It does not seem to matter if the `Join Surfaces` option is ticked or not.  
- It also does not seem to matter if the `Skip Invisible Geometry` box is ticked or not.  

### Coloring Surfaces
Make sure you use the command `explodeblock` then `all` to fully explode the blocks into polysurfaces.  
When splitting up files to get multiple colors (say; pins vs uc chips) you must use `cap` or similar to create watertight polysurfaces for each color. These closed breps do not need to be booleaned into one object (in fact, if you did, they would have to be one single color).  
If you're not sure if you have a closed brep select an object and type `what` and it will confirm the closed/open status.  
Multiple closed breps in a single STEP file are fine.  

### Exporting
When exporting for KiCad;
- Make sure all surfaces are closed polysurfaces. Then use the `export` command.  
- When selecting a filename use the following settings:
  - (Untick) `Save Small`
  - (Untick) `Save Geometry Only`
  - (Tick) `Save Textures`
  - (Tick) `Save Notes`
  - (Tick) `Save Plugin Data`
- Then select either `AP214AutomotiveDesign` or `AP214AutomotiveDesignCC2` but **not** `AP203ConfigControlDesign`
- `Export Parameter Space Curves` can be in any state
- `Split Closed Surfaces` can be in any state (for reasons I don't fully understand)

Now when opening in KiCad you should see separate colors.

# Exporting SVGs from Rhino -> KiCad

### This doesn't work
Annoyingly, while Rhino 7 and above can generate SVG outputs, they don't seem to have any layer information. This makes a one-stop import into KiCad or Gerboylze really tricky.

