# Shell

## Grep

- Print search results:

  - Definition:

    ```bash
    grep {search-string} {filename}
    ```

  - Example:

    ```bash
    grep username logfile0001.log
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

- Search in multiple files:

    - Definition:


      ```bash
      # Method 1: Pass all the file names
      grep {search-string} {filename1} {filename2}
      
      # Method 2: You may use a double dash with no name, it means no more flags can be defined
      grep {search-string} -- {filename1} {filename2}
      ```

  - Example:

      ```bash
      # Method 1
      grep username logfile0001.log logfile0002.log

      # Method 2
      grep username -- logfile0001.log logfile0002.log
      ```

## Sed

- Normally sed is invoked like this:

  - Definition:

    ```bash
    sed {script} {filename}
    ```

  - Example:

    ```bash
    # Method 0: to replace all occurrences of ‘hello’ to ‘world’ in the file input.txt and print the changes
    sed 's/hello/world/' input.txt

    # Method 1: to replace all occurrences of ‘hello’ to ‘world’ in the file input.txt to output.txt
    sed 's/hello/world/' input.txt > output.txt

    # Method 2: To edit files in place 
    sed -i 's/hello/world/' file.txt
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

  