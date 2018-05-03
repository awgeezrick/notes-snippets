# R BASICS

Below are my personal R notes, taken while learning R. These notes outline the basics of R and R syntax. More advanced R notes will be added once I have an opportunity to consolidate additional notes into this document.

__NOTE:__ I am still reformatting these notes with Markdown. Therefore they are currently difficult to read and may contain errors.

## Assigning Variables
```py
var_name <- “var value”
```
## Checking the class of a variable
```py
class(var_name)
```
## CLASS TYPES

1. Numerics
  - Decimal values
1. Integers
  - Natural numbers - Integers are also numerics
1. Character
  - strings / text
1. Logical
  - TRUE FALSE Booleans

## VECTORS

One dimensional lists

```py
# To create a vector

vec_name <- c(val_1, val_2, val_3)

# The c() function is required to ‘combine’ values

# To give names to the values in vector

names(vec_name) <- c(“name_1”, "name_2", “name_3”)

# Vectors can be added

vec_one + vec_two

# This produces a new vector in which each value is added to the corresponding value in the other vector

# Select a specific index in a vector

vec_name[i]

# This selects the vector index number ‘i’

# In R, indexes start at 1, not 0

# To return multiple indexes from a single vector, use the c( ) function

vec_name[ c(i, j, k) ]

# OR

vec_name[ i:k ]

# ...this starts at number i and includes up to (and including) number
# It is also possible to select vector indices by using the corresponding index names

vec_name[ c( “name_1”, ”name_2”, ”name_3” ) ]

# Perform mathematical operations on a single vector
# examples...

sum( vec_name )
mean( vec_name )
```

## LOGICAL COMPARISON OPERATORS
    * &   for and
    * |   for or
    * !  for not
    * < for less than
    * > for greater than
    * <= for less than or equal to
    * >= for greater than or equal to
    * == for equal to each other
    * != not equal to each other
* Logical booleans can be passed into vector selection brackets to select only the TRUE list items
    * vec_name[ bool_list ]
