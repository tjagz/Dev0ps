



# SETTING UP A MULTIPLE STATIC WEBSITES ON A SINGLE SERVER USING NGINX VIRTUAL HOSTS


## In this project we will be setting up root domains and sub-domains of two websites on a single server using Nginx virtual host configuration;


* SPIN UP UBUNTU AWS WEBSERVER ***EC2 INSTANCE AND ASSOCIATING ELASTIC IP***


* SSH INTO THE SERVER THEN INSTALL AND CONFIGURE NGINX ON THE SERVER


* CREATE TWO WEBSITE DIRECTORIES WITH TWO DIFFERENT WEBSITE TEMPLATES


* CREATE THE DOMAINS (TWO MAIN DOMAINS AND TWO SUBDOMAINS OF THE TWO DIFFERENT WEBSITE TEMPLATES)



* CREATE AN A RECORD FOR THE DOMAINS


* VALIDATE THE DOMAINS SETUP



* CREATE CERBOT SSL CERTIFICATES FOR THE DOMAINS


* CONFIGURE CERBOT ON NGINX FOR THE WEBSITES


* VALIDATE THE DOMAIN WEBSITES SSL USING THE OPENSSSL UTILITY




## DOCUMENTATION




### SPIN UP UBUNTU AWS WEBSERVER ***EC2 INSTANCE AND ASSOCIATING ELASTIC IP***



* I logged into my aws console, initiated an Ec2 instance and associated an elastic ip.


***NOTE***: Kindly Check https://github.com/tjagz/Devops00/blob/master/projectA.md for a better understanding on how to do this.





### SSH INTO THE SERVER THEN INSTALL AND CONFIGURE NGINX ON THE SERVER



* Opened my terminal, ssh into the sever and i configured nginx on the server with the execution of the following commands;

- ***sudo apt update***

- ***sudo apt upgrade***

- ***sudo apt install nginx***




- ***sudo systemctl start nginx*** (This started the Nginx server)

- ***sudo systemctl enable nginx*** ( This enabled Nginx to start on boot)

- ***sudo systemctl status nginx*** (Confirmed its running)


- ***sudo apt install unzip*** (Installed the unzip tool to unzip the url files)





[1](./img/1.png)



* Pasted my publicIPv4 address/Elastic Ip address in the web browser to view the default Nginx startup page.




[2](./img/2.png)








### CREATE TWO WEBSITE DIRECTORIES WITH TWO DIFFERENT WEBSITE TEMPLATES


* Went to tooplate.com, navigated and located the two templates I used.


* Scrolled down and located the the download button for the template, right clicked on it and selected inspect on the drop down.



[3](./img/3.png)


***NOTE***: I did same for the second template



* Clicked on the double arrow for more tabs and clicked Network.




[4](./img/4.png)




***NOTE***: I did same for the second template





* Scrolled down and clicked on the download button. While waiting to see the zip file of the template at the Inspect/Network/doc zone.

* Right clicked on the zip file, scrolled down to copy and i clicked on copy url.



[5](./img/5.png)




***NOTE***: I did same for the second template



- Pasted both URL (s) on Notepad and edited the ***curl*** commands for both URL(s)



[6](./img/6.png)



* ***Went back to the terminal and changed directory to /var/www/html***

- ***cd /var/www/html***   (Changed directoy)


* Excecuted the sudo curl command to download and  sudo unzip command to unzip the website files together (for both URL(s)). I Just copied from my notepad and pasted in my terminal.


***URL.1***

- ***sudo curl -o /var/www/html/2107_new_spot.zip https://www.tooplate.com/zip-templates/2107_new_spot.zip && sudo unzip -d /var/www/html/ /var/www/html/2107_new_spot.zip && sudo rm -f /var/www/html/2107_new_spot.zip***



***URL.2***


- ***sudo curl -o /var/www/html/2110_character.zip https://www.tooplate.com/zip-templates/2110_character.zip && sudo unzip -d /var/www/html/ /var/www/html/2110_character.zip && sudo rm -f /var/www/html/2110_character.zip***





[7](./img/7.png)




[8](./img/8.png)





* I setup my website's configuration by creating new files in the Nginx sites-available directory. 



 Executed the followning commands;



- ***cd /etc/nginx/sites-available***  (Changed directory)




-***sudo nano /etc/nginx/sites-available/character*** (opened text editor into a blank file) ***New file created***

***NOTE***: /character is the name from my url.zipfile(2110_character.zip). Hence naming the text file character.





[9](./img/9.png)








- ***sudo nano /etc/nginx/sites-available/newspot*** (opened text editor into a blank file) ***New file created for the second url***


- Copied and pasted the below content into both blank opened text editors;



 {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}




- Edited the root directive within my server block point to the directory where my downloaded website content is stored.

- Edited server_name also to my domain names (Will be creating an A record with this)



[10](./img/10.png)



[10.1](./img/10.1.png)




***NOTE***: I did same for the second one also edited the root directive to root /var/www/html/2107_new_spot.zip and also the server_name newspot.okoko.click www.newspot.okoko.click







* Then I created a symbolic link for both websites by running the following commands;

- ***sudo ln -s /etc/nginx/sites-available/character /etc/nginx/sites-enabled/character***

-  ***sudo ln -s /etc/nginx/sites-available/newspot /etc/nginx/sites-enabled/newspot***


- ***sudo nginx -t***


- ***sudo rm /etc/nginx/sites-available/default***

- ***sudo rm /etc/nginx/sites-enabled/default***


- ***sudo system restart nginx***




[11](./img/11.png)







### CREATE AN A RECORD FOR THE DOMAINS


* I used my domain name bought from namecheap.com to set up a DNS record.

* Hosted on AWS Route 53, where i setup an A record.


* Went to my AWS console, Route 53, selected the domain name and clicked on Create record. 

 ***Reference projectA on how i hosted my domain name/setting up an A record*** 



[13](./img/13.png)




* Pasted my IP address and clicked Create records






[12](./img/12.png)



* Clicked on create record again, to create for my subdomain



[14](./img/14.png)




* Repeated same for the second sub-domain and confirmed both in the recxord list




[15](./img/15.png)







### VALIDATE THE DOMAINS SETUP


* Went to my terminal and restarted Nginx by executing this command;


- ***sudo systemctl restart nginx***




* Verified the accessibility of the domain names in my web browser



* ***character.okoko.click***


[16.1](./img/16.1.png)



* ***newspot.okoko.click***



[1]






### CREATE CERBOT SSL CERTIFICATES FOR THE DOMAINS

 * Installed certbot by executing the folllowing commands;

 - ***sudo apt update***

 - ***sudo apt install python3-cerbot-nginx***

 - ***sudo cerbot --nginx***






### VALIDATE THE DOMAIN WEBSITES SSL USING THE OPENSSSL UTILITY



* Verified the website's SSL using the OpenSSL utility with this command;

-  ***openssl s_client -connect cleaning.cloudghoul.online:443***




* Visited the websites







## The End of Project 2












