#!/bin/bash

#use to run from local workstation

for i in [subjects separated by a single space]
do

#creates directories in which to move data that is needed for DTI preprocessing scripts
 mkdir $i
 mkdir $i/T1
 mkdir $i/rsBOLD

#copies needed data from shared individual folder to working directory
#replace $filepath with filepath to data, $T1MPRAGE with T1-weighted structural nifti path and file name, and $rsbold with resting-state bold nifti path and name
 cp $filepath/$i/$T1MPRAGE $i/T1/T1.nii
 cp $filepath/$i/$rsbold $i/rsBOLD/rsBOLD.nii

#reorients T1 MPRAGE to standard space and performs a rough brain extraction
 fsl5.0-fslreorient2std $i/T1/T1.nii T1_reorient.nii 
 fsl5.0-bet $i/T1/T1_reorient.nii $i/T1/T1_brain.nii -f .1 -B -R

#mcflirt motion correction on resting state bold data
 fsl5.0-mcflirt -in $i/rsBOLD/rsBOLD.nii -o FSL/melodics/motion_corr -smooth 8.0 -stats

#registers motion corrected bold data to highres and mni space
 fsl5.0-flirt -in $i/FSL/melodics/motion_corr -ref $i/FSL/T1/T1_brain -applyxfm -usesqform -out $i/FSL/melodics/subj_reg
 fsl5.0-flirt -in $i/FSL/melodics/subj_reg -ref ../../../../../../apps/fsl/5.0.8/fsl/data/standard/MNI152_T1_2mm -applyxfm -usesqform -out $i/FSL/melodics/mni_reg

#performs melodic independent component analysis on registered bold data
 fsl5.0-melodic -i $i/FSL/melodics/mni_reg -o $i/FSL/melodics/ --report

done
