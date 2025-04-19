# Linux Networking Commands

Linux provides powerful tools for monitoring, diagnosing, and troubleshooting network issues. Below, we explore essential commands and how to use them effectively.

## 1. **Checking Connectivity**

=== "ping"
    - **Usage**: Test connectivity to a host.
      ```bash
      ping google.com
      ```
      - Sends ICMP packets to the destination and measures the response time.
      - Use `Ctrl+C` to stop the command.

    - Troubleshooting Example:
        - If `ping` fails, it could indicate:
          - Network connectivity issues.
          - DNS resolution problems.
=== "traceroute"
    - **Usage**: Trace the path packets take to a destination.
      ```bash
      traceroute google.com
      ```
      - Identifies where delays or failures occur in the route.

    - Troubleshooting Example:
        - Use `traceroute` to pinpoint where packets are getting dropped.

## 2. **Viewing Network Configuration**

=== "ip addr"
    - **Usage**: Display network interfaces and IP addresses.
      ```bash
      ip addr
      ```
      - Look for `inet` to find assigned IPs.

    - Troubleshooting Example:
        - If no IP is assigned, check your DHCP configuration or static IP settings.
=== "ifconfig (Deprecated)"
    - **Usage**: Similar to `ip addr` but older.
      ```bash
      ifconfig
      ```

## 3. **Examining Network Connections**

=== "netstat"
    - **Usage**: Display network connections, routing tables, and more.
      ```bash
      netstat -an
      ```
=== "ss"
    - **Usage**: Faster, modern replacement for `netstat`.
      ```bash
      ss -tuln
      ```
    -Troubleshooting Example:
        - Use `ss` to identify open ports and active connections.


## 4. **Querying DNS**

=== "nslookup"
    - **Usage**: Query DNS for IP addresses.
      ```bash
      nslookup google.com
      ```
=== "dig"
    - **Usage**: Advanced DNS query tool.
      ```bash
      dig google.com
      ```
    - Troubleshooting Example:
        - Use `nslookup` or `dig` to verify DNS resolution.
          - If DNS fails, check your `/etc/resolv.conf` file or DNS server configuration.
