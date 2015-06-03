API Monitor's API Files
=======================

The API Monitor is an excellent tool, but unfortunately its API files 
are a not actively maintained. But that's OK - you can do it yourself.

The purpose of this project is to improve and add to its APIs, and 
document how to do it.

File Structure
==============

Root element - ApiMonitor

	<ApiMonitor>
	</ApiMonitor>

ApiMonitor can contain multiple include statements:

	<Include Filename="Headers\windows.h.xml" />
	
ApiMonitor can also contain multiple module blocks:

	<Module Name="xinput1_1.dll" CallingConvention="STDCALL" ErrorFunc="GetLastError" OnlineHelp="MSDN">
	</Module>

Possible values for CallingConvention: STDCALL, CDECL. (but usually it is STDCALL)  
Possible values for ErrorFunc: Seen: "HRESULT", "GetLastError", "errno", "NTSTATUS"
	
ApiMonitor can contain multiple Interface blocks. An Interface can contain Api blocks.

    <Interface 
		Name="IDirectInput8A" 
		Id="{bf798030-483a-4da2-aa99-5d64ed369700}" 
		BaseInterface="IUnknown" 
		OnlineHelp="MSDN" 
		ErrorFunc="HRESULT" 
		Category="Graphics and Gaming/DirectX Graphics and Gaming/DirectX Input/DirectInput"
	>

Module
======
	
Module can contain multiple API functions: (see the section about Function Parameters)


Module can also contain multiple ModuleAlias statments:

	<ModuleAlias Name="msvcr70.dll" />

Module can conatin multiple Variable blocks / statements. see seperate section.

Module can either have a category attribute, that puts the whole module under this category,
or have multiple Category *statements* dividing the Api block to various categories.

	As attribute:
	<Module 
		Name="Dinput.dll" 
		CallingConvention="STDCALL" 
		OnlineHelp="MSDN" 
		ErrorFunc="HRESULT" 
		Category="Graphics and Gaming/DirectX Graphics and Gaming/DirectX Input/DirectInput"
	>
	
	As statement:
	<Category Name="Visual C++ Run-Time Library/Buffer Manipulation" />

If a module re-export APIs of different DLL, use the SourceModule block. optional attribute Copy="True".

	<SourceModule Name="Advapi32.dll" Include="Windows\Advapi32.xml">
		<Api Name="RegCloseKey" />
		.....
	</SourceModule>
	
API Functions
=============

A simple example:

	<Api Name="XInputEnable">
		<Param Type="BOOL" Name="enable" />
		<Return Type="void" />
	</Api>

Attributes:

* Name: Function name. 
* Ordinal: The ordinal number of the function. Some application load by ordinal instead of by name.
* BothCharset: set to "True" if this API have both A and W versions.

Sub tags:

* Param: function parameter. see section below.
* Return: The return type. 
* Success: determain if the call was success or not. see section below.
	
Function Parameters
===================

Example: <Param Type="PULONG" Name="DataLength" />

* Type: Data type of the parameter
* Name: How it is called
* Count: For arrays / pointers, how many of this type the array contain
* DerefCount:
* PostCount: Same as Count, but only valid post call. (for out arrays)
* DerefPostCount: 
* Display: 
* PostLength: 

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

Variable
========

As much as I can tell, the name attribute is a blind string. there is 
no special meaning to square baracks or astaricks. 

Defining a struct:

	<Variable Name="XINPUT_STATE" Type="Struct">
		<Field Type="DWORD"            Name="dwPacketNumber" />
		<Field Type="XINPUT_GAMEPAD"   Name="Gamepad" />
	</Variable>

Defining a typedef:

	<Variable Name="unsigned"       Type="Alias"    Base="unsigned int" />
	<Variable Name="unsigned*"      Type="Pointer"  Base="unsigned" />
	<Variable Name="wchar_t [260]"  Type="Array"    Base="wchar_t"  Count="260" />

Defining an Enum:

	<Variable Name="EXCEPTION_DISPOSITION" Type="Alias" Base="UINT">
		<Enum>
			<Set Name="ExceptionContinueExecution"      Value="0" />
			<Set Name="ExceptionContinueSearch"         Value="1" />
		</Enum>
	</Variable>

Defining a flag-set variable. apperently values can overlap.

        <Variable Name="[DDSD_FLAGS]" Type="Alias" Base="DWORD">
            <Display Name="DWORD" />
            <Flag>
                <Set Name="DDSD_CAPS"               Value="0x00000001" />
                <Set Name="DDSD_HEIGHT"             Value="0x00000002" />
                <Set Name="DDSD_SRCVBHANDLE"        Value="0x00400000" />
                <Set Name="DDSD_ALL"                Value="0x00fff9ee" />
            </Flag>
        </Variable>
	