* MATRICES
    *  Creating a matrix - 1-9, numbered by row, and 3 rows total
        * matrix(1:9, byrow = TRUE, nrow = 3)
    * Similarly, you can combine multiple vectors into a single matrix
        * matrix( c( vec_1, vec_2, vec_3 ), byrow = TRUE, nrow = 3)
    * To add names to either rows or columns
        * rownames(my_matrix) <- row_names_vector
        * colnames(my_matrix) <- col_names_vector
    * Or, to add names when creating matrix
        * matrix_name <- matrix(vectors, nrow = 3, byrow = TRUE, dimnames = list(c(“row_name1", "row_name2", "row_name3"), c("US", "non-US")))
    * To sum each row or column of a matrix
        * rowSums(matrix_name)
        * colSums(matrix_name)
    * To merge matrices and/or vectors together by column (adds a column or multiple columns to a matrix)
        * big_matrix <- cbind( matrix1, matrix2, vector1 …)
    * To merge matrices and/or vectors together by row (adds a row or multiple rows to a matrix)
        * big_matrix <- rbind( matrix1, matrix2, vector1 …)
    * Selecting multiple elements from a matrix
        * my_matrix[1,2]
            * selects the element at the first row and second column.
        * my_matrix[1:3,2:4]
            * results in a matrix with the data on the rows 1, 2, 3 and columns 2, 3, 4.
        * my_matrix[,1]
            * selects all elements of the first column.
        * my_matrix[1,]
            * selects all elements of the first row.
    * standard operators like +, -, /, *, etc. work in an element-wise way on matrices
    * %*% is an operator that performs a standard matrix multiplication
* ls( ) function
    * “ls" and “objects" return a vector of character strings giving the names of the objects in the specified environment. When invoked with no argument at the top level prompt, ls shows what data sets and functions a user has defined. When invoked with no argument inside a function, ls returns the names of the function's local variables: this is useful in conjunction with browser.
* FACTORS
    * The term factor refers to a statistical data type used to store categorical variables.
        * The difference between a categorical variable and a continuous variable is that a categorical variable can belong to a limited number of categories.
        * A continuous variable, on the other hand, can correspond to an infinite number of values.
    * To create a new factor variable
        1. Create a vector containing all of the factor levels
            * vec_name <- c( “value_1”, “value_2”, “value_3” )
        2. Use the factor( ) function to assign the levels to a new factor
            * factor_name <- factor( vec_name )
    * Nominal Categorical Variable
        * A nominal variable is a categorical variable without an implied order.
    * Ordinal Categorical Variable
        * Ordinal variables have a natural ordering
        * To create one:
            * factor_vec_name <- factor(vec_name, order = TRUE, levels = c(“Level_1", “Level_2", “Level_3”))
    * To Change Factor Level Names, use the levels( ) function
        * levels(factor_vec_name) <- c(“Level_1”,"Level_2”)
            * Be certain to write new level names in the order that R presents them (alphabetically if nominal)
    * summary( ) function
        * summary( ) gives you a quick overview of the contents of a variable. The levels and the number of instances for each level
* DATAFRAMES
    * head( ) or tail( ) functions show the first few or last few rows (and headers) of a dataframe
    * str( )
        * str() shows you the structure of your data set. For a data frame it tells you:
            * The total number of observations (e.g. 32 car types)
            * The total number of variables (e.g. 11 car features)
            * A full list of the variables names (e.g. mpg, cyl ... )
            * The data type of each variable (e.g. num)
            * The first observations
    * data.frame( )
        * this is used to combine vectors into a single dataframe
        * df_name <- data.frame( vec1, vec2, vec3, vec4 )
    * Selecting elements of a dataframe
        * my_df[1,2]
            * selects the value at the first row and select element in my_df.
        * my_df[1:3,2:4]
            * selects rows 1, 2, 3 and columns 2, 3, 4 in my_df.
        * Variable names can also be used to select elements
            * my_df[1:3,”variable_name”]
        * To select an entire column, if the columns have names, a shortcut is
            * my_df$col_name
                * This is similar to typing
                    * my_df[ ,”col_name” ]
        * Booleans can be used to select rows for which the value is TRUE
            * my_df[ boolean_vector, cols_to_return]
        * subset( ) also allows you to select a subset of rows based on some condition
            * subset(my_df, subset = some_condition)
        * SORTING
            * order( vec_name )
                * this gives the ranked position of each element when applied to a variable
            * vec_name[ order( vec_name ) ]
                * This gives a reshuffled output of the vector
            * This can be used to sort a dataframe
                * order_list <- order[ my_df$var_name ]
                * my_df[ order_list, ]
* LISTS
    * my_list <- list(comp1, comp2 …)
        * The list ( ) function takes in the list of components to be contained in the resulting list
    * my_list <- list(name1 = your_comp1,name2 = your_comp2)
        * This names each component of the list
        * If the list is already created, the name ( ) function can be used to set the names
            * my_list <- list(your_comp1, your_comp2)
            * names(my_list) <- c("name1", "name2")
    * A list in R allows you to gather a variety of objects under one name (that is, the name of the list) in an ordered way. These objects can be matrices, vectors, data frames, even other lists, etc. It is not even required that these objects are related to each other in any way.
    * You could say that a list is some kind super data type: you can store practically any piece of information in it!
    * To select a component of a list is either with the position or name of that component (all three of these options work
        1. my_list[ [ # ] ]
        2. my_list[ [ “comp_name” ] ]
        3. my_list$comp_name
    * To select an element of a component in a list…
        * my_list[ [ “comp_name” ] ][ # ]
        * my_list$comp_name[ # ]
    * To extend/add to an existing list use the c( ) function
        * new_list_name <- c( existing_list_name, new_comp_name = new_comp)

R DATA INPUT & OUTPUT

* CSV Input & Output
    * Assign new csv to a dataframe
        * df <- read.csv( ‘file path or file name if in current directory’ )
    * Then to write to csv
        * write.csv(df, file = ‘title.csv’)
* EXCEL files with R
    * Install package ‘readxl’
        * library(readxl)
        * # Call names of sheets located in Excel file
excel_sheets('Sample-Sales-Data.xlsx')
        * # Set desired sheet
df <- read_excel('Sample-Sales-Data.xlsx', sheet = "Sheet1" )
        * # import entire workbook into R using the lapply() function
entire.workbook <- lapply(excel_sheets('Sample-Sales-Data.xlsx'),
              read_excel, path = 'Sample-Sales-Data.xlsx’)
    * To write to Excel install package ‘xlsx'
        * # To writ to an excel file
install.packages('xlsx')
        * # Load library to current session
library(xlsx)
        * # Write Excel file
write.xlsx(df,'Excel_output_example_mtcars.xlsx’)
* SQL with R
    * General strategy for figuring out how to connect R to a SQL database
        * Typically a specialized package for each kind of SQL engine
        * Most are built on top of RODBC package
        * Sample Syntax
            * install.packages(‘RODBC’)
library(RODBC)

myconn <- odbcConnect( ‘Database_Name’, uid = ‘User_ID’, pwd = ‘password’ )
dat <- sqlFetch( myconn, ’Table_Name’ )
querydat <- sqlQuery( myconn, ’SELECT * FROM table’ )

close( myconn )
* WEBSCRAPING with R
    * Install package rvest
        * install.packages('rvest')
library(rvest)
    * Then to view demo examples of how to use rvest
        * demo( package = 'rvest')
demo(package = 'rvest', topic = ‘specific_demo_name’)

PROGRAMMING in R

* IF, ELSE IF, ELSE
    * syntax
        * if ( x==10 ){
    print(‘x is equal to 10’)
} else if ( x == 12 ){
    print(‘x is equal to 12’)
} else {
    print(‘x is not equal to 12 or 10’)
}
* WHILE Statements
    * syntax
        * while (x<10){
    print ( paste0( ‘x is: ‘ ,x) )
    x <- x+1
    if(x==10){
        print( ‘x is now equal to 10! Break Loop!’ )
    }
}
    * Break
        * this command breaks or terminates the while loop
        * This can be a simple break statement or a conditional break
* FOR Loops
    * syntax
        * v <- c(1,2,3,4,5)
        * for (temp.var in v){
    result <- temp.var +1
    print(paste0('The temp.var plus 1 is equal to: ',result))
}
    * Nested FOR loops
        * mat <- matrix(1:25,nrow=5)
        * for(row in 1:nrow(mat)){
    for(col in 1:ncol(mat)) {
        print(paste('The element at row:', row,'and col:',col, 'is', mat[row,col]))
    }
}
* FUNCTIONS
    * syntax
        * name_of_function <- function(input1, input2, input3 = DEFAULT.Value){
    result_var <- # Code to execute
    return(result_var)
}
    * To call function
        * name_of_function(input1, input2, input3 = value)
    * Be careful not to overwrite any default functions such as sum( ), etc.
    * SCOPE
        * if a variable is defined only within a function, then its scope is limited to that function
        * reassignment of a global variable within a function will not affect anything outside the function

ADVANCED PROGRAMMING IN R

* BUILT IN R FUNCTION
    * seq() Function
        * generates a sequence from n to i with step k
        * seq( n, i, by = k )
    * sort() Function
        * sorts a series of values, ascending by default
            * capital and lower case letters are treated equally
        * sort( vec_name, decreasing = FALSE)
    * rev() function
        * reverses values in an object
        * rev(vec_name)
    * str() function
        * Tells us the structure of an object
        * str(obj_name)
    * summary() function
        * Gives us a statistical summary of an object, particularly useful for summarizing columns of a df
        * summary(df_name)
    * append() function
        * allows us to add to an object, works of vectors and lists
        * append(vec_name, additional_data)
    * CHECKING A DATATYPE
        * is.datatype(object)
            * is. array(object), is.vector(object), is.data.frame(object), etc.
        * See dropdown to see all ‘is.’ options
    * CONVERT DATATYPE
        * as.datatype(object) similar to is.
    * sample() function
        * Sample a random value from an object
            * sample( object, #.of.sampled.values )
* APPLY FUNCTIONS
    * lapply(vec, function)
        * applies function to each element in a vector or list and returns a list of results
    * sapply(vec, function)
        * applies function to each element in a vector or list and returns a vector of results
    * Adding in additional parameters when using Apply functions
        * sapply(vec, function,param2_name = value)
* ANONYMOUS FUNCTIONS
    * Allows you to create a function without having to name it. It is similar to Lambda functions in Python.
    * example
        * v <- 1:15
        * result <- sapply(v, function(num){num*2})
        * print(result)
    * this example is equivalent to:
        * times2 <- function(num){
    return(num*2)
}
* MATH FUNCTIONS
    * abs( ) function
        * returns the absolute value
    * round( ) function
        * used to round values
        * round(number, digits = #.past.decimal)
    * CHEAT SHEET WITH R MATH FUNCTIONS:
        * https://cran.r-project.org/doc/contrib/Short-refcard.pdf
* REGULAR EXPRESSIONS
    * grep (General Regular Expressions)
    * grepl( ) function
        * returns logical expression
            * grepl( term.or.pattern.I.am.searching.for, where.I.want.to.search  )
    * grep
        * returns an index
* DATES AND TIMESTAMPS
    * Date Information
        * Sys.Date( )
        * [1] "2017-08-15"
        * today <- Sys.Date( )
        * Converting non standard date strings to date type
            * as.Date(“Nov-03-90”, format = “%b-%d-%y)
        * Codes for conversion based on non-standard format
            * %d - day of month (decimal number)
            * %m - month (decimal number)
            * %b - month (abbreviated)
            * %B - month (full name)
            * %y - year (2 digit)
            * %Y - year (4 digit)
    * Time Information
        * POSIXct or POSIXlt
            * Portable Operating System Interface
            * as.POSIXct( )  converts strings to time stamps
            * as.POSIXct(“11:02:03”, format=“%H:%M:%S”)
                * will add a date and timezone to the result
        * strptime( ) is another method to use to convert to timestamps
            * this is more of our standard tool, not POSIX
        * help(strptime) will give all of the needed codes for conversion

DATA MANIPULATION WITH R

* Packages and Pipe Operator
    * Dplyr - Manipulates Data
    * Tidyr - Cleans data
    * %>% used to further simplify syntax
* DPLYR Package
    * Example functions in dplyr
        * filter( )
            * filter(flights,month==11,day==3,carrier=='AA')
            * Far simpler than similar subset called using bracket notation
        * slice( )
            * slice just returns rows by position
            * slice(flights,1:10) # returns fist 10 rows
        * arrange( )
            * works similar to filter but reorders rows
            * arrange(flights,year, month, day,desc(arr_time)
                * That code orders records based first on year, then month, then etc., except arr_time which is descending
        * select( )
            * Allows you to select subset of columns
            * select(flights, carrier, arr_time)
        * rename ( )
            * Allows us to quickyl rename columns
            * rename(flights, airline_carrier = carrier)
                * renames carrier column to airline_carrier
        * distinct( )
            * Allows us to select distinct values in column
            * distinct(flights, carrier)
        * mutate( )
            * Allows us to add new columns that are functions of other columns
            * mutate(flights, new_col = arr_delay - dep_delay)
                * new_col is resulting calculation
        * transmute( )
            * transmute() is same as mutate () but returns only the new column you created
        * summarise( )
            * aggregate function of all the data
            * summarise(flights, avg_air_time = mean(air_time,na.rm=TRUE))
                * na.rm = TRUE removes na values in data
        * sample_n( )
            * return n random rows
            * sample_n(flights,10)
        * sample_frac( )
            * returns a sample of rows, based on a fraction of the total rows
            * sample_frac(flights,0.1)
* PIPE OPERATOR %>%
    * Complex nesting functions is difficult to read, because they need to be read from the inside out
        * e.g. result <- arrange(sample_n(filter(df,mpg>20), size=5), desc=(mpg))
    * Pipe operator passes each function, starting with the data source and passing results from one operation to the next
        * Result <- df %>% filter(mpg>20) %>% sample_n(size = 5) %>% arrange(desc=(mpg))
* DATA.TABLE package
    * extends data.frame capabilities
    * data.table syntax is very similar to data.frame
    * much more efficient based on copying of values
TIDYR
* gather( )
    * collapses (gathers) multiple columns into one
        * gather(df, new_col_to_gather_col_names_as_keys, new_col_to_gather_values, cols_to_gather)
        * df %>% gather(new_col_to_gather_col_names_as_keys, new_col_to_gather_values, cols_to_gather)
* spread( )
    * the compliment of gather, un-gathers data and spreads it across multiple columns
        * spread(df, key_col_to_spread, value_col_to_spread)
* spread and gather are analogous to pivot tables in Excel
* separate( )
    * turns single character column into multiple columns
    * Default separator will try to separate non alpha-numberic characters
        * separate( df, target.col, c(new.col1, new.col2) )
    * To specify separator
        * separate( df, target.col, c(new.col1, new.col2), sep = ‘-‘ )
* unite( )
    * the compliment to separate, combines separate character columns into one
        * unite( df, new.col, target.col1, target.col2, sep = ‘-‘
GGPLOT2
*  Separates graphics into 3 layers (all three needed to produce a visualization)
    1. Data
    2. Aesthetics
    3. Geometries
* Next 3 layers allow to really customize visualizations
    1. Facets
    2. Statistics
    3. Coordinates
* Final layer is
    1. Theme
* Sample code
    * pl1 <- ggplot(df1, aes(x=factor(cyl),y=mpg))
print(pl1 + geom_boxplot(aes(fill=factor(cyl))) + coord_flip() +theme_dark())
