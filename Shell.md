# Shell

## Basics

- Copy the output of a command to clipboard (only for macos):

  - Definition:

    ```bash
    {command-with-output} | pbcopy
    ```

  - Example:

    ```bash
    cat input.txt | pbcopy
    ```

- Calculate the number of lines of an ouput:

  - Definition:

    ```bash
    # you can pipe the output to word count with the -l flag for number of lines
    {some-command} | wc -l
    ```

  - Usage:

    ```bash
    grep "user" logfile0001.log | wc -l
    ```

## Grep

- Print search results:

  - Definition:

    ```bash
    # Method 0: Searching for a string inside a file
    grep {search-string} {filename}

    # Method 1: Pass all the file names
    grep {search-string} {filename1} {filename2}
      
    # Method 2: You may use a double dash with no name, it means no more flags can be defined
    grep {search-string} -- {filename1} {filename2}
    ```

  - Example:

    ```bash
    # Method 0
    grep username logfile0001.log

    # Method 1
    grep username logfile0001.log logfile0002.log

    # Method 2
    grep username -- logfile0001.log logfile0002.log
    ```

- Copy the contents to a file:

  - Definition:

    ```bash
    grep {search-string} {filename} >{new_filename}
    ```

  - Example:

    ```bash
    grep username logfile0001.log >searched_logs.log
    ```

- Using the regex for digits in grep:

  - Definition:

    ```bash
    # Method 0: You'll have to use the -E flag to activate extended regex and then use the \d+ identifier
    grep -E "some string \d+" {filename}

    # Method 2: Invoke egrep instead of grep
    egrep "some string \d+" {filename}
    ```

  - Usage:

    ```bash
    # Method 0
    grep -E "Writing \d+ lines to the console." logfile0001.log
    
    # Method 1
    egrep "Writing \d+ lines to the console." logfile0001.log
    ```


## Sed

- Normally sed is invoked like this:

  - Definition:

    ```bash
    sed {script} {filename}
    ```

  - Example:

    ```bash
    # Method 0: to replace _first_ occurrence of ‘hello’ to ‘world’ in the file input.txt and print the changes
    sed 's/hello/world/' input.txt

    # Method 1: to replace _all_ occurences, you'll have to specify the g flag at the end of the script
    sed 's/hello/world/g' input.txt

    # Method 2: To paste modified changes of input.txt to output.txt
    sed 's/hello/world/g' input.txt > output.txt

    # Method 3: To edit files in place 
    sed -i 's/hello/world/g' file.txt
    ```

- Creating and using a sed script:

  - Creating the script:

    ```bash
    echo 's/hello/world/' > myscript.sed
    ```

  - Passing the script through sed:

    ```bash
    # Method 1
    sed -f myscript.sed input.txt > output.txt

    # Method 2
    sed --file=myscript.sed input.txt > output.txt
    ```

- Replace the contents of `[]` to something else:

  - Definition:

    ```bash
    # For anything inside the brackets we can use: \[.*\]

    # Content that should be allowed inside brackets:
    #   Match anything that's not a closing bracket: ^]
    #   It may appear 0 or more times: [^]]*
    
    # So the find pattern will be: \[[^]]*\] 
    sed -i "s/\[[^]]*\]/$key/g" {filename}
    ```

  - Usage:

    ```bash
    # As an example it'll replace [23, 48, 51] to [...]
    sed -i "s/\[[^]]*\]/[...]/g" {filename}
    ```


## NVM

- Change Default version of NVM

  ```bash
  nvm alias default v10.24.0
  ```

- Use Default

  ```zsh
  nvm use default
  ```

  