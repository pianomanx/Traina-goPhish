# Installation

## Installing Gophish Using Pre-Built Binaries

Gophish is provided as a [pre-built binary](https://github.com/gophish/gophish/releases) for most operating systems. With this being the case, installation is as simple as downloading the ZIP file containing the binary that is built for your OS and extracting the contents.

## Installing Gophish from Source

One of the major benefits of having written gophish in the Go programming language is that it is extremely simple to build from source. All you will need is the Go language and a C compiler (such as gcc).

To install gophish, simply run `go get github.com/gophish/gophish`. This downloads gophish into your `$GOPATH`.
Next, navigate to `$GOPATH/src/github.com/gophish/gophish` and run the command `go build`. This builds a gophish binary in the current directory.

## Understanding the config.json
There are some settings that are configurable via a file called config.json, located in the gophish root directory. Here are some of the options that you can set to your preferences:

|Key                       | Value (Default) | Description                     |
|--------------------------|-----------------|---------------------------------|
|admin_server.listen_url   | 127.0.0.1:3333  | IP/Port of gophish admin server |
|admin_server.use_tls      | false           | Use TLS for admin server?       |
|admin_server.cert_path    | example.crt     | Path to SSL Cert                |
|admin_server.key_path     | example.key     | Path to SSL Private Key         |

## Creating SSL Certificate and Private Keys

It’s a good idea to have the admin server available over HTTPS. While automatic SSL cert/key generation will be included in a later release, for now let’s take a look at how we can leverage openssl to generate our cert and key for use with gophish (this assumes you already have openssl installed!)

We can start the certificate and key generation process with the following command:
```
openssl req -newkey rsa:2048 -nodes -keyout gophish.key -x509 -days 365 -out gophish.crt
```
Then, all we have to do is answer the CSR process that asks for details such as country, state, etc. Since this is a local self-signed cert, these won’t matter too much to us.

This creates two files, gophish.key and gophish.crt. After moving these files into the gophish root directory (in the same folder as config.json), we can have the following in our config.json file:
```
	"admin_server" : {
		"listen_url" : "127.0.0.1:3333",
		"use_tls" : true,
		"cert_path" : "gophish.crt",
		"key_path" : "gophish.key"
	}
```
Now when we launch gophish, you’ll connect to the admin server over HTTPS and accept the self-signed certificate warning.

## Running Gophish
Now that you have gophish installed, you’re ready to run the software. To launch gophish, simply open a command shell and navigate to the directory the gophish binary is located.

Then, execute the gophish binary. You will see some informational output showing both the admin and phishing web servers starting up, as well as the database being created. This output will tell you the port numbers you can use to connect to the web interfaces.

```
gophish@gophish.dev:~/src/github.com/gophish/gophish$ ./gophish
 2016/01/10 23:13:42 worker.go:34: Background Worker Started Successfully - Waiting for Campaigns
 2016/01/10 23:13:42 models.go:64: Database not found... creating db at gophish.db
 2016/01/10 23:13:42 gophish.go:49: Admin server started at http://127.0.0.1:3333
 2016/01/10 23:13:42 gophish.go:51: Phishing server started at http://0.0.0.0:80
```