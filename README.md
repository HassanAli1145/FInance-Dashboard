# Finance Management-Dashboard

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiYTc4Y2M4YzUtYTUzYy00MjkzLWJjMDMtMGNlMTQzZDQ2MzZjIiwidCI6ImZhNDYzMGI5LTY1YjEtNDY1ZC05ZDcxLTJkNmY5Y2I4NWE4YiIsImMiOjl9

## Problem Statement

This dashboard helps us to find out expenses saving tagets and income details to keep on track and maintain the finances.This dashboard will helps us to find the 
contribution of all the aspects in the system.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors.
- Step 5 : Firstly we will make the table by unpivoting the columns. 
- Step 6 : After this we will close and apply the Power Query editor.
- Step 7 : For seperate sections record maintaining we willmake some measures for savings income and expense i.e. 
DAX Expression for Total Value:
	
		TotalValue = SUM('Dataset'[Value])
For Filtering Income,Expenses,Target and Savings we will make there filters by DAX Expressions.
DAX Expression for Income:
			
			Income = CALCULATE([TotalValue],'Dataset'[Type] = "Income")
DAX Expression for Expense:
			
			Income = CALCULATE([TotalValue],'Dataset'[Type] = "Expense")
DAX Expression for Savings:
			
			Income = CALCULATE([TotalValue],'Dataset'[Type] = "Savings")
DAX Expression for Target:
	
	Income = CALCULATE(SUM('Dataset'[Value]),'Dataset'[Type] = "Target")
Savings,Expense Percentages:
	
	Savings% = DIVIDE([Savings],[Income])
	
	Expense% = DIVIDE([Expense],[Income])
- Step 8 : We will make two columns in the data set i.e. Year and Month Year and for them the Dax Expression is.
	
	
		Year = FORMAT('Dataset'[Date],"yyyy")
		MonthYear = FORMAT('Dataset'[Date],"mmm-yy")


- Step 9 : We will make four Cards i.e. for Savings Saving% Expenses and Income.
- Step 10 : We will also make two pie charts One for Expenses and other for Savings.
- Step 11 : Then we build the slicer of Year for all charts and cards.
- Step 12 : Now we will make measures for last month income and month over month percentage
DAX Expressions
	
		IncomeLM = CALCULATE([Income],DATEADD('Dataset'[Date],-1,MONTH))
		Income Change MOM % = DIVIDE([Income],[IncomeLM])
- Step 13 : Now we will make a line selection table and for its dynamic switching we will make a mesure for its switching.
TABLE DAX Expression
	
			Line_Selection_Table = DATATABLE("Type",STRING,"No",INTEGER,
		{{"Savings",2},{"Expenses",3},{"Target",4},{"Income",1}}) 
Dynamic Switching DAX Measure
	
	Line_Chart_Measures = var Selected_var = SELECTEDVALUE(Line_Selection_Table[No])
	RETURN SWITCH(Selected_var,1,[Income Change MOM %],2,[Savings%],3,[Expenses%],4,[Target])

- Step 14 : We will also make a seperate summarize table for month and year for monthly growth.Then we will make measures for monthly sales , previous month sales and monthly growth.
Expression:
	
	
		Selected_Date = SUMMARIZE('Dataset','Dataset'[Date],'Dataset'[MonthYear],'Dataset'[Year])
DAX Expression for Monthly Sales:
	
	
		MonthlySales = var CYSelectedValue = SELECTEDVALUE(Selected_Date[MonthYear])
		RETURN 
		CALCULATE([TotalValue],'Dataset'[MonthYear] = CYSelectedValue)  
DAX Expression for Previous Month Sales:

		
	
		PYmonth = var previousm = PREVIOUSMONTH(Selected_Date[Date])
		RETURN
		CALCULATE([TotalValue],'Dataset'[Date] = previousm)
DAX Expression for Monthly Growth:
	
	monthlyGrowth = DIVIDE([MonthlySales]-[PYmonth],[PYmonth])

- Step 15 :Now from monthly growth we will calculate expense,savings and income monthly growth
DAX Expression
	
		ExpenseGrowth = var val = (CALCULATE([monthlyGrowth],'Dataset'[Type] = "Expense"))
		RETURN
		if(val = BLANK(),0,val)

	
		IncomeGrowth = var val = (CALCULATE([monthlyGrowth],'Dataset'[Type] = "Income"))
		RETURN
		if(val = BLANK(),0,val)

		SavingGrowth = var val = (CALCULATE([monthlyGrowth],'Dataset'[Type] = "Savings"))
		RETURN
		if(val = BLANK(),0,val)

- Step 16 : We will add another slicer month year for these growth percentages.
- Step 17 : We will add a refresh button to reset all the filters.
- Step 18 : Publish the report by clicking on the button.


<img width="595" alt="Capture" src="https://github.com/user-attachments/assets/5c0114be-2de0-46e7-bef2-e2dd41e97d87">

