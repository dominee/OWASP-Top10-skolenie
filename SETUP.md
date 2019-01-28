# Lab Setup

This simple guide should help you to re-create the lab for you at home

1. Besides **Docker** you will need [Docker Compose](https://docs.docker.com/compose/install/).
2. You can test it just by running `docker-compose -v`
3. Pick an unused IP address from your home network that will be used to access the lab. I'll use `192.168.1.250` in this example.
4. Assign this additional IP to your network interface, for example on linux to add it to your first ethernet (wire) adapter `sudo ip address add 192.168.1.250/24 dev eth0`
5. Check if it is accessible from your notebook `ping -c 3 192.168.1.250`
7. Rename the configuration file to `.env`. On linux  `mv env .env`
8. Edit the configuration and replace the IP in config with the IP you are using: `vim .env`
9. Start the lab using `docker-compose up -d`. It will take some time for the first time since the images will be downloaded.
10. Check if the DNS resolver is running ok on the lab IP with `host dvwa.lab 192.168.1.250` and working. If yes, setup the DNS server in your networking configuration (on your notebook). After this you should be able to access the lab using hostnames as described in `README.md`.

**Note:** The used `XVWA` image has `allow_url_include = Off` in `php.ini` by default, so RFI will not work without changing the setting in the image.

You can stop the lab using `docker-compose stop`.

