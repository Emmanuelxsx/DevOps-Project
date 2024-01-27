# Introduction to Shell Scripting and User Input

## Shell Scripting Syntax Elements

1. **Varible**: Bash allows you to define and work with variables.

Example: Assigning value to a variable: `name="John"` and Retrieving value from a variable `echo $name`

![](images/assigning%20variable.PNG)

2. **Control Flow**: Bash provides control flow statements like if-else, for loops, while loops and case statements to control the flow of execution in your scripts.

Example: Using if-else to execute script based on a condition

copy and paste this shell script in a file

![](images/create%20if-else.sh.PNG)

```
#!/bin/bash

# Example script to check if a number is positive, negative, or zero

read -p "Enter a number: " num

if [ $num -gt 0 ]; then
    echo "The number is positive."
elif [ $num -lt 0 ]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi
```

![](images/if%20else%20excution%20script.PNG)

now, run the script with `./` followed by the file name of the script: `./if-else.sh`
 
 output:
 ![](images/if%20else%20output.PNG)

Example: Iterating through a list using a for loop

copy and paste this shell script in a file

![](images/create%20loop.sh.PNG)

```
#!/bin/bash

# Example script to print numbers from 1 to 5 using a for loop

for (( i=1; i<=5; i++ ))
do
    echo $i
done

```
![](images/loop.sh%20excution%20script.PNG)

now, run the script with `./` followed by the file name of the script: `./loop.sh`

output
![](images/loop.sh%20output.PNG)

3. **Command Substitution**: Command Substitution allows you to capture the output of a command and use it as a value within your script.

Example: Using backtick for command substitution using the command ``current_date=`date +%Y-%m-%d``

![](images/using%20Backtick.PNG)

Example: Using `$()` syntax for command substitution using the command `current_date=$(date +%Y-%m-%d)`

![](images/using%20$().PNG)

4. **Input and Output**: Bash provides various ways to handle input and output. The various ways are demonstarted in the examples below.

Example: Accept user input using the command:
`echo "Enter your name:"
read name`

![](images/enter%20your%20name%20script.PNG)

output
![](images/user-input%20output.PNG)

Example: output text to the terminal using `echo "Hello World"`

![](images/hello%20world%20output.PNG)

Example: Output the result of a command into a file using `echo "hello world" > index.txt`

![Alt text](<images/outputing result of a command to a file.PNG>)

Content of the index.txt file
![Alt text](<images/content of index.txt file.PNG>)

Example: Pass the content of a file as input to a command using `grep "pattern" < input.txt`

![Alt text](<images/grep pattern.PNG>)

Example: Pass the result of a command as input to another command using `echo "hello world" | grep "pattern"`

![Alt text](<images/grep pattern hello.PNG>)

5. **Functions**: Bash allows you to define and use functions to group related commands together.

copy and paste this shell script in a file

```
#!/bin/bash

# Define a function to greet the user
greet() {
    echo "Hello, $1! Nice to meet you."
}

# Call the greet function and pass the name as an argument
greet "John"
```
![Alt text](<images/function shell script.PNG>)

output:
![Alt text](<images/function output.PNG>)

# Shell Script Documentation

**step 1**: On terminal, open a folder called shell-scripting using `mkdir shell-scripting`.


**step 2**: create a file called user-input.sh using the command `touch user-input.sh`

![Alt text](<images/creating user-input.sh.PNG>)
**step 3**: Inside the file copy and paste the block of code below:

```
#!/bin/bash

# Prompt the user for their name
echo "Enter your name:"
read name

# Display a greeting with the entered name
echo "Hello, $name! Nice to meet you."
```
![Alt text](<images/user-input shell script.PNG>)

**NOTE**: `#!/bin/bash` helps specify the type of bash interpreter to be used to execute the script

**step 4**: save your file

**step 5**: Run the command `chmod +x user-input.sh`, this makes the file executable

**step 6**: Run the script using the command `./user-input.sh`

![Alt text](<images/user-input output.PNG>)

# Directory Manipulation and Navigation

**step 1**: open a file named navigating-linux-filesystem.sh

![Alt text](<images/opening navigating-linux-filesystem.sh file.PNG>)

**step 2**: paste the code block below into the file

