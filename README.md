# reverse-proxy
## This project describe  How to set reverse proxy in front of spring boot app

Youtube : https://www.youtube.com/watch?v=e9eriHQJUYc
What the mean reverse proxy : is a server that sits between the client and the origin server and forwards client requests to the origin server on behalf of the client. The main function of a reverse proxy is to act as an intermediary between the client and the origin server, allowing the client to communicate with the origin server indirectly.

Functionality of reverse proxy
- reverse proxies can be used for security purposes, such as hiding the IP address of the origin server from clients.
- improve performance by caching content and serving it directly to clients, without requiring the origin server to handle every request.

Prerequisite :
1. Your operation system is Ubuntu(when make this tutorial i use ubuntu with version 20.04)
2. your laptop already installed Nginx  (if no, see previous tutorial to follow. How to install Nginx)
3. Nginx web server already run
4. You understand most command used in Nginx
5. You have spring boot application(i used spring boot project in my previuos tutorial, you can fork on my github https://github.com/AminTurmudiYusup/login-page-springboot)
6. The spring boot application already run and set to spesific port


Let's jump right in
1. Config context root of spring boot app(open spring boot using your favourite IDEA)
  -server.servlet.context-path=/app(inside application.properties)
2. Create Controller pacakage, create IndexController class and map root into login-page.html
3. map domain name to IP address using this command (sudo gedit /etc/hosts) you can edit using nano or vim etc
  -because i using local computer, so i map my domain(example.com www.example.com) to 127.0.0.1
4. run the bellow command to configure Nginx server
  -sudo nano /etc/nginx/sites-available/default
5. add this script into Nginx configuration
  server_name example.com;

  location /ecs-admin/ {//adjust the location with root spring boot context
            proxy_pass http://127.0.0.1:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
6. ctrl+X , press Y button and Enter
7. make sure if configuration not show the error
   - sudo nginx -t
8. reload Nginx web server
   - sudo systemctl reload nginx
9. and run spring boot app, try to access appliaction using domain(make sure your port same with config script)
   - http://example.com/myapp
10. make sure if the response from port 80(Nginx server run in port 80)

Success, happy learning and happy sharing
