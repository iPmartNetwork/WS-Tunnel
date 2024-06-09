<p align="center">
<picture>
<img width="160" height="160"  alt="XPanel" src="https://github.com/iPmartNetwork/iPmart-SSH/blob/main/images/logo.png">
</picture>
  </p> 
<p align="center">
<h1 align="center"/>WS Tunnel</h1>
<h6 align="center">تانل بر اساس وب سوکت <h6>
</p>



## پیش نیاز


 سیتم عامل های تست شده  (ubuntu,debian,centos,fedora)

---
## نصب

بر روی هر دو سرور خارج و ایران اجرا شود .


```
bash <(curl -Ls https://raw.githubusercontent.com/ipmartnetwork/WS-Tunnel/main/wstunnel.sh)

```

---
[لینک اصلی پروژه](https://github.com/erebe/wstunnel/tree/main)


[تشکر ویژه](https://github.com/Ptechgithub)


---

### لیست دستورات
```
Use the websockets protocol to tunnel {TCP,UDP} traffic
wsTunnelClient <---> wsTunnelServer <---> RemoteHost
Use secure connection (wss://) to bypass proxies

Client:
Usage: wstunnel client [OPTIONS] <ws[s]://wstunnel.server.com[:port]>

Arguments:
  <ws[s]://wstunnel.server.com[:port]>  Address of the wstunnel server
                                        Example: With TLS wss://wstunnel.example.com or without ws://wstunnel.example.com

Options:
  -L, --local-to-remote <{tcp,udp,socks5,stdio}://[BIND:]PORT:HOST:PORT>
          Listen on local and forwards traffic from remote. Can be specified multiple times
          examples:
          'tcp://1212:google.com:443'      =>       listen locally on tcp on port 1212 and forward to google.com on port 443

          'udp://1212:1.1.1.1:53'          =>       listen locally on udp on port 1212 and forward to cloudflare dns 1.1.1.1 on port 53
          'udp://1212:1.1.1.1:53?timeout_sec=10'    timeout_sec on udp force close the tunnel after 10sec. Set it to 0 to disable the timeout [default: 30]

          'socks5://[::1]:1212'            =>       listen locally with socks5 on port 1212 and forward dynamically requested tunnel

          'tproxy+tcp://[::1]:1212'        =>       listen locally on tcp on port 1212 as a *transparent proxy* and forward dynamically requested tunnel
          'tproxy+udp://[::1]:1212?timeout_sec=10'  listen locally on udp on port 1212 as a *transparent proxy* and forward dynamically requested tunnel
                                                    linux only and requires sudo/CAP_NET_ADMIN

          'stdio://google.com:443'         =>       listen for data from stdio, mainly for `ssh -o ProxyCommand="wstunnel client -L stdio://%h:%p ws://localhost:8080" my-server`
  -R, --remote-to-local <{tcp,udp}://[BIND:]PORT:HOST:PORT>
          Listen on remote and forwards traffic from local. Can be specified multiple times.
          examples:
          'tcp://1212:google.com:443'      =>     listen on server for incoming tcp cnx on port 1212 and forward to google.com on port 443 from local machine
          'udp://1212:1.1.1.1:53'          =>     listen on server for incoming udp on port 1212 and forward to cloudflare dns 1.1.1.1 on port 53 from local machine
          'socks://[::1]:1212'             =>     listen on server for incoming socks5 request on port 1212 and forward dynamically request from local machine
      --socket-so-mark <INT>
          (linux only) Mark network packet with SO_MARK sockoption with the specified value.
          You need to use {root, sudo, capabilities} to run wstunnel when using this option
  -c, --connection-min-idle <INT>
          Client will maintain a pool of open connection to the server, in order to speed up the connection process.
          This option set the maximum number of connection that will be kept open.
          This is useful if you plan to create/destroy a lot of tunnel (i.e: with socks5 to navigate with a browser)
          It will avoid the latency of doing tcp + tls handshake with the server [default: 0] 
      --tls-sni-override <DOMAIN_NAME>
          Domain name that will be use as SNI during TLS handshake
          Warning: If you are behind a CDN (i.e: Cloudflare) you must set this domain also in the http HOST header.
                   or it will be flagged as fishy and your request rejected
      --tls-verify-certificate
          Enable TLS certificate verification.
          Disabled by default. The client will happily connect to any server with self signed certificate.
  -p, --http-proxy <http://USER:PASS@HOST:PORT>
          If set, will use this http proxy to connect to the server
      --http-upgrade-path-prefix <HTTP_UPGRADE_PATH_PREFIX>
          Use a specific prefix that will show up in the http path during the upgrade request.
          Useful if you need to route requests server side but don't have vhosts [default: morille]
      --http-upgrade-credentials <USER[:PASS]>
          Pass authorization header with basic auth credentials during the upgrade request.
          If you need more customization, you can use the http_headers option.
      --websocket-ping-frequency-sec <seconds>
          Frequency at which the client will send websocket ping to the server. [default: 30]
      --websocket-mask-frame
          Enable the masking of websocket frames. Default is false
          Enable this option only if you use unsecure (non TLS) websocket server and you see some issues. Otherwise, it is just overhead.
  -H, --http-headers <HEADER_NAME: HEADER_VALUE>
          Send custom headers in the upgrade request
          Can be specified multiple time
  -h, --help
          Print help

Server:
Usage: wstunnel server [OPTIONS] <ws[s]://0.0.0.0[:port]>

Arguments:
  <ws[s]://0.0.0.0[:port]>  Address of the wstunnel server to bind to
                            Example: With TLS wss://0.0.0.0:8080 or without ws://[::]:8080

Options:
      --socket-so-mark <INT>
          (linux only) Mark network packet with SO_MARK sockoption with the specified value.
          You need to use {root, sudo, capabilities} to run wstunnel when using this option
      --websocket-ping-frequency-sec <seconds>
          Frequency at which the server will send websocket ping to client.
      --websocket-mask-frame
          Enable the masking of websocket frames. Default is false
          Enable this option only if you use unsecure (non TLS) websocket server and you see some issues. Otherwise, it is just overhead.
      --restrict-to <DEST:PORT>
          Server will only accept connection from the specified tunnel information.
          Can be specified multiple time
          Example: --restrict-to "google.com:443" --restrict-to "localhost:22"
  -r, --restrict-http-upgrade-path-prefix <RESTRICT_HTTP_UPGRADE_PATH_PREFIX>
          Server will only accept connection from if this specific path prefix is used during websocket upgrade.
          Useful if you specify in the client a custom path prefix and you want the server to only allow this one.
          The path prefix act as a secret to authenticate clients
          Disabled by default. Accept all path prefix. Can be specified multiple time
      --tls-certificate <FILE_PATH>
          [Optional] Use custom certificate (.crt) instead of the default embedded self signed certificate.
      --tls-private-key <FILE_PATH>
          [Optional] Use a custom tls key (.key) that the server will use instead of the default embedded one
  -h, --help
          Print help
```
---







# تلگرام

[@ipmart_network](https://t.me/ipmart_network)

[@iPmart Group](https://t.me/ipmartnetwork_gp)




 # حمایت از ما :hearts:
حمایت های شما برای ما دلگرمی بزرگی است<br> 
<p align="left">
<a href="https://plisio.net/donate/kB7QU7f7" target="_blank"><img src="https://plisio.net/img/donate/donate_light_icons_mono.png" alt="Donate Crypto on Plisio" width="240" height="80" /></a><br>
	
|                    TRX                   |                       BNB                         |                    Litecoin                       |
| ---------------------------------------- |:-------------------------------------------------:| -------------------------------------------------:|
| ```TJbTYV1fFo2485sYMyajxGPLFzxmNmPrNA``` |  ```0x4af3de9b303a8d43105e284823d95b4c600961a3``` | ```MPrkzFiNtw4Rg67bbZB6gCxa9LV87orABM``` |	

</p>	




<p align="center">
<picture>
<img width="160" height="160"  alt="XPanel" src="https://github.com/iPmartNetwork/iPmart-SSH/blob/main/images/logo.png">
</picture>
  </p> 




