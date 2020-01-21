---
title: "Determining Optimal Parameters"
teaching: 35
exercises: 10
questions:
- "How do you determine the optimial parameters for a dataset?"
objectives:
- "Understand how to optimize parameters for each dataset"
keypoints:
- "Optiminal parameters can be determined by running the `localparam.m` script."
- "Once optiminal parameters are determined, these values can be input into the batch configuration file `c01`."
---

## Optimizing Parameters

Prior to running the data through the pipeline, optiminal parameters can be determined by running the `localParam.m` script on a loaded data file. This script is a version of the `s01` pipeline script that is designed to be run locally. Running the script locally enables you to quickly determine the affect of parameter edits and find optiminal values. 

This script will load the EEG channel data, component data, and component topographies which should all be used to determine appropriate parameters. The channel EEG data should be investigated to ensure that appropriate decisions were made regarding removing channels. Channels that are artefactual for most of the file should be marked for removal. These channels can later be interpolated from neighbouring channels. Channels that are only artefactual for short periods of time or channels that contain eyeblinks should remain in the data. The artefacts in these channels (including the eyeblinks) can be isolated into a component then can then be marked for removal. Decisions about channels **can not** be edited during the QC procedure and as such, parameters will have to be edited in order to change channel decisions. The component and channel data should be used to determine if accurate decisions are made about time. Decisions about time can be edited by the reviewer during the QC procedure, however, it is still important to ensure that the pipeline is accurately marking out time. If a staging script was used to mark in-task and out-task periods, these should also be confirmed to be accurate. Time periods that have ben marked out-task by the staging script will be manually marked at the beginning of `s01` and these time periods will not have undergone the ICA. The topographies are helpful in determining if the montage transformation used was correct. The transformation that was determined and input into `c01` warps the data to the standard MNI montage that is used for average rereferencing during the pipeline. If the transformation matrix did not rotate the head correctly, the topographies will be plotted in the incorrect direction on the head. The ICLabel classifier uses the topography to classify each component. If the topographies are not plotted in the correct direction, the classifier will not be able to accurately identify componentes. This is most obvious when looking at eye components. Eye componets contain eyeblinks, which are easily identified in the component data. One way to ensure the topographies are plotted correctly, is to identify the eyeblink component in the data and look at the topography for that component. The topography activation should be strongest at the front of the head over the eyes. If the eye component's activation is strongest at the sides or back of the head it is likely that the data was not correctly warped to the standard montage during the pipeline. In this case, the [co-registration](https://bucanl.github.io/SDC-LOSSLESS/04-coregister/index.html) should be redone to determine the correct matrix that can then be input into `c01`. 

1. Open MATLAB and navigate to your **local** project directory to make it your current path.

2. Open EEGLAB by typing the following into the command window:
 
    ```matlab
    >> addpath derivatives/BIDS-Lossless-EEG/code/install
    >> lossless_path
    >> eeglab
    ``` 

3. If you are using an older version of MATLAB (pre-2014b), you will need to set the default figure renderer to OpenGL by typing the following into the command window:

    ```matlab
    >> set(0,'DefaultFigureRenderer','OpenGL');
    ``` 

4. In the EEGLAB drop-down menu, navigate to **Import data-> Using EEGLAB functions and plugins-> From BIDS subject folder**. In the file chooser navigate into the `sub-001/eeg` directory, select the `sub-001_task-faceFO_eeg.set` file and press open. 

5. Once the file has loaded, run the localParam.m script by typing the following into the command window:

    ```matlab
    >> localParam
    ``` 

6. The script will display 'Done!' in the command window when it has completed. The script will have made decisions about the data using the default parameters in the `localParam.m` script. To visualize the decisions that have been made, navigate in the EEGLAB drop-down menu to **Edit** and press **Visually edit in scroll plot.**

7. In the **Select vidual editing parameters** pop-up window, type `60` in the `Y axis spacing [spacing]` field and `40` in the `window tine length [winlength]` field. The window should look like this:

    ![Visually Edit in Scroll Plot Winodw]({{ page.root }}/fig/visedit_localparam.png "Visually Edit in Scroll Plot Window for local params")

8. Press enter after you finish inputting the values to ensure that they have been saved. Then press the `Ok` button. This will laod a window containing the **EEG channel** data.

9. The script has added annotations to the file that show the decisions the pipeline has made. These decisions are based off of the parameters. First look at the decisions that have been made about channels. Channels that have been marked artefactual are shown in gray and have a gray `manual` flag and a coloured flag on the left side of the data window. The colour of the flag indicates the reason the channel has been indentified as artefactual and there is a legend for the marks at the top left above the data window. In this example, there are several channels that have been found to be bridged and these channels are indicated with a green flag. Bridged channels have consistently high correlation coefficients to neighbouring channels. It is important to identify bridged channels prior to running the independent component analysis. 

Next steps:
- add staging script and scroll through the ensure that the staging script is accurately marking in and out task time periods
    - maybe not do staging script here
- Turn decision criteria figures on to see these figures 
- Change the parameter values and look at how these changes affect the decision criteria figures and the decisions about channels/time by looking at the file

*** Steps for between script runs 

1. close the scroll plot 
2. eeglab window- file > clear study/clear all
3. type clear in the command window to clear all the variables from the workspace
4. reload the file


## Investigating Decision Making Criteria

1. open in text
2. turn figures for time decisions on. recommend turning on the figure one at a time for channel and time
3. describe what each figure is


{% include links.md %}

---
