# Calibration of CCD Images

Calibration is a procedure for reducing the effects of CCD Bias and Dark Current (see CCD Basics) and certain defects in the optical system.

## Light Frames

'Light Frames' are images (or a video) of a celestial object captured by the CCD. Each pixel in the images will have a numeric value that includes the influences of:

- Light from the celestial objects in the field of view.
- The various types of noise generated by the CCD.
- Defects in the optical system.
- Cosmic Ray hits

With calibration we will try to remove as much as possible of the Bias and Dark Current from each Light Frame. Also, we will try to remove the effects of certain types of optical defect and noise.

## Dark Frame Subtraction

The objective here is to remove the CCD Bias and Dark Current noise from each Light Frame image. First we build a Master Dark Frame that contains only the Bias and Dark Current elements of an exposure. Then we subtract the Master Dark Frame from each of our Light Frames (or each frame of our video).

Dark Frames are images taken with no light falling on the CCD, so that the only values present in the pixels are due to Bias and Dark Current (+ or - some noise). Typically, Dark Frames are acquired by placing a light-proof dust cap over the telescope objective and making a series of exposures with the same settings (exposure, gain etc) as for the Light Frames to be calibrated.

For still images, several Dark Frames should be captured (say 10) and averaged into a single Master Dark Frame. This will minimise the amount of quantum noise in the Master Dark Frame.

The Master Dark Frame, containing only Bias and Dark Current, should be subtracted from each of the Light Frames effectively removing most of the effects of Bias and Dark Current from the Light Frames.

## Flat Fielding

The objective here is to minimise the effects of various imperfections such as:

- CCD inhomogeneities (pixels more or less sensitive to light)
- Optical vignetting (image is darker away from the centre)
- Dust on the CCD window (seen as 'donut' shaped spots on the image)
- Internal reflections

First we build a Master Flat Field that shows the imperfections, then we divide each Light Frame by the Master Flat Field. 
A Flat Field  is an image of a perfectly plain flat white surface. Any deviation from absolute homogeneity across the image is due to CCD or optical imperfections.

Flat Field images should be obtained using an exposure that gives about 50-75% saturation of the CCD. Everything about the optical set-up (type of projection, focus, filter etc) should be as used for the Light Frames to be calibrated. This is to ensure that the imperfections in the Flat Field images are as near as possible identical to those in the Light Frames.

To minimise quantum noise, build a Master Flat Frame by averaging several flat frame exposures (say 10), or stacking a flat frame video.

In theory Flat Field exposures themselves include Bias and Dark Current so you should build a Master Dark Frame of the same exposure length as your Flat Field images and subtract it from each of your Flat Field images before averaging them. However unless you are trying to resolve really bad flat frame problems this step can usually be bypassed.

Each of the Light Frames (after Dark Frame subtraction) should be divided by the Master Flat Field  to produce calibrated images in which dark-frame and flat-frame defects have been minimised.


**Note** : The relevancy of calibration for our problem statement is still doubtful. We will get to this later. 