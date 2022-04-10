
# Creators
  Aleksei Marianov and Alena Kochubei
# Integrator Shimadzu

The program Integrator_1_spectrum is written for a quick automatic integration of multiple spectra/chromatograms with similar positions of the peaks. In case of gas chromatography it usually means that chromatograms are obtained using the same program and on the same day, i.e. a batch or sequence mode, to prevent significant drift of the peaks.

![How the window of the program looks like.](./Figure%20of%20the%20working%20software.JPG "Window of the software")

## General description

In current configuration, Integrator_1_spectrum takes a path to a folder as an input. In this folder, it indicates all the txt files and extracts the x and y values of the curves. Next, it slices the curves to extract the required range and performs the calculations for the peaks, baseline, integration boundaries and integrals. After pushing the button "Review plots", it displays all the calculated data as 8 figures: 6 figures with the slices containing peaks for txt files and 2 figure with the integrals of the corresponding 3 peaks. After pushing the button "Export xlsx", it exports integrals into an .xlsx file stored in the location indicated in the corresponding entry.

In terms of the code, Integrator_Shimadzu contains 3 parts: functions for the class Peakfinder, class Peakfinder and a software created using Gui object and Tk.

* Functions for spectrum slices, baseline, peaks, integration boundaries, integrals are used for each peak separately (6 times in current configuration). To make the program shorter, they were placed at the beginning of the file.
  
* Instance of a class Peakfinder computes and stores all the values necessary for drawing the figures and integrals calculation. 

* The Gui object software creates a window in which there are entries for the arguments necessary for an instance of Peakfinder class to be created.
   
## Installation

1. The software is fully compiled into .exe file that can be run on any computer provided a person can grant a permission for it.

2. The integrator_1_spectrum can be run from Pycharm or VSCode if you have Python and all the required modules(numpy, matplotlib, pandas, scipy, os, importlib, peakutils, shutil, typing, tkinter, time) installed on your computer. 


## Input files

As the input files, there should be used txt files with two column for x and y numerical data. NOTE: _get_curves functions at the beginning is designed to trim the beginning and end lines which might contain non-numerical data.

## Input files from Shimadzu LabSolutions software

In the current configuration, the Integrator_1_spectrum was tested and is working for the txt files created following the algorithm below:
   1. Labsolutions software(The software was installed for Shimadzu GC_2014) -> Data -> Postrun(part that allows manual integration) -> open the folder with your data
   2. Choose all the files you want to integrate -> press right button on the mouse -> File Conversion -> Convert to ASCII format
   3. Choose all the files you want to integrate again -> File -> Extract -> extract the info to txt ticking only Chromatogram pictogram for the info that should be extracted.

## Customization

* ### Different input files
  
In case of ASCII files from LabSolutions software, there are lines containing non-numerical information at the beginning and the end of txt. If you have other type of txt file(see Input files.), change skip_header and skip_footer arguments in the lines 89 and 90 to suit your txt file.

```python
vals = np.genfromtxt(path_in, usecols=(1), skip_header=106, skip_footer=4)
times = np.genfromtxt(path_in, usecols=(0), skip_header=106, skip_footer=4)
```

* ### Different initial settings for the peaks

In the lines between 606 and 800, find the name of the setting. Go to the name_of_the_setting.insert() line. This is the third line in the example below. Change the second number of two(13.55 in this case). It will permanently change the number in the window.

```python
sig6_small_peak_pos = Entry(frame)
sig6_small_peak_pos.grid(row=28, column=4, sticky=E)
sig6_small_peak_pos.insert(0, 13.55)
```
* ### Different names of parameters in the window:

In the lines from 802 to 863, find the line which has the name of the label in argument text (text=""). You can change the name "Signal 6" to the chemical compound in the first line or the current x units (min) if you have other units. It is not recommended to delete the text like "left margin", "right margin" etc since they indicate what is the parameter for any user in the future.

```python
Label(frame, text="Signal 6 name:").grid(row=22, column=2, sticky="w")
Label(frame, text="Left margin (min):").grid(row=23, column=2, sticky="w")
Label(frame, text="Right margin (min):").grid(row=24, column=2, sticky="w")
Label(frame, text="Baseline polynomial order:").grid(row=25, column=2, sticky="w")
Label(frame, text="Peak half-width, (min):").grid(row=26, column=2, sticky="w")
Label(frame, text="Peak prominence:").grid(row=27, column=2, sticky="w")
Label(frame, text="Peak distortion:").grid(row=28, column=2, sticky="w")
Label(frame, text="Small peak position, min:").grid(row=29, column=2, sticky="w")
```
  
Note: The changes in the text argument will not affect the names of the initial settings for the peaks in python file (see. Different initial settings for the peaks)


## Current issues

* ### Speed of calculations

Currently the calculations for 50 chromatograms takes about 7 sec which is slow enough to notice. However, the program works fine even though it displays "Not responding" message on it. We expect to fix this issue in the following updates.

* ### The number of x-axis on integrals are displayed on the plot next to it

Currently the program does not change the big numbers into the scientific format (1*10^6). We expect to fix this issue in the following updates.

* ### The integrals values are different from the ones I get from the manual integration - Integration boundaries & Baseline

1) Make sure that left_margin and right_margin are tailored to fit the integration boundaries and peak as precise as possible.
2) Make sure that integration_boundaries defined by peak_half_width and peak_distortion include the peak only and, as close as possible, nothing else.
3) The Integrator_1_spectrum can not precisely match the integrals defined manually. 
In Integrator_1_spectrum baseline is calculated automatically and represents a smooth, not always linear curve (displayed as dashed lines on the Figures). Since the method of baseline definition is different compared to majority of softwares for integration i.e. linear between two points defined by the user, the exact values of integrals are not going to match. 
Currently, the differences were found to be about 1000 for the integrals in a range from 500,000 to 700,000 and less than 100 for the integral in a range from 1500 to 2000. The authors are working on it at the moment. 

* ###  Not all the peaks are integrated - Integration of small peaks

When signal/noise ratio is less than 2/1, the simulated baseline considers the peak as a part of a baseline and, subsequently, the integrals can not be calculated by this software. Usually, it might become an issue for small peaks with integrals < 5. Those peaks should be integrated manually in any suitable software and manually fixed in the xlsx file. 

## References

For your questions and suggestions please send an e-mail to alenakochubei2104@gmail.com with 'Integrator' typed in the subject.

The permanent changes described above can be performed free of charge after the request.

The permanent changes beyond the scope described above can be negotiated with creators. E-mail: alenakochubei2104@gmail.com

## Acknowledgements
