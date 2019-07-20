# SecurityNotes
This involes some o my notes for security understanding

# Session ID and Cookies Vs Access tokens

Session ID and cookie mechanism is used traditionally for storing user session (eg. shopping cart or user login state)
Now this state is managed and stored at server side (typically session ID are stored in DB and points to users state stored in DB)
More the users traffic, more the sessions and more overhead on server system

Cookies are used to store the session ID once passed in to user by server

JWT tokens are used to create a stateless mechanism, in which there is no need for server to store any state or session, server just authenticate the user for the first time  and then give user info directly in jwt token

Read below articles for more details

Awesome Video on REST Security ---->  https://www.youtube.com/watch?v=9CJ_BAeOmW0
https://jwt.io/
https://security.stackexchange.com/questions/81756/session-authentication-vs-token-authentication
https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide


# Digital Signatures:

Digital Signature is used to prove the identity of a user sending a message and also to make sure the message sent is not tampered

It involves:
Client taking the message, hashing it with hashing algorithm, encrypting it with its private key and then encrypting it with receivers public key
Now Receiver will first decrypt first layer by its private key, then decrypt second layer with senders public key(which makes sure its sent by correct user)
and then we can check whether message after hash matches with available hash 
if yes even message tampering is verified

https://medium.com/coinmonks/a-laymans-explanation-of-public-key-cryptography-and-digital-signatures-1090d4bd072e


# SSL Certificates:

It combines both Public Key Infrastructure and Digital Signature Concept

1)first step is on server side (Developer)

Creates a keystore (public/private keypair for server)
Creates a CSR (Certificate Signing Request which has servers or organizations info and servers public key(or senders public key))
Now CSR's are given to CA's who verify the company and signs the certificate which is done in following way(This is the digital signature part):
1)	CA takes the CSR info, hashes it with hash algorithm
2)	Takes data + hash of data and encrypts it with its private key
3)	Encrypts the entire thing again with users or requestors public key which was there in CSR
sends it to requestor
Now Requestor can only decrypt it (Normally done by keystore which has privatekeyEntry once we do import)
and get the signed certificate 
So requestor (developer) has its private key and a certificate which has its public key signed by CA's private key

2) Second step is communication between client and server
Client sends request to server over https and demands for servers identity
Server Sends certificate which is signed by CA's private key
Client (Browser) has CA's public key which it uses to decrypt the certificate and then it gets dev servers data and a hash 
It validates whether data is tampered or no by checking hash equality
and then it got the servers identity which is correct and signed by correct CA

So Client is ensured of server's identity

3) Third step is client sends a secret (symmetric key) to be used for next communication over a SSL Tunnel which cannot be known to anyone
Client takes the public key of server obtained from certificate and encrypts a Change Cipher(AKA Symmetric Key) and sends it to server
No one apart from server can decrypt this, as it can only be done by servers private key
Now both Client and Server has a Secret Symmetric Key for furthur communication 


So with SSL and SSL Certificates we achieved:
1) Identity of server is confirmed by client
2) Encryption of messages by a key which is only known to two mutual parties

Links -->  https://medium.com/@User3141592/what-is-ssl-and-how-does-it-work-a5465d19b494
