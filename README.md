# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
## CLIENT
```
import socket

# Create a socket
s = socket.socket()
s.connect(('localhost', 8001))
print("Connected to the server successfully!")
while True:
    data = s.recv(1024).decode()
    if not data:
        print("No more frames to receive. Closing connection.")
        break

    print(f"Received frames: {data}")
    
    # Send acknowledgment back to the server
    s.send("Acknowledgment received from client.".encode())

s.close()
```
## SERVER
```
import socket

# Create a socket
s = socket.socket()
s.bind(('localhost', 8001))
s.listen(5)
print("Server is waiting for connection...")

# Accept connection
c, addr = s.accept()
print("Connected with:", addr)

# Input number of frames
size = int(input("Enter number of frames to send: "))
frames = list(range(size))

# Input window size
window_size = int(input("Enter Window Size: "))

start = 0  # Starting frame index

# Send frames in windows
while start < len(frames):
    end = start + window_size
    window = frames[start:end]
    print(f"Sending frames: {window}")
    
    # Send the current window
    c.send(str(window).encode())
    
    # Wait for acknowledgment
    ack = c.recv(1024).decode()
    if ack:
        print(f"Acknowledgment received for frames up to: {ack}")
        start += window_size  # Move the window

print("All frames sent successfully.")
c.close()
s.close()
```

## OUPUT
<img width="1847" height="424" alt="Screenshot 2025-10-30 090629" src="https://github.com/user-attachments/assets/7b1cc58e-86bc-4964-9b93-083cde1fdcf9" />

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
