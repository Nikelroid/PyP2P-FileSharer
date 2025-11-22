# PyP2P File Sharer

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Socket](https://img.shields.io/badge/Socket-TCP%2FIP-orange)
![Threading](https://img.shields.io/badge/Threading-Concurrency-green)
![JSON](https://img.shields.io/badge/JSON-Data_Exchange-lightgrey?logo=json&logoColor=white)


## Description
This project is a robust implementation of a Peer-to-Peer (P2P) file sharing system built entirely in Python using standard socket programming libraries. It employs a **hybrid P2P architecture** where a central **Tracker** server manages the index of connected peers and their shared files, while the actual file transfers occur directly between **Peers** via TCP connections.

The system handles dynamic peer registration, file searching, and concurrent downloads/uploads using multi-threading. It also includes a "keep-alive" mechanism to ensure the tracker maintains an active list of online peers.

## Features
* **Centralized Indexing:** A Tracker server maintains a realtime database of active peers and available files.
* **Direct P2P Transfer:** Files are transferred directly from source to destination without passing through the server.
* **Multi-threaded Architecture:**
    * Peers can download and upload simultaneously.
    * Tracker handles multiple peer requests concurrently.
* **Peer Discovery:** Users can search for files by name and receive a list of peers hosting them.
* **Live Status Monitoring:** Peers send periodic "keep-alive" signals; the Tracker automatically removes inactive peers.
* **Dynamic File Management:** Automatically detects files in the user's shared directory upon startup.

## Project Structure & File Details

### 1. `Tracker.py`
This is the central server component.
* **Functionality:**
    * Binds to a specific host and port to listen for peer messages.
    * Maintains data structures for `peers` (user details) and `files` (file-to-peer mapping).
    * **Background Monitor:** Runs a thread `monitor_peers()` that checks for heartbeat signals and removes peers that have timed out.
    * **Request Handling:** Processes JSON-formatted commands:
        * `register`: Adds a new peer and their files to the index.
        * `search`: Returns a list of peers holding a specific file.
        * `keep_alive`: Updates the timestamp for a peer.
        * `exit`: Gracefully removes a peer from the network.
* **Key Config:** Default runs on `localhost` port `5000`.

### 2. `Peer.py`
This represents the client application.
* **Functionality:**
    * **Server Mode:** Starts a background thread `start_listening()` to accept TCP connections from other peers wanting to download files.
    * **Client Mode:** Provides a CLI menu for the user to Search, Download, or List files.
    * **Tracker Communication:** Connects to `Tracker.py` to register files and search for content.
    * **File Transfer:**
        * `download_file()`: Connects to a target peer, requests file size, and receives data in chunks.
        * `handle_peer_connection()`: Serves requested files to other peers.
    * **Heartbeat:** Runs a `keep_alive()` thread to ping the tracker every few seconds.
* **File Storage:** Creates a directory `shared_files/<username>` to store and share files.

## Installation

1.  **Prerequisites:** Ensure you have Python 3.x installed. No external `pip` packages are required as this uses standard libraries (`socket`, `threading`, `json`, `os`, `time`).

2.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/nikelroid/socketprogramming-p2p.git](https://github.com/nikelroid/socketprogramming-p2p.git)
    cd socketprogramming-p2p
    ```

## Usage

To test the application, you will need to run one Tracker instance and at least two Peer instances (in separate terminal windows).

### Step 1: Start the Tracker
Open a terminal and run:
```bash
python Tracker.py
````

*The tracker will start listening on 127.0.0.1:5000.*

### Step 2: Start Peer 1

Open a new terminal and run:

```bash
python Peer.py
```

1.  Enter a username (e.g., `Alice`).
2.  The script will create a folder `shared_files/Alice`. **Place some dummy files (e.g., `test.txt`) inside this folder manually now if you want to share them.**
3.  The peer will register with the tracker.

### Step 3: Start Peer 2

Open a third terminal and run:

```bash
python Peer.py
```

1.  Enter a different username (e.g., `Bob`).
2.  This creates `shared_files/Bob`.

### Step 4: Transfer Files

1.  In **Bob's** terminal, select option `2` (Search for a file).
2.  Type the name of a file located in Alice's folder (e.g., `test.txt`).
3.  The tracker will return Alice's details (IP and Port).
4.  Select option `3` (Download a file).
5.  Enter the filename (`test.txt`) and Alice's details as prompted.
6.  The file will be downloaded directly from Alice to `shared_files/Bob`.

## Contributing

Contributions are welcome\! Please follow these steps:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/YourFeature`).
3.  Commit your changes.
4.  Push to the branch.
5.  Open a Pull Request.

## License

This project is open-source.
## Contact

For support or queries, please contact the repository maintainers or open an issue on GitHub.
