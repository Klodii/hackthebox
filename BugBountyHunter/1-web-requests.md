## HTTP

**Note**: the default port for HTTP communication is port 80

### URL

![url](imgs/url_structure.webp)

| Component    | Example           | Description    |
|--------------|-------------------|----------------|
| Scheme       | http:// https://  | This is used to identify the protocol being accessed by the client, and ends with a colon and a double slash (://) |
| User Info    | admin:password@   |	This is an optional component that contains the credentials (separated by a colon :) used to authenticate to the host, and is separated from the host with an at sign (@) |
| Host         | inlanefreight.com |	The host signifies the resource location. This can be a hostname or an IP address |
| Port         | :80               | The Port is separated from the Host by a colon (:). If no port is specified, http schemes default to port 80 and https default to port 443 |
| Path         | /dashboard.php    | This points to the resource being accessed, which can be a file or a folder. If there is no path specified, the server returns the default index (e.g. index.html). |
| Query String | ?login=true       | The query string starts with a question mark (?), and consists of a parameter (e.g. login) and a value (e.g. true). Multiple parameters can be separated by an ampersand (&). |
| Fragments    | #status           | Fragments are processed by the browsers on the client-side to locate sections within the primary resource (e.g. a header or section on the page). |

The main mandatory fields are the scheme and the host, without which the request
would have no resource to request.

### HTTP Flow

![http_flow](imgs/HTTP_Flow.webp)

1. the browser asks the DNS the IP address of the given URL
2. the DNS returns the IP address
3. The browser sends a HTTP request to the server in IP address
4. The server will responde back

**Note**: Our browsers usually first look up records in the local '/etc/hosts' file,
and if the requested domain does not exist within it, then they would contact
other DNS servers. We can use the '/etc/hosts' to manually add records to for
DNS resolution, by adding the IP followed by the domain name.


## HTTPS
To counter the Man-in-the-middle problem that the HTTP has, the HTTPS (HTTP
Secure) protocol was created, in which all communications are transferred in an
encrypted format, so even if a third party does intercept the request, they
would not be able to extract the data out of it. For this reason, HTTPS has
become the mainstream scheme for websites on the internet, and HTTP is being
phased out, and soon most web browsers will not allow visiting HTTP websites.

HTTP requests are sent in clean, so if it is intercepted, all the information
will be visible by all; even username and password data.

In contrast, an HTTPS request is enctrypted and all the data are passed through
a encrypted string. So, no information is leaked.

### HTTPS Flow

![https_flow](imgs/HTTPS_Flow.webp)

If we type http:// instead of https:// to visit a website that enforces HTTPS,
the browser attempts to resolve the domain and redirects the user to the
webserver hosting the target website. A request is sent to port 80 first, which
is the unencrypted HTTP protocol. The server detects this and redirects the
client to secure HTTPS port 443 instead. This is done via the 301 Moved
Permanently response code, which we will discuss in an upcoming section.

Next, the client (web browser) sends a "client hello" packet, giving information
about itself. After this, the server replies with "server hello", followed by a
key exchange to exchange SSL certificates. The client verifies the
key/certificate and sends one of its own. After this, an encrypted handshake is
initiated to confirm whether the encryption and transfer are working correctly.

Once the handshake completes successfully, normal HTTP communication is
continued, which is encrypted after that. This is a very high-level overview of
the key exchange, which is beyond this module's scope.

## HTTP Headers

### General Headers
General headers are used in both HTTP requests and responses. They are
contextual and are used to describe the message rather than its contents.


| Header     | Example                             | Description |
|------------|-------------------------------------|-------------|
| Date       | Date: Wed, 16 Feb 2022 10:38:44 GMT | Holds the date and time at which the message originated. It's preferred to convert the time to the standard UTC time zone. |
| Connection | Connection: close                   | Dictates if the current network connection should stay alive after the request finishes. Two commonly used values for this header are close and keep-alive. The close value from either the client or server means that they would like to terminate the connection, while the keep-alive header indicates that the connection should remain open to receive more data and input. |

### Entity Headers
Similar to general headers, Entity Headers can be common to both the request and
response. These headers are used to describe the content (entity) transferred by
a message. They are usually found in responses and POST or PUT requests.


