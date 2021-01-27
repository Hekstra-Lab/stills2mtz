# stills2mtz
Example using CCTBX to convert DIALS output to mtz with Ewald offsets.


### Installation and Setup
This script requires that you have 
 - [DIALS](https://dials.github.io/)
 - [reciprocalspaceship](https://github.com/hekstra-lab/reciprocalspaceship)
The trick is that `reciprocalspaceship` needs to be accessible from the DIALS python intepreter. 
To accomplish this, first follow the instructions on the [DIALS website](https://dials.github.io/installation.html) to install DIALS.
Once you have DIALS in your path, you can install `reciprocalspaceship` with the following command:

```bash
cctbx.python -m pip install reciprocalspaceship
```
This will make sure that the Python interpreter shipped with DIALS is aware of `reciprocalspaceship`.

### Usage
For this example, we will convert a pair of reflection and experiment files from [CXIDB: 81](https://cxidb.org/id-81.html) to `mtz`. 

```bash
./stills2mtz data/t016_rg013_chunk000_reintegrated_experiments.json data/t016_rg013_chunk000_reintegrated_reflections.pickle -o data.mtz
```

Now there should be a new `data.mtz` file. We can see all the columns by using the `rs.mtzdump` script from `reciprocalspaceship`. 

```bash
$ rs.mtzdump data.mtz
Spacegroup: P6122
Extended Hermann-Mauguin name: P 61 2 2
Unit cell dimensions: 93.239 93.239 130.707 90.000 90.000 120.000

mtz.head():

           BATCH  cartesian_fixed_obs_x  ...  cartesian_delta_z  PARTIAL
H   K  L                                 ...                            
-37 -4 18      0            -0.39638376  ...     -6.3847256e-06    False
-35 -4 25      0            -0.37496334  ...      -9.921834e-06    False
    -3 19      0            -0.37502986  ...      1.8138476e-07    False
    -2 2       0            -0.37495732  ...      4.3450336e-06    False
       3       0            -0.37503228  ...     -2.2314734e-07    False

[5 rows x 20 columns]

mtz.describe():

           BATCH  cartesian_fixed_obs_x  ...  cartesian_delta_y  cartesian_delta_z
count  1.098e+06              1.098e+06  ...          1.098e+06          1.098e+06
mean   1.617e+03             -7.497e-04  ...          1.626e-07         -8.234e-08
std    9.149e+02              1.993e-01  ...          1.490e-04          1.495e-04
min    0.000e+00             -5.810e-01  ...         -1.727e-03         -1.991e-03
25%    8.060e+02             -1.391e-01  ...         -5.915e-05         -3.389e-05
50%    1.664e+03             -1.192e-05  ...          6.831e-08          2.921e-08
75%    2.404e+03              1.290e-01  ...          5.959e-05          3.388e-05
max    3.159e+03              5.905e-01  ...          1.794e-03          2.004e-03

[8 rows x 19 columns]

mtz.dtypes:

BATCH                        Batch
cartesian_fixed_obs_x      MTZReal
cartesian_fixed_obs_y      MTZReal
cartesian_fixed_obs_z      MTZReal
cartesian_fixed_x          MTZReal
cartesian_fixed_y          MTZReal
cartesian_fixed_z          MTZReal
ewald_offset               MTZReal
I                        Intensity
SigI                        Stddev
xcal                       MTZReal
ycal                       MTZReal
xobs                       MTZReal
yobs                       MTZReal
sigxobs                    MTZReal
sigyobs                    MTZReal
cartesian_delta_x          MTZReal
cartesian_delta_y          MTZReal
cartesian_delta_z          MTZReal
PARTIAL                       bool
dtype: object
```

For the example from the `careless` manuscript, we will only require:
 - `I`
 - `SigI`
 - `xobs`
 - `yobs`
 - `ewald_offset`

### Caveats
This is just a simple script to serve as a template for future implementations. 
Don't expect too much out of it.

### Acknowledgments
Special thanks to @phyy-nx for doing an excellent job curating the data from his [paper](https://journals.iucr.org/d/issues/2018/09/00/lp5037/) about using DIALS to refine experimental geometry. 
