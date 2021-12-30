# HTTP

- A *request* is from the client side.

  ```zsh
  GET /search?name=abc HTTP/1.1
  Accept: text/html
  ```

  

- A *response* is from the server side.

  ```zsh
  HTTP/1.1 200 OK
  Content-type: text/html
  
  <!DOCTYPE html?
  <html lang="en">
  ...
  </hmtl>
  ```

- Architecture

  ```zsh
  +------------------------------+
  | Server                       |
  +------------------------------+
  |                              |
  | +----------+---------------+ |
  | |TCP Listener              | |
  | +----------+---------------+ |
  |                              |
  |                              |
  | +----------+---------------+ |
  | |HTTP Parser               | |
  | +----------+---------------+ |
  |                              |
  |                              |
  | +----------+---------------+ |
  | |Handler                   | |
  | +----------+---------------+ |
  |                              |
  +------------------------------+
  ```

  

