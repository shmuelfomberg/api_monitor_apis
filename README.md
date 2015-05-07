API Monitor's API Files
=======================

The API Monitor is an excellent tool, but unfortunately its API files 
are a not actively maintained. But that's OK - you can do it yourself.

The purpose of this project is to improve and add to its APIs, and 
document how to do it.

Function Parameters
===================

Example: <Param Type="PULONG" Name="DataLength" />

* Type: Data type of the parameter
* Name: How it is called
* Count: For arrays / pointers, how many of this type the array contain
* DerefCount:
* PostCount: Same as Count, but only valid post call. (for out arrays)
* DerefPostCount: 

The value of Count/PostCount can be the name of other parameter.  
PostCount can also be "Return", to refer to the return value of the function.

API Success condition
=====================

API call can also have a success indicator. such as:

<Success Return="NotEqual" Value="0" />

Parameters:

* Return can be: "NotEqual", "Greater", "Equal", 
* Value: Anything to compare the return to.
* ErrorFunc: Seen: "HRESULT", "GetLastError", "errno", "NTSTATUS".
