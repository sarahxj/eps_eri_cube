# eps_eri_cube

This repository contains the data cube (an [xarray](https://xarray.dev/) `DataArray`) used in Jiang et al. 2023 _Revisiting ε Eridani with NEID: 
Identifying New Activity-Sensitive Lines in a Young K Dwarf Star_ (in press). The cube contains all line parameters (centroid, depth, FWHM, and integrated 
flux) for each line in the compiled line list over 32 NEID observations of ε Eridani, as well as the measured RV and activity indices for each 
observation. For information on how the line parameters are measured, see the paper.

### Structure of cube

The cube has three dimensions, or "coordinates," as follows:

__The first coordinate__ is named "param" and is of length 40. Each index in the "param" coordinate corresponds to one of the following parameters:
centroid, depth, FWHM, integrated flux, RV, or the various activity indices. In total, there are 20 parameters; the error on each parameter is listed
directly after the parameter itself, for a total length of 40. The following graph shows the index that corresponds to each label:

0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11
--- | --- | --- | --- |--- |--- |--- |--- |--- |--- |--- |---
centroid | centroid_err | depth | depth_err | fwhm | fwhm_err | int_flux | int_flux_err | rv | rv_err | cahk | cahk_err 

12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23
--- | --- | --- | --- |--- |--- |--- |--- |--- |--- |--- |---
hei_1 | hei_1_err | hei_2 | hei_2_err | nai | nai_err | ha06_1 | ha06_1_err | ha06_2 | ha06_2_err | ha16_1 | ha16_1_err 

24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35
--- | --- | --- | --- |--- |--- |--- |--- |--- |--- |--- |---
ha16_2 | ha16_2_err | cai_1 | cai_1_err | cai_2 | cai_2_err | cairt1 | cairt1_err | cairt2 | cairt2_err | cairt3 | cairt3_err 

36 | 37 | 38 | 39 
--- | --- | --- | ---
nainir | nainir_err | padelta | padelta_err

Each of these parameters can be accessed using the corresponding index 
(see [Accessing data in cube](https://github.com/sarahxj/eps_eri_cube/new/main?readme=1#accessing-data-in-cube)).
For more information on what each label means, see the 
[NEID documentation](https://neid.ipac.caltech.edu/docs/NEID-DRP/algorithms.html#activity-index-definition).

__The second coordinate__ is named "line" and is of length 11572. This coordinate contains the wavelength values of each line in the compiled line list.

__The third coordinate__ is named "time" and is of length 32. This coordinate contains the JD timestamps for each of the 32 NEID observations of ε Eridani.

Each of these coordinates can be accessed with `cube['coordinate_name']`. For example, `cube['line']` prints out the full list of 11572 lines. Note that
this method of accessing the coordinate values returns a `DataArray` object; to obtain the coordinate values as an array, use 
`cube.coords['line'].values`. Also, `cube['param']` will not return the values of any parameters, rather the list of labels ('centroid', 'centroid_err',
'depth', 'depth_err', etc.). To access the _values_ of any parameters, you must first specify at least one line and observation.

### Accessing data in cube

In order to access individual data values in the cube, it's as easy as `cube[1,2,3]`, where each index corresponds to the index of interest in each of the
three coordinates mentioned in [Structure of cube](https://github.com/sarahxj/eps_eri_cube/new/main?readme=1#structure-of-cube). 
For example, `cube[6,10,20]` returns the integrated flux of the 10th line for the 20th observation.

Time series of a particular parameter can be obtained by replacing the 'time' index with ':', or otherwise by using the `DataArray.isel` method. The 
following two statements: `cube[6,10,:]` and `cube.isel(param=6,line=10)` will both return the integrated flux of the 10th line over all 32 observations.
You can then easily plot these time series, along with the errors (in this example, by using `cube[7,10,:]` or `cube.isel(param=7,line=10)`). 
Similarly, you can access all parameters of a particular line in one observation by using `cube[:,10,20]`, or one parameter over all lines in one 
observation by using `cube[6,:,20]`, and so on.

For more information on different techniques for accessing the data in the cube, consult the 
[xarray DataArray documentation](https://docs.xarray.dev/en/stable/generated/xarray.DataArray.html).

-----

*Updated: October 2022.*
