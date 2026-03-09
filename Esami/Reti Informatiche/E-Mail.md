#uni 
E-Mail protocol is described in RFC(5321).
It uses TCP to reliably transfer email messages from clients (mail server that initiates the connection) to server, port 25.

The transfer is divide in 3 phases:
1. handshaking
2. transfer of messages
3. closure

The interaction is command/response like http:
- commands: ASCII text
- response: status code and phrase
The messages must be in 7-bit ASCII
# Sending Emails
1. User composes email
2. sends email to his email server
3. the client side of SMTP server opens a TCP connection with the destination user's server
4. email is sent over the tcp connection
5. destination user can access the email via his email server
# E-mail access protocol
To reach mail providers (gmail, hotmail ecc) we use HTTP and IMAP (internet mail access protocol) (or POP) protocol to retrieve e-mail access.
# Sample SMTP interaction
s: 220 serverName
c: `HELO clientName`
s: 250
c: `MAIL FROM:<emailSourAddr>`
s: 250 emailSourAddr Sender ok
c: `RCPT TO:<emailDestAddr>`
s: 250 emailDestAddr Recipient ok
c: `DATA`
s: 354 Enter mail, end with "."
c: `SUBJECT:`
C: `FROM: emailSourcAddr`
C: `TO: emailDestAddr`
c: message
c: .
s: 250 Message accepted for delivery
c: QUIT
s: 221 serverName closing connection
# Comparison SMTP-HTTP
- HTTP: pull
- SMTP: push
- both have ASCII command/response interaction, status codes
- SMTP uses persistent connections
- SMTP requires message (header and body) to be in 7-bit ASCII
- SMTP server uses CRLF.CRLF to determine end of message
# Mail message format
The format of email messages is specified in RFC 531 (defines protocol) and RFC 822 (defines syntax).
- header lines
	- to:
	- from:
	- subject:
- body: the message, 7-bit ASCII characters only