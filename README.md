# Web Server Workshop

This workshop is intended to students of EPITA SIGL specialization.

In this workshop you will:
- learn how to communicate with a remote server from your computer
- expose a web page on internet using [Nginx](https://www.nginx.com/)

## Step 0 - Create your Github project

From now on until the end of the course, you will code on your own github repository.
Only one repository is needed per pair of students

- Create a new project with the following name format `socra-groupXX` replacing XX by your group number (from 1 to 25)
  - keep 1 digit for groups 1 to 10 (e.g. `socra-group1` for group 1)
  - make sure your repository is private
- Once created, add push a README.md file with your Group number and EPITA logins
```plain
// ./README.md
# SOCRA Group 26

Owners:
- florent.fauchille <florent.fauchille@epita.fr>
- lucas.boisserie <lucas.boisserie@epita.fr>
```
- And add teachers as collaborators:
  - lucas: [LucasBoisserie](https://github.com/LucasBoisserie)
  - florent: [ffauchille](https://github.com/ffauchille)

## Step 1 - Connect to your remote server

This step is only to make sure you have understood how to access your remote server from your local host.

To do so:
- Download your SSH key provided by your teachers
- Once downloaded, type the following commands to connect remotely:
```sh
# From your favourite terminal session

# SSH KEY path can be relative or absolute
# make sure to replace XX by your group number
$ ssh -i <path_to_your_ssh_key> sigl@groupXX.socra-sigl.fr

# Accept the prompt message (only happens on first SSH connection)
# You should be connected remotely
```
> Note: make sure your rights on your private SSH key are not too open
> type `$ chmod 400 <path_to_your_ssh_key>` to make sure you have correct rights

- Add an `AUTHORS.txt` in sigl's user home with your logins:
```plain
# Inside AUTHORS.txt
* florent.fauchille
* lucas.boisserie
```

## Step 2 - Deploy your first web application

In this last step, you will deploy a very minimal first web application on internet.
It will be a simple `html` page with your group and logins.

- Connect to your remote server via ssh
- Install [nginx](https://ubuntu.com/tutorials/install-and-configure-nginx#2-installing-nginx):
```sh
sudo apt update
sudo apt install nginx
```
- Make sure you can see the default `Welcome to nginx!` page from your browser by visiting your group's address http://groupXX.socra-sigl.fr (replace XX by your group number)
- On your local host, create a new `index.html` file with:
```html
<!doctype html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>SOCRA Group XX</title>
  </head>
  <body>
    <h1>Group XX</h1>
    <ul>
      <li>florent.fauchille</li>
      <li>lucas.boisserie</li>
    </ul>
  </body>
</html>

``` 
- Make sure to replace `XX` by your group number
- Make sure to replace list's logins (fauchi_f and boisse_l) by logins member of your pair

Make sure your HTML displays correctly on your browser (simply open your html file with your browser)

Deploy your index.html file by replacing default's NGINX html file on your remote server:
```sh
# from your local host; make sure it ends with ':'
$ scp -i <path_to_your_ssh_key> <path_to_your_index.html> sigl@groupXX.socra-sigl.fr:
```
Connect with ssh to your remote server, and move your `index.html` to default nginx html's folder:
```sh
# see previous step how to connect with ssh to your machine
# Once connected, from your remote server:
$ sudo cp /home/sigl/index.html /var/www/html/index.nginx-debian.html 
```
- You should be all set! 
- Visits your group's address and you should see your group's participants

Congratulation, you just deployed your first version of SOCRA on internet!

## Challenge #1: Secure me

Objective: Catter for `https` requests:

- When visiting `https://groupXX.socra-sigl.fr` you reach your html page
- When visiting `http://groupXX.socra-sigl.fr` you get redirected to `https://groupXX.socra-sigl.fr` and you reach your html page

Use `certbot` and `let's encrypt` to manage your `SSL` certificates.

Visit and follow [certbot instructions](https://certbot.eff.org/instructions).

> Note: You can find out which system you're server is running with `uname -a` command. And get version of your system using `lsb_release -a`


## Challenge #2: Get me Lucas

Objective: When navigating to `https://lucas.groupXX.socra-sigl.fr` in your browser, user gets proxied to `https://github.com/LucasBoisserie`

You should only have to add a new `server {}` block in your nginx configuration.

To try out your changes, just run `nginx reload`.


> Note 1: Nothing is needed on the DNS side, you have wildcard subdomain redirection already configured to point on your server

> Note 2: Check `proxy_pass` nginx module, this could be usefull!

> Note 3: If you have a doubt on the configuration nginx has loaded, you can output it using `nginx -T`
