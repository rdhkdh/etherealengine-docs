# Troubleshooting

### Browser Debug

```p key``` debug colliders view

### Invalid Certificate errors in local environment

As of this writing, the cert provided in the ethereal engine package for local use
is not adequately signed. Browsers will throw up warnings about going to insecure pages.
You should be able to tell the browser to ignore it, usually by clicking on some sort
of 'advanced options' button or link and then something along the lines of 'go there anyway'.

Chrome sometimes does not show a clickable option on the warning. If so, just
type ```badidea``` or ```thisisunsafe``` when on that page. You don't enter that into the
address bar or into a text box, Chrome is just passively listening for those commands.

### Allow instanceserver address connection via installing local Certificate Authority
For more detailed instructions check: https://github.com/FiloSottile/mkcert

Short version (common for development process on Ubuntu):
1. Execute `sudo apt install libnss3-tools`
2. Execute `brew install mkcert` (if you don't have brew, check it's page: https://brew.sh/)
3. Execute `mkcert --install`
4. Navigate to `./certs` folder
5. Execute `mkcert -key-file key.pem -cert-file cert.pem localhost 127.0.0.1 ::1`

### Allow local file http-server connection with invalid certificate

Open the developer tools in your browser by pressing ```Ctrl+Shift+i``` at the 
same time. Go to the 'Console' tab and look at the message history. If there are 
red errors that say something like
```GET https://127.0.0.1:3030/socket.io/?EIO=3&transport=polling&t=NXlZLTa net::ERR_CERT_AUTHORITY_INVALID```,
then right-click that URL, then select 'Open in new tab', and accept the invalid certificate.

### Allow instanceserver address connection with invalid certificate

The instanceserver functionality is hosted on an address other than 127.0.0.1 in the local
environment. Accepting an invalid certificate for 127.0.0.1 will not apply to this address.
Open the dev console for Chrome/Firefox by pressing ```Ctrl+Shift+i``` simultaneously, and
go to the Console or Network tabs. 

If you see errors about not being able to connect to
something like ```https://192.168.0.81:3031/socket.io/?location=<foobar>```, right click on
that URL and open it in a new tab. You should again get a warning page about an invalid
certificate, and you again need to allow it.  

### AccessDenied connecting to mariadb
Make sure you don't have another instance of mariadb running on port 3306

```
lsof -i :3306
```

On Linux, you can also check if any processes are running on port 3306 with
```sudo netstat -utlp | grep 3306```
The last column should look like ```<ID>/<something```
You can kill any running process with ```sudo kill <ID>```

### Error: listen EADDRINUSE :::3030

Check which process is using port 3030 and kill
```
killall -9 node 
```

Or

```
lsof -i :3030
kill -3 <proccessIDfromPreviousCommand>
```

### 'CORS error' in terminal
Try accessing the page using ```https://localhost:3000``` 
instead of ```https://127.0.0.1:3000```

### Default blank screen

Try typing ```“thisisunsafe”``` or ```"iknowwhatiamdoing"``` then reload page

### Instanceserver or resource loading error?
Open dev console, click on the GET link in new tab and  accept certificate by 
typing ```thisisunsafe”``` or ```"iknowwhatiamdoing"``` then reload original page

### To install a new package for editor react components in monorepo

Type in terminal

     npm i <packagename> -w @etherealengine/editor

### DB not seeding routes (E.g. Error: No project installed- please contact site admin)

Try

  npm run dev-reinit 
or
 
  docker container stop etherealengine_db
  docker container rm etherealengine_db
  docker container prune
  npm run dev-docker
  npm run dev-reinit

### Weird issues with your database?
Try
```
npm run dev-reinit
```
Or if on windows
```
npm run dev-reinit-windows
```
