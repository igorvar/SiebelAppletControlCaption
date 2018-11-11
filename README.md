# SiebelAppletControlCaption
Often when developing in Siebel requirement to show messages about the Applet control in the code. When this occurs, the problem is to indicate in the message the name of the control as it is displayed on the GUI.
This is especially acute in multilingual projects.
It is proposed to take the field captions from the applet, which is based on BC. To do this, perform the following actions:
1. Creation BC [VIF Repository View Web Template Item], based on [Repository View Web Template Item] with the addition of the [BusComp] field.
2. Creation BO [VIF Fields Caption] with two BCs: [VIF Repository View Web Template Item] and [SRF Applet Controls for Task Admin]
3. Create a BS [VIF Fields Caption BS] with a function that searches in the repository for all the applets that are based on the interesting BC and are used in the interesting view. After that, for each applet found, run through all the GUI elements related to the fields and find the need one.

All this objects are present in the archive AppletControlCaption.sif.
In BS The code calls the GetFieldsCaption function with three parameters: 
- ViewName is the name of the View for which the applet of interest is located 
- BcName is the name of the BC on which this applet is based
- FieldList is Siebel PS, in which the properties with the BC field on the applet will be recorded, the value is Caption for Control and Display Name for List Column.

Notes:
1. For this work correctly, View with name ViewName must be in the repository on the server, BC and the applet is not necessary, the presence in the SRF is sufficient.
2. The search is executed on all applets based on BC in view, therefore if one or several applets contain the same field (even if it is not displayed on the applet), but with different captions, only one of them will be returned, regardless of whether Control is or List Column.
3. A search is executed for applets explicitly prescribed for View, so Toggle Applets will not be counted. To solve the problem, you can use non-displayable elements on applets, for example List Column for Form Applets and Controls for List Applet.
Example of use on BC in a separate file.
