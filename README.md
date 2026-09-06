# UDP-Socket-Based-File-Transfer-System

A socket-based client-server file transfer system implemented in C

## Overview

This project implements a simple client-server application for transferring text files over a network.

The workflow is:

1. The client starts and requests a file by entering its filename.
2. The client sends the filename to the server.
3. The server checks whether the requested file exists and is readable.
4. If the file does not exist, the server sends a `NOTFOUND` message.
5. If the file exists, the server sends the file contents line by line.
6. After receiving each line, the client requests the next line using sequential messages:

   * `WORD1`
   * `WORD2`
   * `WORD3`
   * and so on.
7. The server validates each request before sending the next line.
8. The transfer ends when the line `FINISH` is received.
9. The client saves the received file as:

```text
<original_filename>(copy)
```

---

## Project Structure

```text
TCP-Socket-Based-File-Encryption-System/
│
├── wordclient.c       # UDP client implementation
├── wordserver.c       # UDP server implementation
└── README.md          # Project documentation
```

---

## Features

* Client-server communication using UDP sockets.
* File request mechanism.
* File availability checking on the server.
* Line-by-line file transfer.
* Sequential request validation using `WORD1`, `WORD2`, etc.
* Error handling for invalid client requests.
* File-not-found detection.
* Automatic creation of a copy of the received file on the client side.
* Transfer termination using the `FINISH` message.

---

## Communication Protocol

The application follows a simple request-response protocol.

### Step 1: File Request

The client sends the name of the requested file.

```text
example.txt
```

### Step 2: Server Response

If the file is not available:

```text
NOTFOUND <filename>
```

If the file is available, the server starts sending the file contents.

### Step 3: Sequential Requests

After receiving a line, the client requests the next line using:

```text
WORD1
WORD2
WORD3
...
```

The server verifies that the received request matches the expected request.

For example:

```text
Expected: WORD1
Received: WORD1
```

If the request is invalid, the server responds with:

```text
ERROR: UNHANDLED REQUEST
```

### Step 4: Transfer Completion

The transfer terminates when:

```text
FINISH
```

is sent and received.

---

## Network Configuration

The current implementation uses:

```text
IP Address: 127.127.127.127
Port: 5000
Protocol: UDP
```

Make sure that both the client and server use the same IP address and port configuration.

---

## Compilation

Compile the server:

```bash
gcc wordserver.c -o wordserver
```

Compile the client:

```bash
gcc wordclient.c -o wordclient
```

---

## Running the Application

### 1. Start the Server

Run:

```bash
./wordserver
```

Expected output:

```text
Server running...
```

### 2. Start the Client

In another terminal, run:

```bash
./wordclient
```

The client will ask:

```text
Enter name of the file you want to request:
```

Enter the name of a file available in the server's working directory.

Example:

```text
sample.txt
```

---

## Output

If the requested file is successfully transferred, the client creates a copy with:

```text
(copy)
```

appended to the original filename.

Example:

```text
sample.txt
```

becomes:

```text
sample.txt(copy)
```

---

## Error Handling

### File Not Found

If the requested file does not exist or cannot be read, the server sends a `NOTFOUND` response.

### Invalid Request

If the client sends an unexpected sequential request, the server sends:

```text
ERROR: UNHANDLED REQUEST
```

and terminates the current transfer.

### File Opening Error

Both the client and server check whether files can be opened successfully before continuing.

---

## Current Status

### Implemented

* [x] UDP socket creation.
* [x] Server binding.
* [x] Client file requests.
* [x] File existence checking.
* [x] Line-by-line file transfer.
* [x] Sequential request validation.
* [x] File-not-found handling.
* [x] Invalid request handling.
* [x] Client-side file copy creation.


---

## Future Direction

The intended goal of this repository is to develop a **TCP Socket-Based File Encryption System** where files can be securely transferred between a client and server.

Future versions may include:

* TCP-based reliable communication.
* Encryption of file contents before transmission.
* Secure key management.
* Decryption on the receiving side.
* Improved authentication and error handling.
* Support for transferring different file types.


