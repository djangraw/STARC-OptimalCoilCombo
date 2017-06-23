# STARC-OptimalCoilCombo
Optimize MR coil weightings to maximize temporal stability

Academic publications using this software should cite the following:

Huber, Jangraw et al. (2017) ?Simple Approach to Improve Time Series fMRI Stability: STAbility-Weighted Rf-Coil Combination (STARC).? International Society for Magnetic Resonance in Medicine; Honolulu, Hawaii.

FindBestWeights_Wrapper is the top-level function. For details on how to use it, see the header on this function, reproduced below.
 FindBestWeights_Wrapper(dataDir,nCoils,oddevenall,lastSampleForWeightCalc,smoothingSigma,usepar)

INPUTS:
-dataDir is a string indicating the absolute path to the folder where the
nCoils-channel data is stored. Files inside should be named 0.nii -
<nCoils-1>.nii  [default = './'] 
-nCoils is a scalar indicating the number of coils (and therefore number
of files) in your dataset. [default: 32]
-oddevenall is a string indicating whether the odd samples, even samples,
or all samples should be used when optimizing. [default: 'even']
(NOTE: this uses MATLAB's one-based numbering, so 'even' means the second,
fourth, etc. samples.)
-lastSampleForWeightCalc is a scalar indicating the last sample that
should be used to calculate the weights. If you are using odd samples
only (for example), this will use TRs 1:3:lastIndForWeightCalc. 'all' is
the same as nT, the number of samples in your datasets. [default: 'all']
-smoothingSigma is a scalar or 3-element vector indicating the stddev of
the smoothing kernel to be applied to the data before calculating the
weights IN UNITS OF VOXELS. If smoothingSigma is a vector [a,b,c], an
asymmetric kernel of stddev [a,b,c] in the (XxYxZ) dimensions will be
used. The kernel size will be 2*ceil(smoothingStd)+1. This kernel will
NOT be applied to the reweighted data. [default: 0 (no smoothing)]
-usepar is a binary value indicating whether we should use parallel
processing. It will also accept strings 'true','false','0','1'.
[default: true]

OUTPUTS:
-file <dataDir>/mSTARC_CoilWeights.nii contains an mxnxpxnCoils
matrix, where (mxnxp) is the size of each file in dataDir. The matrix
contains the optimized weights for each voxel and coil. 
-file <dataDir>/mSTARC_CombinedData.nii is an (mxnxp) data brick containing the
data combined across coils using the optimized weights.
-file <dataDir/mSTARC_command.txt is a text file containing the MATLAB
command that could be used to reproduce the output files. Note that some
values are as interpreted by this script, not precisely what was input.