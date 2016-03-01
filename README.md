#reverse-socks5-proxy
A basic reverse tcp mux server written in node.js.

##Features/Limitations

- up to 255 channels
- raw tcp *only* now
- resource server is *not* auto re-connect
- hub server have no Admin tool/interface yet

##Architecture

- A : resource server (res) (can connect to B & self)
- B : hub server (A & B can connect to)
- C : user/client (can connect to B only)


```
                              |                          A                   |     [network]      |       B        |        |     C    |
'some way else' <--- | socks5 server| <-- | res server | -----------------> | hub server | <---  | client |

```


##Usage
###resource server (A)
set config in `res.js`
```javascript
	var conf = {
		hub_host: '127.0.0.1', // hub server here
		hub_port: 2000,
		local_socks: '127.0.0.1', // your local socks server
		local_socks_port: 8080
	};
```

```
[start your socks server first]
$ node res.js
```

###hub server (B)
set config in `hub.js`
```javascript
	var conf = {
		resbind: 2000, // the port for resource server connect
		slots_base: 2010, // the port for client connect
		slots_count: 5, // from '2010 + 0' to '2010 + 5'
		admin_port: 9999
	};
```

```
$ node hub.js
```

###Client (C)
Just set proxy server to hub server.