```
#!/bin/bash

# Display current directory
echo "Current directory: $PWD"

# Create a new directory
echo "Creating a new directory..."
mkdir my_directory
echo "New directory created."

# Change to the new directory
echo "Changing to the new directory..."
cd my_directory
echo "Current directory: $PWD"

# Create some files
echo "Creating files..."
touch file1.txt
touch file2.txt
echo "Files created."

# List the files in the current directory
echo "Files in the current directory:"
ls

# Move one level up
echo "Moving one level up..."
cd ..
echo "Current directory: $PWD"

# Remove the new directory and its contents
echo "Removing the new directory..."
rm -rf my_directory
echo "Directory removed."

# List the files in the current directory again
echo "Files in the current directory:"
ls
```
![Alt text](<images/navigating linux directory shell script.PNG>)

**step 3**: Run the command `sudo chmod +x navigating-linux-filesystem.sh` to set execute permission on the file

**step 4**: Run the script using this command `./navigating-linux-filesystem.sh`

![Alt text](<images/navigating linux directory output.PNG>)

# File Operations and Sorting

**step 1**: Open terminal and create a file called sorting.sh using the command `touch sorting.sh`

![Alt text](<images/creating sorting.sh.PNG>)

**step 2**: Copy and paste the code block below into the file

```
#!/bin/bash

# Create three files
echo "Creating files..."
echo "This is file3." > file3.txt
echo "This is file1." > file1.txt
echo "This is file2." > file2.txt
echo "Files created."

# Display the files in their current order
echo "Files in their current order:"
ls

# Sort the files alphabetically
echo "Sorting files alphabetically..."
ls | sort > sorted_files.txt
echo "Files sorted."

# Display the sorted files
echo "Sorted files:"
cat sorted_files.txt

# Remove the original files
echo "Removing original files..."
rm file1.txt file2.txt file3.txt
echo "Original files removed."

# Rename the sorted file to a more descriptive name
echo "Renaming sorted file..."
mv sorted_files.txt sorted_files_sorted_alphabetically.txt
echo "File renamed."

# Display the final sorted file
echo "Final sorted file:"
cat sorted_files_sorted_alphabetically.txt
```
![Alt text](<images/sorting.sh shell script.PNG>)

**step 3**: Set execute permission on sorting.sh using the command `sudo chmod +x sorting.sh`

**step 4**: RUn the script using the command `./sorting.sh`

![Alt text](<images/sorting.sh output.PNG>)

# Working with Numbers and Calculations

**step 1**: On the terminal create a file called calculation.sh using the command `touch calculations.sh`

![Alt text](<images/creating calculations.sh file.PNG>)

**step 2**: Copy and paste the code block below:

```
#!/bin/bash

# Define two variables with numeric values
num1=10
num2=5

# Perform basic arithmetic operations
sum=$((num1 + num2))
difference=$((num1 - num2))
product=$((num1 * num2))
quotient=$((num1 / num2))
remainder=$((num1 % num2))

# Display the results
echo "Number 1: $num1"
echo "Number 2: $num2"
echo "Sum: $sum"
echo "Difference: $difference"
echo "Product: $product"
echo "Quotient: $quotient"
echo "Remainder: $remainder"

# Perform some more complex calculations
power_of_2=$((num1 ** 2))
square_root=$(echo "sqrt($num2)" | bc)

# Display the results
echo "Number 1 raised to the power of 2: $power_of_2"
echo "Square root of number 2: $square_root"
```

![Alt text](<images/calculations.sh shell script.PNG>)

**step 3**:  Set execute permission on sorting.sh using the command `sudo chmod +x calculations.sh`

**step 4**: Run the script using this command: `/calculations.sh`

![Alt text](<images/calculations.sh output on terminal.PNG>)

# File Backup and Timestamping

**step 1**: on your terminal open a file backup.sh using the command `touch backup.sh`

![Alt text](<images/creating backup.sh.PNG>)

**step 2**: Copy and paste the code block into a file.

```
#!/bin/bash

# Define the source directory and backup directory
source_dir="/path/to/source_directory"
backup_dir="/path/to/backup_directory"

# Create a timestamp with the current date and time
timestamp=$(date +"%Y%m%d%H%M%S")

# Create a backup directory with the timestamp
backup_dir_with_timestamp="$backup_dir/backup_$timestamp"

# Create the backup directory
mkdir -p "$backup_dir_with_timestamp"

# Copy all files from the source directory to the backup directory
cp -r "$source_dir"/* "$backup_dir_with_timestamp"

# Display a message indicating the backup process is complete
echo "Backup completed. Files copied to: $backup_dir_with_timestamp"
```

![Alt text](<images/back up shell script.PNG>)

**step 3**: Set execute permission on sorting.sh using the command `chmod +x backup.sh`


**step 4**: Run the script using the command: `./backup.sh`

![Alt text](<images/backup.sh output.PNG>)



 



