# Notes

## Johan's email

### Illustration script (for students, etc.):
```python
"""
The script produces a figure showing the GW peak freq. vs. L_ij angle.
All BHs have the same mass 20Msun and the initial SMA is set to 1 AU.
"""
import numpy as np
import matplotlib.pyplot as plt

open_data_filename          = 'RVA_0_003_01_03_1_2_3_4_5_10_15_25_45_75_90_ISO'
data_all_array              = np.load(open_data_filename + 'savetestarr' + '.npz')['arr_0']
#define:
sec_year        = 31536000.
dc  = 1         #dataset index.
id_arr          = data_all_array[dc, 0, :, 0]          
obij_arr        = data_all_array[dc, 0, :, 1]
tGWij_SI_arr    = data_all_array[dc, 0, :, 2]
fGWij_SI_arr    = data_all_array[dc, 0, :, 4]
ecc_A_arr       = data_all_array[dc, 0, :, 7]   #(if > fGW_limit then = -1)
ecc_B_arr       = data_all_array[dc, 0, :, 8]   #(if > fGW_limit then = -1)
Lij_ang_arr     = data_all_array[dc, 0, :, 9]   #in degrees
pos_LTtlim        = np.where(tGWij_SI_arr < (10**5)*sec_year)
pos_GTtlim        = np.where(tGWij_SI_arr > (10**5)*sec_year)    
#plot:
fig = plt.figure(figsize=(8.0, 8.0))
ax1 = fig.add_subplot(111)
ax1.plot(fGWij_SI_arr[pos_LTtlim], Lij_ang_arr[pos_LTtlim], marker = '.', markersize = 2.0, linewidth = 0, color = 'red')
ax1.plot(fGWij_SI_arr[pos_GTtlim], Lij_ang_arr[pos_GTtlim], marker = '.', markersize = 0.5, linewidth = 0, color = 'black')
ax1.set_xlim(1e-8,1e3)
ax1.set_xscale('log')
#show:
plt.show()    

 ```

### EXPLANATION:
- dc: 
    - This can take values from 0, 15 (1,2,3,4...) 
    - Each value corresponds to an initial inclination between the binary and the incoming single BH. 
    - The value in degree can be seen in the data file name: the first is 0 (co-planar), then 0.003 degrees, where the last is "ISO = isotropic"
    - Therefore, chose dc = 0 for co-planar interactions, etc.

- id_arr: 
    - id = 5 if the BHs merge during the interaction. 
    - id = 3 if BHs merge After the interaction  
    - id >= 10 if the interaction did not end

- obij_arr:
    Tells you which of the two objects that end up in a binary:
    -   1, 2 are the intial binary components
    -   3 is incoming BH
    The encoding shows which pair remains:
    - 1: (1,2) remain
    - 2: (1,3) remain
    - 3: (2,3) remain

- tGWij_SI_arr: inspiral time of resultant binary i,j in seconds.

- fGWij_SI_arr: GW peak frequence of the BBH that merges (i,j)

- ecc_B_arr: eccentricity at 10 Hz. If fGWij_SI_arr > 10 Hz then = -1.

- Lij_ang_arr: Angle between initial and final orbital angular momentum vector.

