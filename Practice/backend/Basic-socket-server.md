# Basic socket server

## Create a tcp socket that listens on port 1111 on local host,

- When a user connects to the socket, the following should happen:

- If the user sends a string containing only the word "exit", the socket and connection should close and the function should end.
- For any other string the user sends, the server should send a copy of the string back to the user.
- you can assume short strings all ending in "\n" other than "exit"

## Solution:
```commandline
   import socket
   import threading

    def socket_server():
        # Создаём TCP-сокет
        server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  # позволяет повторно использовать порт
        server_socket.bind(('127.0.0.1', 1111))
        server_socket.listen(1)
        
        conn, addr = server_socket.accept()
        
        with conn:
            while True:
                data = conn.recv(1024).decode().strip()  # убираем лишние пробелы и \n
                if data == "exit" or not data:
                    break
                conn.sendall(data.encode())  # возвращаем чистую строку без \n
        
        server_socket.close()
    
    # Запуск сервера в отдельном потоке
    server_thread = threading.Thread(target=socket_server, daemon=True)
    server_thread.start()

```

