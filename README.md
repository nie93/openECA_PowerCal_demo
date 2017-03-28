# openECA Analytic Development Example -- Power Calculation


There are four primary steps in the basic analytic devolopment in openECA:
* Data Strutures Management
* Input/Output Data Mapping
* C# Project Generation
* Analytics Development in Algorithm.cs

In this demo, we are going to extract the voltage and current measurement 
data of positive sequence, including the magnitudes and the angles 
information and calculate the active power and reactive power using C#.

Run **openECA Client** from the Start Menu, then it will automatically 
open a UI host terminal at the background and your default web browser 
as well (make sure your browser is compatible with HTML5, Chrome is 
fine!) and as shown below. The UI host terminal monitors the connection 
and report instantly if any errors occur.

## Step 1: Data Strutures Management
Click on the **Manage Data Structures** button on the left of your openECA 
Client webpage. 

In the input data structure, we have to create four measurements (*fields*) 
to store the data extracted from the openECA, including 
* VoltMag (*double*)  -- voltage magnitude (positive sequence)
* VoltAng (*double*)  -- voltage angle (positive sequence)
* CurrMag (*double*)  -- current magnitude (positive sequence)
* CurrAng (*double*)  -- current angle (positive sequence)

Click on **Save** to save all the measurements for the input data structure. 

Besides, we also need one placeholder for the output data structure, 
* OutputMeas (*double*)  -- placeholder for the output data

which has no effect on the algorithm but it is required for openECA's project 
generation.

Click on **Save** to save the placeholder for the output data structure. 

Return to **Home** page.

## Step 2: Input/Output Data Mapping
Click on the **Manage Input Data Mappings** button on the left of your 
openECA Client webpage. 

### *InputMapping*
Field | Selection | Identifier
------------ | ------------- | -------------
VoltMag | Test Device ABB-521 500 kV Bus 1 Positive Sequence Voltage Magnitude | PPA:5
VoltAng | Test Device ABB-521 500 kV Bus 1 Positive Sequence Voltage Phase Angle | PPA:6
CurrMag | Test Device ABB-521 Dell Positive Sequence Current Magnitude | PPA:11
CurrAng | Test Device ABB-521 Dell Positive Sequence Current Phase Angle | PPA:12

### *OutputMapping*
Field | Selection | Identifier
------------ | ------------- | -------------
OutputMeas | Test Device ------------- | PPA:--

Return to **Home** page. Then continue to click on the **Manage Output Data Mappings** 
button on the left of your openECA Client webpage. 

Return to **Home** page.

## Step 3: C# Project Generation
Click on the **Generate Project** on the left of your openECA Client 
webpage. We are going to generate a C# project with the following configurations.
* Project Name: PowerCalDemo
* File Directory: C:\Users\USERNAME\Documents\openECA Projects\PowerCalDemo (*default*)
* Input Mapping: InputMapping
* Output Mapping: OutputMapping

Click **Generate Project**.

The project folder should be generated in the directory according to the 
configurations, which is "C:\Users\USERNAME\Documents\openECA Projects\"
by default.


## Step 4: Analytics Development Example -- Power Calculation
Locate to the project directory and run the auto-generated solution file 
(PowerCalDemo.sln) in Visual Studio.

Insert the following code after the *TODO* part in Algorithm.cs file.

```cs
string _message = "----------------\n"; // create a string holder for output message

double voltMagV = inputData.VoltMag;
double voltAngV = inputData.VoltAng;
double currMagV = inputData.CurrMag;
double currAngV = inputData.CurrAng;

double activePowerV = 3 * voltMagV * currMagV * Math.Cos( ( voltAngV - currAngV ) * Math.PI/180);
double reactivePowerV = 3 * voltMagV * currMagV * Math.Sin( ( voltAngV - currAngV ) * Math.PI/180);

_message += String.Format("Active Power: {0}\n", activePowerV);
_message += String.Format("Reactive Power: {0}\n", reactivePowerV);
MainWindow.WriteMessage(_message);

```

Finally, click on the **Start** button to test this application. 
