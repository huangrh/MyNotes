# convert .pem into a .ppk file
# 
- PEM files are also used for SSH. If you’ve ever run ssh-keygen to use ssh without a password, your ~/.ssh/id_rsa is a PEM file, just without the extension. [source](https://www.howtogeek.com/devops/what-is-a-pem-file-and-how-do-you-use-it/)

```
ssh -i keyfile.pem root@host
ssh-add keyfile.pem

```
- you could also always simply append your primary public key to the instance’s ~/.ssh/authorized_keys 
- SSH Key: Ed25519 vs RSA

```
sudo puttygen ppkkey.ppk -O private-openssh -o pemkey.pem
sudo puttygen pemKey.pem -o ppkKey.ppk -O private
```


# [sFTP Upload & Download](https://www.tecmint.com/sftp-upload-download-directory-in-linux/)
```
scp -i /PATH/TO/PUBLICKEY -P 52222 /PATH/TO/SOURCEFILE user@ip:/path/
```

```
> sftp tecmint@192.168.56.10
# priviate_key.pem is a OPENSSH Key.
# a passphrase is required. 
> sftp -i priviate_key.pem tecmint@192.168.56.10
sftp> ls
sftp> pwd
sftp> lpwd
sftp> mkdir
sftp> put -r folder-name
sftp> get -r folder-name
sftp> put -pr folder-name # to preserve the modification times ,  access times and modes from the original files transferred, use -p
sftp> ? # to get help
```

```
# how to extract gz file gunzip
> gzip –dk FileName.gz
```

[how to use curl](https://everything.curl.dev/cmdline/options)

[useful curl commands](https://www.atatus.com/blog/19-useful-curl-commands-that-you-should-know/)

[what is curl](https://developer.ibm.com/articles/what-is-curl-command/)


### cURL  
- stands for client URL, is a command line tool that developers use to transfer data to and from a server.
- Underlying the curl command is the libcurl development library
- There are over two hundred curl options
- Postman is a UI-based client 
- Rest Client in VS Code: Rest Client for VS Code is probably one of my favorite tools for executing curl commands. It's lightweight and has good syntax highlighting. It's a really useful add-on to do some quick curl requests from within VS Code.


### 1. setup
### 2. fundamental usage
> curl http://www.weirdserver.com:8000/
> curl ftp://ftp.funet.fi/README
> curl dict://dict.org/m:curl

### 3. Downloading Files
- -o : option to provide output filename
> curl -o vlc.dmg http://ftp.belnet.be/mirror/videolan/vlc/3.0.4/macosx/vlc-3.0.4.dmg  

- -O : Options to let the cURL figure out the filename
> curl -O http://ftp.belnet.be/mirror/videolan/vlc/3.0.4/macosx/vlc-3.0.4.dmg

- -C to resume/continue to download
> curl -O -C - http://ftp.belnet.be/mirror/videolan/vlc/3.0.4/macosx/vlc-3.0.4.dmg

- -L options to follow the redirects (HTTP 3XX redirect, 
- the server used a 3XX status code and the "Location" header to identify the new URL
> curl -L http://www.instagram.com/
> 
- - i = information. viewing response headers
> curl -L -i http://www.instagram.com/

- v = verbose mode. view request headers and connection information
> curl -v https://www.atatus.com/

- Most of the time, we aren't interested in the body of the response. 
- Simply “save” the output to the null device, which on Linux and macOS is /dev/null and on Windows is NUL.
> curl -vo /dev/null https://www.atatus.com/    # Linux/MacOS
> curl -vo NUL https://www.atatus.com/          # Windows

- Silencing Errors
- s = silence mode: hide the progress bar and error messages, etc. 
> curl -svo /dev/null https://www.atatus.com/      # Linux/MacOS
> curl -svo NUL https://www.atatus.com/            # Windows

- -sS: you can still see the error messages
> curl -sSvo file.html https://www.atatus.com/

### set HTTP request headers
> curl -H 'X-My-Custom-Header: 123' https://httpbin.org/get  
> curl -H 'User-Agent: Mozilla/5.0' -H 'Host: www.instagram.com' https://www.instagram.com/
- The "User-Agent" header can be set with the -A shortcut or --user-agent:
> curl -A Mozilla/5.0 http://httpbin.org/get
> curl --user-agent 'Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0' http://example.com/

- The -e flag can be used to provide a "Referer" header
> curl -e https://www.instagram.com/ https://httpbin.org/get

### make a POST requests
- Using –data is equivalent to using -d
- both of these imply that the curl request made is an HTTP post
> curl --data "firstname=atatus" https://httpbin.org/post
> curl --data "email=test%40example.com" https://httpbin.org/post
> curl --data-urlencode "email=test@example.com" --data-urlencode "name=atatus" https://httpbin.org/post
> 
- If the --data option is too long to type in on the terminal, save it to a file and submit it with @
> curl --data @params.txt example.com
- Use the -F (“form”) option if you want to upload files using a POST request.
> curl -F file=@test.c https://httpbin.org/post

- submit JSON data
> curl --data '{"email":"test@example.com", "name": ["atatus"]}' 
  -H 'Content-Type: application/json' 
  https://httpbin.org/post
> curl --data @data.json https://httpbin.org/post  

### Specify the type of request:
- The -X or —-request option lets you specify the type of HTTP request method
> curl -X POST https://httpbin.org/post
> 
- updating the value of param2 to be test 3 on the record id  
> curl -X 'PUT' -d '{"param1":"test1","param2":"test3"}' http://test.com/1

- You can also alter the method of the request to anything other, like PUT, DELETE, or PATCH. The HEAD method is one significant example, as it cannot be set using the -X option. The HEAD method is used to see if a document is available on the server without having to download it. Use the -I option to use the HEAD method
> curl -I https://www.atatus.com/

### Making cURL Fail on HTTP Errors
- f : fail on http errors
> curl https://www.atatus.com/404 -fsSo file.txt

### With the -u option, you can specify the username and password:
- pass in the credentials when making the request using the -u or -user option
> curl -u atatus:@t@tus https://example.com/
- alternatively
> curl https://atatus:@t@tus@example.com/
> 
- download from ftp server  
> curl -u ftpuser:ftp123 ftp://192.168.1.10/file1.txt -O
> 
- upload file to ftp server. 
> curl -u ftpuser:ftp123 -T file1.txt ftp://192.168.1.10
> 
- Upload multiple files to FTP server
> curl -u curlftp:ftp123 -T '{file1.txt,file2.txt}' ftp://192.168.1.10/ 


### Display Progress bar With # tags:
> curl -# http://example.com -o output_file


### test protocol support   

 The *sslvversion and tlsvversion* flags can be used to see if a site supports a specific version of SSL
> curl -v --tlsv1.2 https://www.atatus.com/
> curl -v --sslv3 https://www.atatus.com/
  
### ignore invalid or self-signed certificates
  
- the -k or --insecure option
> curl -k https://localhost/my_test_endpoint


### Max File Size. 
- the --max-filesize flag use to specify the maximum size (in bytes) of a file to download. 
- If file is larger than the max size, curl command in Linux will not download the file.
> curl --max-filesize 1000 -O https://wordpress.org/latest.zip

- Maximum Data Transfer Rate. The --limit-rate options set the maximum download or upload rate
- Example: use maximum speed of 64 kilobytes per second
> curl --limit-rate 64k http://example.com

### complicated example
> curl --request GET "https://api.nasa.gov/planetary/apod?api_key=$NASA_API_KEY&date=2020-01-01" -s | python3 -c "import sys, json; print(json.load(sys.stdin)['url'])" | xargs curl -o NASApicture.jpg && open -a Preview NASApicture.jpg

