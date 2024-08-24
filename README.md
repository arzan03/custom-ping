# Ping Utility with Statistics

This Python script implements a simple ping utility that sends ICMP echo requests to a specified host, calculates round-trip times (RTTs), and provides statistics including packet loss, minimum, maximum, average RTT, and RTT standard deviation. It also includes a user-friendly command-line interface for customization.

## Features

- **Checksum Calculation**: Ensures data integrity of ICMP packets.
- **Multi-Request Pinging**: Sends multiple ping requests and tracks RTT for each.
- **Comprehensive Statistics**: Reports packet loss, minimum, maximum, average RTT, and RTT standard deviation.
- **Command-Line Interface**: Uses `argparse` for user customization.

## Requirements

- Python 3.x
- Administrative privileges (for raw socket access)

## Usage

1. **Save the Script**

   Save the provided Python script as `ping.py`.

2. **Run the Script**

   Execute the script from the command line. You can specify the target host and the number of ping requests.

   ```bash
   python ping.py <host> <count>
![Output_Example](./shot.png?raw=true "Output Example")
## How It Works

The Custom Ping Utility script is designed to send ICMP Echo Requests (pings) to a target and measure the Round-Trip Time (RTT) for each request. Here’s a step-by-step breakdown of how the script operates:

### 1. Checksum Calculation

Before sending an ICMP packet, the script calculates a checksum to ensure data integrity.

- **Function:** `checksum(data)`
- **Process:**
  - Adjusts the data length to ensure it is even.
  - Unpacks the data into 16-bit words.
  - Sums the words and adds any overflow bits.
  - Inverts the result to obtain the checksum.

### 2. Sending a Ping Request

The script creates and sends an ICMP Echo Request packet to the target.

- **Function:** `send_ping_request(destination, sequence_number)`
- **Process:**
  - **Create Socket:** A raw socket is created for ICMP communication.
  - **Build Header:** Constructs the ICMP header with type (8 for Echo Request), code (0), checksum (initially 0), identifier, and sequence number.
  - **Calculate Checksum:** Computes and updates the checksum for the header.
  - **Send Packet:** Sends the ICMP packet to the target and records the time of sending.

### 3. Receiving a Ping Reply

The script waits for a reply that matches the sent request.

- **Function:** `receive_ping_reply(icmp_socket, expected_seq_number)`
- **Process:**
  - **Listen for Response:** Continuously listens for incoming ICMP packets on the socket.
  - **Extract Header:** Extracts and inspects the ICMP header from the response.
  - **Match Reply:** Checks if the response is an Echo Reply (type 0) with the correct sequence number.
  - **Record Time:** Captures the time when the reply is received.

### 4. Calculating RTT

For each received reply, the script calculates the Round-Trip Time (RTT).

- **Function:** `ping(target, num_requests)`
- **Process:**
  - **Send Requests:** Sends the specified number of ping requests to the target.
  - **Receive Replies:** Receives and processes replies, calculating the RTT for each.
  - **Store RTT:** Collects RTT values in a list.

### 5. Displaying Statistics

After completing all ping requests, the script calculates and displays summary statistics.

- **Packet Loss:** Computes the percentage of lost packets.
- **RTT Statistics:** Calculates minimum, maximum, average, and standard deviation of RTT values.
- **Print Results:** Outputs the RTT statistics and packet loss percentage.

### Example Workflow

1. **Send Request:** The script sends an ICMP Echo Request to `example.com` with sequence number 0.
2. **Receive Reply:** The script waits for a reply and verifies it matches the sequence number 0.
3. **Measure RTT:** The time difference between sending the request and receiving the reply is calculated.
4. **Repeat:** This process is repeated for the specified number of requests.
5. **Calculate Stats:** After all requests, the script calculates and prints statistics on RTT and packet loss.

By following these steps, the Custom Ping Utility provides a simple way to measure network latency and reliability to a target host.