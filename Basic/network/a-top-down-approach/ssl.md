# SSL

TLS 2.0

```
Client                                                             Server
Client Hello                          ----->                                
	                                                                Server Hello
	                                                                  Certficate
	                                                      [opt]Certficate Status
	                                                    [opt]Server Key Exchange
	                                                    [opt]Certificate Request
	                              <-----                       Server Hello Done
Client Key Exchange(1.3 ✖️)
Change Cipher Spec
[Finished]Encrypted Handshake Message ----->
	                                                          Change Cipher Spec
	                              <-----           [Finished]Encrypted Handshake 
Application Data              <------------>                Application Data
```

