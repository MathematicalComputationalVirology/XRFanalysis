# General rules for using BoXFP

BoXFP utilises dye size sets (DSSs) included in the capillary electrophoresis (CE) data to determine nucleotide (nt) position and spacing within the CE traces.
Generally speaking there are 3 different modes for DSS use in the BoXFP package. 

1. The entire DSS is used
2. Part of the DSS is used
3. Extrapolation is used to calculate the reactivity profile beyond the DSS


## Using the entire DSS
The default setting for the BoXFP functions has the full DSS used. For instance the DSS ROX-400HD contains 21 different DNA fragments ranging in size from 50 to 400 nts.
By default BoXFP functions will utilise all of the 21 peaks generated by the DNA fragments, resulting in a profile of 350 nts generated.

## Using a subset of the DSS
However the entire DSS cannot and should not be used in all cases, particularily if an exit peak is located within the DSS peaks. This occcurs if the start position of the primer used
less than the the size of the largest DNA fragment in the DSS (in the case of ROX-400HD this would mean a primer start position <400 nts into the sequence under investigation).
Furthermore, due to the size of the exit peak, it is advised that the DSS peak set is not used unless the primer start position is at least 10 nts greater than the size of the largest DNA
fragment in the DSS (for ROX-400HD this would be a primer start position >410 nts). 

Due to the effects that the intense exit peak has on preprocessing (see [preprocessing tutorial](Tutorial/Data_preprocessing.md)), using the reduced DDS peak set should be considered during 
proprocessing and reactivity calculations. In the wrapper function `RX_preprocess`, a subset of the DSS peakset can be specified using the `Top` argument (if you only want to use the first 
4 peaks in the peak set `Top` should be set to 4). The `end` should also be set to a value below the location of the exit peak to better facilitate DSS peak location. 

During the position determination and reactivity calculation steps the reduced subset of DSS peaks must also be specified. in the reactivity calculation wrapper function `RX_analyse`
the `sm_cutoff` argument when assigned a negative value indicates that a smaller subset is required (setting `sm_cutoff` to -4 indicates that the first 4 DSS peaks are to be used). 

In the position determination wrapper algorithm `RX_position` the reduced subset does not necessarily need to be calcutated however the `clip` argument should be set to the number of nucleotides
in the reduced size marker set (the first 4 DSS peaks cover 50 nts, therefore the `clip` argument should ideally be set to 50)


## Using an extended version of the DSS