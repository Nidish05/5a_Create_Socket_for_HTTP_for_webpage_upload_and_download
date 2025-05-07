# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
```
import socket
def send_request(host, port, request_bytes):
    response = b""
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request_bytes)
        while True:
            chunk = s.recv(4096)
            if not chunk:
                break
            response += chunk
    return response
def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()

    content_length = len(file_data)
    headers = (
        f"POST /upload HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Content-Length: {content_length}\r\n"
        f"Content-Type: application/octet-stream\r\n"
        f"Connection: close\r\n\r\n"
    ).encode()

    request = headers + file_data
    response = send_request(host, port, request)
    return response.decode(errors='ignore')
def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Connection: close\r\n\r\n"
    ).encode()
    response = send_request(host, port, request)

    header_end = response.find(b"\r\n\r\n")
    if header_end == -1:
        raise ValueError("Invalid HTTP response")

    body = response[header_end + 4:]
    with open(filename, 'wb') as file:
        file.write(body)
if __name__ == "__main__":
    host = 'example.com'
    port = 80
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)
    # Download file
    download_file(host, port, 'example.txt')
    print("File downloaded successfully.")

```
## OUTPUT
![Screenshot 2025-05-07 113220](https://github.com/user-attachments/assets/b3e337dc-1ec9-4c42-8a30-bec5623d295b)

![Screenshot 2025-05-07 112953](https://github.com/user-attachments/assets/59e25b61-edcc-40a7-ba44-9dbbe8170b95)

![Screenshot 2025-05-07 113023](https://github.com/user-attachments/assets/b2ca8b4d-58fd-4c4d-a38e-195bae3b6cf7)

## Result
Thus the socket for HTTP for web page upload and download created and Executed
