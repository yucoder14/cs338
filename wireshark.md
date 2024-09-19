# DAYTIME

1. TCP handshake
```
1   192.168.187.128     129.6.15.28         TCP         [SYN]
2   129.6.15.28         192.168.187.128     TCP         [SYN,ACK]
3   192.168.187.128     129.6.15.28         TCP         [SYN]
```
2. 34176 
3. The client need a port because there are multiple processes running on my machine and needs an identifier to disambiguate the source.
4. Daytime protocol
```
4   129.6.15.28         192.168.187.128     DAYTIME     DAYTIME Response
``` 
5. [SYN] means synchronize, or a request to open a TCP connection, and [ACK] means data was received or confirm that connection has been established. 
6. It was the Daytime Server that initiated the closing of TCP connection. When I observed the DAYTIME protocol's packet, it contained a TCP packet, which had the FIN flag set.

# HTTP
1. Total of two TCP connections were opened, for there are two frames with [SYN, ACK] flags set
2. Homepage request
```
5   192.168.187.128     172.233.221.123     HTTP        GET / HTTP/1.1
```
3. Image request 
```
11  192.168.187.128     172.233.221.123     HTTP        GET /jeff-square-colorado.jpg HTTP/1.1
```

# QUESTIONS

- Are series of [PSH, ACK] from the server an indication that it has sent data to the client? 
- what does PSH flag do? 
