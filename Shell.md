# Unix

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


## Node Management

- Change Default version of NVM

  ```bash
  nvm alias default v10.24.0
  ```

- Use Default

  ```zsh
  nvm use default
  ```

  