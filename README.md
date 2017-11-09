The post is under continious construction, last update: November 9, 2017.  
If you have any suggestions or feedback, you are very welcome to contact me using [github issues](https://github.com/sergeyfrolov/configuring-secure-web-proxy/issues),
or at <sergey.frolov@colorado.edu>.

---

**Setting up the WebServer with ForwardProxy**

This post is mostly about the client, but I will provide a few links to server configuration tutorials.

| Server  | Probing resistance | HTTP/2 | Notes                                                                                                                                                                                          |
|---------|--------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Caddy   |         ✔*         |    ✔   | RECOMMENDED. Configuration is extremely easy, see instruction from Caddy's author Matt Holt in his [blog post on Medium](https://medium.com/@mattholt/private-browsing-without-a-vpn-e91027552700). |
| Apache2 |          ✕         |    ✕   | See how to configure forward proxy on https://httpd.apache.org/docs/2.2/mod/mod_proxy.html. mod_proxy and http2 do not work together.                                                   |
| nghttp2 |          ✕         |    ✔   | Could act as frontend to forwadproxy, say squid, see https://github.com/nghttp2/nghttp2/issues/547                                                                                                        |                                                                                 |

\* Probing resistance in Caddy Web Server is experimental


**Ways to start using Secure Web Proxy on client side:**

* Set up per-appilcation proxy
    * [Google Chrome/Chromium](#google-chromechromium)
    * [Firefox](#firefox)

<!--
    * [TODO: IE/Edge]()
    * [TODO: Safari]()
-->

* Set up system-wide proxy. Most browsers use system proxy by default.

    * [Windows 7](#windows-7-system-wide-proxy)
<!--

    * [Ubuntu 16.04](#ubuntu-16-04-system-wide-proxy)
    * [TODO: Android]()
    * [TODO: iOS?]()
-->

* Use [Proxy Auto-Config](https://en.wikipedia.org/wiki/Proxy_auto-config) (PAC) file.  
    PAC files are less common, but they don't require special apps or extensions
for browsers or mobile platforms and allow flexible configuration.
    * [On Android](#pac-on-android)
    * [On Windows 7](#pac-on-windows-7)
    * [On macOS](#pac-on-macos)
    * [On Ubuntu](#pac-on-ubuntu-16-04)
    * [In Firefox](#pac-in-firefox)

---

# Google Chrome/Chromium

 1. Use extension, for example [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=en). This extension guides user through setup, and here's mine for reference:

    ![SwitchyOmega Setup](https://sfrolov.io/images/secure-web-proxy//20170812-163032.png)

    Then in upper-right corner you can switch between "Direct" connection and your proxy:

    ![SwitchyOmega Switch](https://sfrolov.io/images/secure-web-proxy//20170812-163247.png)

 2. Alternatively, you can pass `--proxy-server="https://sfrolov.io"` (substitute `sfrolov.io`for `your.server.com`) to google-chrome. In Linux/MacOS you can call `google-chrome --proxy-server="https://sfrolov.io"`. In Windows/macOS you can right click on icon and set up passing aforementioned string as argument. 

# Firefox

1. Easiest option would be to use [SwitchyOmega](https://addons.mozilla.org/en-US/firefox/addon/switchyomega/).
Setup is same as for [Google Chrome](#google-chromechromium), just follow the instructions.

2. There is also a way to configure secure web proxy without addons, but it is somewhat less convinient.
Open Menu (upper right corner) → Preferences → Advanced → Network → Settings  
Choose Automatic proxy configuration URL, and paste the following:  

```
data:text/plain,function%20FindProxyForURL(){return%20"HTTPS%20sfrolov.io";}
```  

Don't forget to substitute `sfrolov.io` for `your.server.com`, and don't lose `HTTPS%20` before and `";}` after.  
You can also check _"Do not prompt for authentication if password is saved"_  for convinience.

![Firefox Proxy](https://sfrolov.io/images/secure-web-proxy/20170812-154731.png){: height="50%"}

# Proxy Auto-Config (PAC) File

Remember that you first have to generate PAC file. [ForwardProxy](https://caddyserver.com/docs/http.forwardproxy) plugin for Caddy web server does that automatically.

## PAC on Android

Open Wi-Fi settings → hold/tap on currently used Wi-Fi and choose "Modify Network" → In Advanced options set proxy to "Proxy Auto-Config" → Set PAC URL to whereever your PAC file is, for example, https://sfrolov.io/proxy.pac

## PAC in Firefox

Open Menu (upper right corner)  Preferences → Advanced → Network → Settings → Set Automatic proxy configuration URL to whereever your PAC file is, for example, https://sfrolov.io/proxy.pac

You can also specify full path to local PAC file.

## PAC on Windows 7

Unless you are sure that you need PAC file, I'd recommend to specify [just a proxy](#windows-7-system-wide-proxy)

Control Panel → Network and Internet → Internet Options → Connections → LAN settings → Check the "Use automatic configuration script" and specify url or path to your PAC file.   

## PAC on macOS

System Preferences → Network → Choose needed network and click "Advanced..." → Proxies → Check "Automatic Proxy Configuration" and specify url or path to your PAC file.

## PAC on Ubuntu 16.04

System Settings → Network → Network Proxy → Choose method "Authomatic" and specify url or path to your PAC file → Click "Apply System-Wide"

# System-wide proxy set up

## Windows 7 system wide proxy

Control Panel → Network and Internet → Internet Options → Connections → LAN settings → Check "Use a proxy server..." and paste your "https://yourserver.com" in Address and "443" in port. Don't lose "https://" in Address, Windows likes to remove it when you open LAN settings window again.

![Windows 7 proxy setup](https://sfrolov.io/images/secure-web-proxy/win7-proxy.jpeg)

<!--
## Ubuntu 16.04 system wide proxy

This method may not apply to all applications, and generally not advised to use.  

System Settings → Network → Network Proxy → Choose method "Authomatic" nd paste your "https://yourserver.com" in Address and "443" in port. For kicks do it for HTTP, HTTPS and FTP → Click "Apply System-Wide" 
-->
