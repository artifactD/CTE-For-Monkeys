# Setting up Nginx with TCP proxy 

1. To start lets install Nginx
    ```bash
    sudo apt install nginx 
    ```
2. Now lets create a config file for pass through connections 
    ```bash 
    sudo vi /etc/nginx/conf.d/lp_proxy.conf
    ```
3. Paste the follwing in `lp_proxy.conf`
    ```
    server {
        listen 80;
        #NOTE: This line is so payloads can be downloaded over port 80 using Powershell
        server_name <IP of Proxy Server>;

        location / {
            proxy_pass https://<posh-server IP>:<Posh bind port>;
        }
    }
    ```

4. Now lets reload Nginx with the new config files
    ```bash 
    sudo systemctl restart nginx.service
    ```
5. From here, create a poshc2 project with the bind port set to 80, and in `payloadcomshost` set your `http://proxy-ip`

6. Host your payload however and download on victim 

## Important Nginx File Locations 
- `/var/www/html` – Website content as seen by visitors.
- `/etc/nginx` – Location of the main Nginx application files.
- `/etc/nginx/nginx.conf` – The main Nginx configuration file.
- `/etc/nginx/sites-available` – List of all websites configured through Nginx.
- `/etc/nginx/sites-enabled` – List of websites actively being served by Nginx.
- `/var/log/nginx/access.log` – Access logs tracking every request to your server.
- `/var/log/ngins/error.log` – A log of any errors generated in Nginx.

## [Return to course page](README.md) or [Go To Next Step](step3-persistence.md)