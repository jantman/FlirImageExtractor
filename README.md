# Flir Image Extractor

FLIR® thermal cameras like the FLIR ONE® include both a thermal and a visual light camera.
The latter is used to enhance the thermal image using an edge detector.

The resulting image is saved as a jpg image but both the original visual image and the raw thermal sensor data are embedded in the jpg metadata.

This small Python tool/library allows to extract the original photo and thermal sensor values converted to temperatures, as well as creating side-by-side thermal and visual images.

## NOTE ABOUT THIS FORK

This is an absolutely minimal change to get this working, since I'll probably only use it once. There's a lot in this code that could be improved.

## Requirements

This tool relies on `exiftool`. It should be available in most Linux distributions (e.g. as `perl-image-exiftool` in Arch Linux or `libimage-exiftool-perl` in Debian and Ubuntu). Installing that dependency is outside of the scope of this README, but should be done using your OS'es package manager.

It also needs the Python packages *numpy* and *matplotlib* (the latter only if used interactively).

```bash
python -mvenv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Usage

This module can be used by importing it:

```python
import flir_image_extractor
fir = flir_image_extractor.FlirImageExtractor('examples/ax8.jpg')
fir.process_image()
fir.plot()
```

Or by calling it as a script:

```bash
python flir_image_extractor.py -p -i 'examples/zenmuse_xtr.jpg'
```

```bash
usage: flir_image_extractor.py [-h] -i INPUT [-p] [-exif EXIFTOOL]
                               [-csv EXTRACTCSV] [-d]

Extract and visualize Flir Image data

optional arguments:
  -h, --help            show this help message and exit
  -i INPUT, --input INPUT
                        Input image. Ex. img.jpg
  -p, --plot            Generate a plot using matplotlib
  -exif EXIFTOOL, --exiftool EXIFTOOL
                        Custom path to exiftool
  -csv EXTRACTCSV, --extractcsv EXTRACTCSV
                        Export the thermal data per pixel encoded as csv file
  -d, --debug           Set the debug flag
```

This command will show an interactive plot of the thermal image using matplotlib and create two image files *flir_example_thermal.png* and *flir_example_rgb_image.jpg*. 
Both are RGB images, the original temperature array is available using the `get_thermal_np` or `export_thermal_to_csv` functions.

The functions `get_rgb_np` and `get_thermal_np` yield numpy arrays and can be called from your own script after importing this lib.

## Supported/Tested cameras:

- Flir One (thermal + RGB)
- Xenmuse XTR (thermal + thumbnail, set the subject distance to 1 meter)
- AX8 (thermal + RGB)

Other cameras might need some small tweaks (the embedded raw data can be in multiple image formats)

## Credits

Raw value to temperature conversion is ported from this R package: https://github.com/gtatters/Thermimage/blob/master/R/raw2temp.R
Original Python code from: https://github.com/Nervengift/read_thermal.py