| Header           | Example                     | Description |
|------------------|-----------------------------|-------------|
| Content-Type     | Content-Type: text/html     | Used to describe the type of resource being transferred. The value is automatically added by the browsers on the client-side and returned in the server response. The charset field denotes the encoding standard, such as UTF-8. |
| Media-Type       | Media-Type: application/pdf | The media-type is similar to Content-Type, and describes the data being transferred. This header can play a crucial role in making the server interpret our input. The charset field may also be used with this header. |
| Boundary         | boundary="b4e4fbd93540"     | Acts as a marker to separate content when there is more than one in the same message. For example, within a form data, this boundary gets used as --b4e4fbd93540 to separate different parts of the form. |
| Content-Length   | Content-Length: 385         | Holds the size of the entity being passed. This header is necessary as the server uses it to read data from the message body, and is automatically generated by the browser and tools like cURL. |
| Content-Encoding | Content-Encoding: gzip      | Data can undergo multiple transformations before being passed. For example, large amounts of data can be compressed to reduce the message size. The type of encoding being used should be specified using the Content-Encoding header. |

### Request Headers

The client sends Request Headers in an HTTP transaction. These headers are used
in an HTTP request and do not relate to the content of the message. The
following headers are commonly seen in HTTP requests.


| Header        | Example                                | Description |
|---------------|----------------------------------------|-------------|
| Host          | Host: www.inlanefreight.com            | Used to specify the host being queried for the resource. This can be a domain name or an IP address. HTTP servers can be configured to host different websites, which are revealed based on the hostname. This makes the host header an important enumeration target, as it can indicate the existence of other hosts on the target server. |
| User-Agent    | User-Agent: curl/7.77.0                | The User-Agent header is used to describe the client requesting resources. This header can reveal a lot about the client, such as the browser, its version, and the operating system. |
| Referer       | Referer: http://www.inlanefreight.com/ | Denotes where the current request is coming from. For example, clicking a link from Google search results would make https://google.com the referer. Trusting this header can be dangerous as it can be easily manipulated, leading to unintended consequences. |
| Accept        | Accept: */*                            | The Accept header describes which media types the client can understand. It can contain multiple media types separated by commas. The */* value signifies that all media types are accepted. |
| Cookie        | Cookie: PHPSESSID=b4e4fbd93540         | Contains cookie-value pairs in the format name=value. A cookie is a piece of data stored on the client-side and on the server, which acts as an identifier. These are passed to the server per request, thus maintaining the client's access. Cookies can also serve other purposes, such as saving user preferences or session tracking. There can be multiple cookies in a single header separated by a semi-colon. |
| Authorization | Authorization: BASIC cGFzc3dvcmQK      | Another method for the server to identify clients. After successful authentication, the server returns a token unique to the client. Unlike cookies, tokens are stored only on the client-side and retrieved by the server per request. There are multiple types of authentication types based on the webserver and application type used. |

### Response Headers

Response Headers can be used in an HTTP response and do not relate to the
content. Certain response headers such as Age, Location, and Server are used to
provide more context about the response. The following headers are commonly seen
in HTTP responses.


| Header           | Example                                   | Description |
|------------------|-------------------------------------------|-------------|
| Server           | Server: Apache/2.2.14 (Win32)             | Contains information about the HTTP server, which processed the request. It can be used to gain information about the server, such as its version, and enumerate it further. |
| Set-Cookie       | Set-Cookie: PHPSESSID=b4e4fbd93540        | Contains the cookies needed for client identification. Browsers parse the cookies and store them for future requests. This header follows the same format as the Cookie request header. |
| WWW-Authenticate | WWW-Authenticate: BASIC realm="localhost" | Notifies the client about the type of authentication required to access the requested resource. |

### Security Headers

Finally, we have Security Headers. With the increase in the variety of browsers
and web-based attacks, defining certain headers that enhanced security was
necessary. HTTP Security headers are a class of response headers used to specify
certain rules and policies to be followed by the browser while accessing the
website.


| Header                    | Example                                     | Description |
|---------------------------|---------------------------------------------|-------------|
| Content-Security-Policy   | Content-Security-Policy: script-src 'self'  | Dictates the website's policy towards externally injected resources. This could be JavaScript code as well as script resources. This header instructs the browser to accept resources only from certain trusted domains, hence preventing attacks such as Cross-site scripting (XSS). |
| Strict-Transport-Security | Strict-Transport-Security: max-age=31536000 | Prevents the browser from accessing the website over the plaintext HTTP protocol, and forces all communication to be carried over the secure HTTPS protocol. This prevents attackers from sniffing web traffic and accessing protected information such as passwords or other sensitive data. |
| Referrer-Policy           | Referrer-Policy: origin                     | Dictates whether the browser should include the value specified via the Referer header or not. It can help in avoiding disclosing sensitive URLs and information while browsing the website. |

## Notes

- Having a valid cookie may be enough to get authenticated into many web
  applications. This can be an essential part of some web attacks, like
  Cross-Site Scripting
