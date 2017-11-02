# VBA - Notes and Useful Commands
----------------------
This document contains my consolidated notes for writing code in Microsoft's Visual Basic for Applications (VBA). Note: My focus primarily on VBA use for automating Excel workbooks, so you will find little if anything here related to other Microsoft applications

__USER BEWARE: I am currently consolidating these notes from various personal note sources. They are not yet formatted, nor have errors been corrected.__

__In its current state, I give this document a 0 points out of 10 for usefulness, accuracy, or readability.__

# Contents
- [Basics](#basics)
-

# Basics
Option + fn + F11
* Opens VBA Editor
View —> Properties
Insert —> Module
* The module is where we write our code
Insert —> Prodecure
* Creates a procedure or a sub-procedure within our Module
* Any code we input in this procedure goes within the bookends of the procedure stubs

Programming concepts
* Variables
    * dim is reservce keyword for declaring a variable
    * variable types
        * Integer
            * range is only up to 32,000
        * Long
            * Therefore we should use the “Long” variable type
                * ~ -2.147B to 2.147B
        * Double
            * “Double” is variable type needed for decimals, otherwise, long and integer round up
        * Strings
            * Can be manipulated with Right, Left, Mid
                * Right(text, 4)
                * Left(text, 4)
                * Mid(text, 4, 3) - start at fourth position and return three characters
            * Len - returns the length of the string as an integer
            * InStr - returns the numeric position of whatever string elements you search for
                * e.g. InStr(text, “abc”)
* Setting a constant variable
    * To set the value of a variable that will remain constant throughout module’s VBA code (for instance, to make code more readable and more easily updated)
        * Const VAR_NAME As Long = 5
    * This is set at top of Module, before 1st Sub
    * Best Practice is to use ALL CAPS in constant variable name so that they are clear throughout code
* Val Function
    * If it comes across a cell with a string, it returns a zero (0) as an integer
* Range Property
    * When writing target cells, we can us variables embedded into the range location for Row (but not for Column)
        * .Range(“A9”) is the same as .Range(“A” & 9)
            * Therefore, 9 can be replaced with a variable that calls a specific row number
* Cells Property
    * Can only take one cell, unlike range
    * Takes row first and then column
    * .Cells(row#, column#) e.g. .Cells(2,3)
    * A benefits of cells is that we can use variables to call BOTH row AND column e.g. .Cells(var1, var2)
    * We use cells when we don’t know our columns in advance
* Embed Cells property into Range property to access a range of cells using the Cells property
    * With cnSheet
     .Range(.Cells(1,1),.Cells(2,2)) = 67
End With
    * This example writes 67 to the upper right four cells in worksheet with codename “cnSheet”
* Offset Property
    * Offset is a property of Range and is an offset from the original position of Range
    * .Range(“B2").Offset(0,2)
        * counts 0 rows and 2 columns from the cell B2
        * can use negative to count up or left
* RandBetween function
    * This gives random value between two numbers we give
    * .RandBetween(1,12)
* Formatting Cells
    * One of the attributes of Range is Font
        * Setting bold text
            * .Range(“B3:B6”).Font.Bold = True
        * Change color of text to Blue
            * .Range(“B3:B6”).Font.Color = rgbBlue
    * Set fill color of cell to brown
        * .Range(“B3:B6”).Interior.Color = rgbBrown
    * To perform multiple formatting changes to a single .Range
        * With .Range(“B3:B6”)
    .Font.Bold = True
    .Interior.Color = rgbBrown
    .Borders.Weight = xlThick
End With
    * Custom formats, we can copy what’s shown in the custom formats dialogue menu
        * .Range(“B3:B6”).NumberFormat=“$#,##0.00”
    * Setting a border around a range (not each individual cell)
        * .Range(“B3:B6”).BorderAround Weight:=xlMedium
        * This has many attributes that can be chosen and set
* Debugging
    * Step through steps
    * Break Points in code
        * Temporary breakpoints
            * select line in margin
            * or put cursor on line and add breakpoint
        * Permanent breakpoints
            * by adding the “Stop” keyword to a line, we create a permanent break point
    * Immediate window
        * debug.print prints to immediate window
        * can ask immediate window questions by typing in it with code such as
            * ?ThisWorkbook.Worksheets.Count
        * Can also execute single lines of code such as
            * ThisWorkbook.Worksheets.Add
    * Locals Window - shows current variables and values
    * Watch Window
        * expressions can be dragged to the watch window, which then shows us the current value of each expression
        * Quick Watch button can also show pop-up with similar info
        * If you drag a complex object such as a worksheet to the Watch window, it will provide a dropdown of all sub and sub-sub properties/values
            * So, for worksheet “shRead”, if you modify Expression in Watch window to say “shRead.Parent.Name”, it will tell you the value (i.e. Name) for the workbook in which shRead is contained
        * Also possible to add break in code when a selected variable changes
            * this can be achieved in the context menu when setting a watch
    * Call Stack
        * shows current breakpoint in function and also context location in which the function was called
* Conditions
* If Then
    * Else
    * ElseIf
* Select Case statements
    * Works similar to If then, but allows you to add many conditions easily without extra code
* Cmd+Shift+I (F8 on Windows)
    * This steps you through the procedure, line by line

## Loops
```vbnet
For ... Next
    Dim i As Long
For i = 10 To 2 Step -2
     Debug. Print i
Next i

' An "Exit For” exits the For sub, useful
'   in case of embedded If statements in the
'   For loop

' For loops are useful when we know how many
'   times we want the loop to run

For ... Each
        * We always get items in the same order
            * Order cannot be changed
    * Do Loop
        * Be careful not to create a loop that will never end
        * Useful when we don’t know what starting value is, or how long the loop will need to go
        * Dim i As Long
Do
     i = InputBox(“Please enter a number”)
Loop Until i>5
        * Do Until ... Loop
            * subtle difference with Until at start of loop vs end is that in Loop Until the loop will run at least once. In Do Until, it is possible that we will never enter the loop depending on the condition.
        * Do ... Loop While
        * Do While ... Loop
        * While ... Wend
            * Older loop left in VBA for backwards compatibility
            * Exactly the same as Do While loop, but not as flexible or structured - use Do While instead
        *
    * do while
        * Ex 1
            * Do While ActiveCell.Value <> “"            
                * ActiveCell.Font.Bold = True           
                * ActiveCell.Offset(1, 0).Select              
            * Loop
        * Ex 2
            * Do While ActiveCell.Value <> “"            
                * If ActiveCell.Value > 10 Then                
                    * ActiveCell.Font.Bold = True                
                    * ActiveCell.Interior.Color = RGB(0, 0, 255)            
                * End If                        
                * ActiveCell.Offset(1, 0).Select               
            * Loop
```

## Syntax Errors

- VBA knows when we leave out required text
- Syntax Check only checks current line and whether syntax is right

### Compiler Errors

- Relates to more than one line
- “Option Explicit” at the top of our Module if we want VBA to require that we explicitly declare our variables
    - This can be set at default in “Tools” —> “Options”

### Runtime Errors

- Errors that only occur at Runtime
- “Type Mismatch”
    - Typically wrong type of value in spreadsheet or code
- Runtime errors come in all shapes and sizes and we need to often validate results to catch them

### FUNCTIONS

- ByRef vs. ByVal when passing variables
    - ByRef changes value of variable, even once leaving function
    - ByVal does not change the value of the original variable, and returns to the original value after leaving the functions
        - Therefore, ByVal is the better option, as it prevetns you from inadvertently changing a variable value when not in use by the function
- Optional Parameters
    - Normal parameters come first
    - Optional parameters establish defaults if we pass no new value into the sub
    - With multiple parameters, use parameter name and := to set the value so it is clear whick one we are setting

### ARRAYS
`For i = LBound(Name) to UBound(Name)`

Debug.Print
* Allows us to debug code, which prints results of code to the “Immediate Window” [accessible via “View” menu in VBA editor]

### Accessing Workbooks
```vbnet
' Avoid...
ActiveWorkbook

Workbooks(Index)
' ...will always access workbooks in order opened

' Use...
ThisWorkbook

Workbooks(“NameOfSpecificWorkbook.xlsx”)

Accessing Worksheets
* Avoid:
    * ActiveSheet
    * Can access via index such as Worksheets(n) and is indexed based on order of worksheets
        * Depends on user not changing order of sheets
* Better:
    * Worksheets(“SpecificWorksheetName”)
* Best:
    * Specify specific workbook before specific worksheet
    * ThisWorkbook.Worksheets(“Name”)
* Also Best:
    * Use a codename for each worksheet
        * This allows you to streamline your code by calling the codename directly without needing to enter a Worksheets variable and it protects against a user changing the visible name of the worksheet
    * First set the worksheet codename in the properties window of the VBA editor (1st instance of name vs the second one which represents the visible name of the worksheet)
    * For example if codename of worksheet is cnReportData
        * cnReportData.Range(“B4”) = 342
* Also Best:
    * “With” statement allows us to only mention the codename for an object once, for example:
        * With cnReportData
            * .Range(“B4”) = 342
            * .Range(“B5”) = 455
            * .Range(“B6”) = 667
            * .Range(“B7”) = 873
        * End With
```


DECLARE VARIABLES FOR REPEATED FUNCTIONS OR CALLED WORKBOOKS TO MAKE EDITING EASIER (e.g. if the workbook title changes, you only need to change it in one place in your code)
*     Dim wkRead As Workbook        
    * Set wkRead = Workbooks(“SampleWorkbookTitle.xlsx”)        
    * Debug.Print wkRead.Path    
    * Debug.Print wkRead.Name    
    * Debug.Print wkRead.FullName    
    * Debug.Print wkRead.Worksheets.Count    

```vbnet
Range (“A3:F3”) .Select
With Selection.Interior
    .Pattern = xlSolid
    .PatternColorIndex = xlAutomatic
    .ThemeColor = xlThemeColorAccent6
    .TintAndShade = -0.249988111117893
.PatternTintAndShade = 0
End With
With Selection.Font
    .ThemeColor = xlThemeColorDark1
    .TintAndShade = 0
End With
Selection.Font.Bold = True
Rand
```
