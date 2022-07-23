# Deploy multiple websites on an Nginx server that runs on the EC2 instance and apply SSL.

#### Description:

- Create an EC2 server.
- Install Nginx in it.
- Host multiple websites with multiple pages like about.html, contact.html, etc and deploy it on that Nginx server.
- Create an elastic IP and attach it to the ec2 instance.
- Configure domain and apply SSL.
- Configure automation script for SSL renewal.
- Migrate Freenom DNS to Route53.

#### Step-1: Create an EC2 server

Firstly, log in to the AWS management console.

![Img-1](https://user-images.githubusercontent.com/74168188/178555843-f062573f-166c-4b06-b947-d2d11da46507.png)

After login, navigate to the search bar, type EC2, and select EC2

![Img-2](https://user-images.githubusercontent.com/74168188/178555883-5e169bfd-d205-4b99-9992-8dca7e957ff2.png)

Now, the EC2 dashboard will appear. Click Instances

![Img-3](https://user-images.githubusercontent.com/74168188/178555921-6213742a-726c-4532-923e-3edc4c4d1413.png)

Click the Launch instances button.

![Img-4](https://user-images.githubusercontent.com/74168188/178555939-529e74f0-6ece-43dc-aa1c-98d6b7f2deda.png)

To launch an EC2 instance, a few details are required, i.e., instance name, OS image (AMI), instance type, etc.

![Img-5](https://user-images.githubusercontent.com/74168188/178555964-f159e8e9-9b0e-40df-9d5b-7baa60613640.png)
![Img-6](https://user-images.githubusercontent.com/74168188/178556050-f90b180a-0dca-48fb-b30b-8365f9ac8f28.png)
![Img-7](https://user-images.githubusercontent.com/74168188/178556073-b0a18233-2e38-4dbd-926d-f7e411f7fa06.png)

Select a key pair to attach with the instance to log in with that key. If you do not have key pair, then create a new key pair by clicking Create a new key pair. Moreover, according to usage, download key pair in .pem or .ppk format.

![Img-8](https://user-images.githubusercontent.com/74168188/178556119-067cdf5b-023e-46ad-91f5-e9d536e048c4.png)

![Img-9](https://user-images.githubusercontent.com/74168188/178556147-35533b8a-5551-4386-84ea-e36e90c8e0b3.png)

Now, to allow traffic from the internet, we need to create a rule in network settings.

![Img-10](https://user-images.githubusercontent.com/74168188/178556169-37041f2e-e272-4d66-90f3-736fb5a0f726.png)

We are good to go to launch an instance for this. Click the Launch instance button. If everything is correctly set up, you will find a success message on the screen. Click on the instance id to navigate to the EC2 dashboard.

![Img-11](https://user-images.githubusercontent.com/74168188/178556301-ac2e5bdd-7efa-4ad4-99d9-b669e599eefd.png)

Now, take the public IP from the EC2 dashboard and use it to login inside the instance using ssh.

![Img-12](https://user-images.githubusercontent.com/74168188/178556348-e8cd639f-b6a0-422e-9a82-10575ee35ceb.png)

Finally, it will be logged in successfully if everything is configured correctly.

#### Step-2: Install Nginx in EC2 server

First, update the list of packages using ```sudo apt update```

![Img-13](https://user-images.githubusercontent.com/74168188/178556381-7181b99e-cf22-4749-9e5c-c66e4e1e2198.png)

Now, install Nginx using ```sudo apt install nginx```

![Img-14](https://user-images.githubusercontent.com/74168188/178556410-a0cb6304-f811-4b31-8350-29145a0476b5.png)

To check whether Nginx is successfully installed or not, use ```sudo nginx -v```

![Img-15](https://user-images.githubusercontent.com/74168188/178556440-a711a7c2-634d-448a-9712-6b3d4bf4697e.png)

#### Step-3: Host multiple websites with multiple pages like about.html, contact.html, etc and deploy it on that Nginx server

Here, we will be hosting two websites whose domain are mannan18.ml and test1354.ml. So we have to create two document root. For mannan18.ml document is /var/www/html and for test1354.ml document root is /var/www/test1354.ml/html

![image](https://user-images.githubusercontent.com/74168188/179043011-7f7da30d-3d3e-4a0a-9acf-042407a9da4f.png)

![image](https://user-images.githubusercontent.com/74168188/179042808-bf71de2c-ede2-4aa8-9e15-ef6a33f7789b.png)

Now, we have to create server block for each website this concept is known as virtual hosts. Copy the default file then change accordingly.

![image](https://user-images.githubusercontent.com/74168188/179044239-e8a67b64-b8c5-4958-9029-9b689e898c4f.png)

![image](https://user-images.githubusercontent.com/74168188/179043742-5c8ad5c2-b1e4-4252-9b0c-d49f5270bfe3.png)

Update document root path, server_name, and remove default_server keyword in server block file and save by pressing Esc key then write :wq!

![image](https://user-images.githubusercontent.com/74168188/179125810-7ade8bdf-7c1a-490a-a218-fcfcbf10a0f4.png)

![image](https://user-images.githubusercontent.com/74168188/179044637-1161f48d-a784-41e9-99a1-9d40dce2f45f.png)

![image](https://user-images.githubusercontent.com/74168188/179125860-de81fa46-51b6-477f-8563-85a0d939ab91.png)

![image](https://user-images.githubusercontent.com/74168188/179045200-037f22f1-91fe-4cc0-ae70-77c72a5e072a.png)

Next, let's enable the file by creating a link from sites-available to the sites-enabled directory, which Nginx reads from during startup.

![image](https://user-images.githubusercontent.com/74168188/179126554-5adf5845-d20a-4100-ad6e-9dc74892e765.png)

![image](https://user-images.githubusercontent.com/74168188/179126621-bc827b7c-b79a-4dbe-a809-f49c01d26f99.png)

To check that there are no syntax errors in any of your Nginx files, use ```sudo nginx -t```

![image](https://user-images.githubusercontent.com/74168188/179126740-72fb0ed4-41c5-4b2e-b16b-4390d197ea48.png)

Time to restart Nginx server using ```sudo systemctl restart nginx```

![image](https://user-images.githubusercontent.com/74168188/179126851-cd5f65ad-1e79-46f1-9211-4c1bebea3b92.png)

Now, let's try to hit mannan18.ml and test1354.ml

![image](https://user-images.githubusercontent.com/74168188/179131131-0cab86eb-881f-4d09-a5da-120b4ca7ba7c.png)

![image](https://user-images.githubusercontent.com/74168188/179131157-9da99fa1-6895-4715-a879-95b0c8e40949.png)

#### Step-4: Create an elastic IP and attach it to an EC2 instance

For this, visit the AWS management console, navigate to the search box, type elastic IP, and then click Elastic IPs. It will redirect to a new page. Click on Allocate Elastic IP address to create an elastic IP.

![Img-19](https://user-images.githubusercontent.com/74168188/178556817-a34dcba0-8497-4ad4-a88b-36139c7116f8.png)

![Img-20](https://user-images.githubusercontent.com/74168188/178556839-91868ea4-e9c4-4beb-8c4f-1f590c444b76.png)

Click on Allocate to create an elastic IP.

Now, we have to attach this elastic IP to an EC2 instance. Select the unallocated elastic IP, click on Actions, then Associate Elastic IP address. Now, select the instance to attach with elastic IP, then select the private IP of the instance.

![Img-21](https://user-images.githubusercontent.com/74168188/178556870-09ea2eac-f67e-4fad-84d5-4843fdf966a4.png)

![Img-22](https://user-images.githubusercontent.com/74168188/178556894-94ca38f1-dfa3-4ffe-866b-b07465d8019d.png)

Now, hit the elastic IP to see the sample index.html page.

![Img-23](https://user-images.githubusercontent.com/74168188/178556932-47dec8e0-5474-4aed-80cc-25454fe112fc.png)

#### Step-5: Configure domain and apply SSL

Purchase a free domain from freenom.com. For this, search any domain and click select, then checkout and complete the order.

![Img-24](https://user-images.githubusercontent.com/74168188/178556964-db9c96e3-443e-4be7-88d1-0b450c5feb7b.png)
![Img-25](https://user-images.githubusercontent.com/74168188/178556984-c7d8378f-e7b4-452b-a4f1-3a24d90cd58b.png)
![Img-26](https://user-images.githubusercontent.com/74168188/178557005-4c881e54-da28-4ab4-8c7b-5b28939de62e.png)

Now, select My Domains inside Services.

![image](https://user-images.githubusercontent.com/74168188/178632468-d7ba16f6-37db-441a-8376-3aef7e3f6296.png)

Click the manage button of the domain that you want to configure.

![Img-29](https://user-images.githubusercontent.com/74168188/178655181-8b5d6421-d8b6-4f6c-a862-3ff0e7b3df05.png)

Now, click Manage Freenom DNS and add records, then click save changes.

![Img-30](https://user-images.githubusercontent.com/74168188/178658491-46e2084f-b0d6-47f6-a1ae-f420d8ee886b.png)

Similarly, i purchased and configured DNS for test1354.ml

To obtain an SSL certificate, we must install certbot software and the Nginx plugin.

```
sudo apt install certbot python3-certbot-nginx -y
```
![Img-40](https://user-images.githubusercontent.com/74168188/178668761-72b5f4fe-0041-4bd7-8042-2ea92eb1c9c4.png)

To obtain a certificate for a domain.

```
sudo certbot --nginx -d mannan18.ml -d www.mannan18.ml
```
It will prompt you to enter your email address and agree to the terms of service. 

![Img-41](https://user-images.githubusercontent.com/74168188/178669321-56dc0c19-796f-49f8-a3de-28f6ce15f7c0.png)
![Img-42](https://user-images.githubusercontent.com/74168188/178670579-be14977f-85d4-44dc-8c9a-007b30a71406.png)

As you see, certificate is saved at /etc/letsencrypt/live/mannan18.ml/fullchain.pem and key is saved at /etc/letsencrypt/live/mannan18.ml/privkey.pem

Similarly, i obtain a certificate for test1354.ml

Also, Certbot has set up a scheduled task to renew the certificate in the background automatically.

#### Step-6: Configure automation script for SSL renewal

Certbot automatically creates the script to renew the certificate twice a day. This script is located at /etc/cron.d/certbot

![Img-43](https://user-images.githubusercontent.com/74168188/178673034-451825ba-7238-4266-85da-2ee782a6552c.png)

#### Step-7: Migrate Freenom DNS to Route53

Firstly, we must create a hosted zone using the Route53 service of AWS.

![Img-47](https://user-images.githubusercontent.com/74168188/178700658-ba293416-af6e-4deb-aae0-9a328b9acd1c.png)

Fill in the domain name you want to route traffic, then click create a hosted zone.

![Img-48](https://user-images.githubusercontent.com/74168188/178701549-a95ca56b-1f4b-40d0-8a3a-775267ee18da.png)

![Img-49](https://user-images.githubusercontent.com/74168188/178701591-13e43594-3576-480f-98f8-93d6647fc3cb.png)

A hosted zone is now created. Create a record to route traffic to the IP address

![Img-50](https://user-images.githubusercontent.com/74168188/178714826-fa7dee3d-7def-414b-b8bf-971e5d957207.png)

![Img-51](https://user-images.githubusercontent.com/74168188/178715359-23008800-551d-4c18-8c7d-829d3f52d257.png)

![Img-52](https://user-images.githubusercontent.com/74168188/178716697-60d42097-7528-462c-b536-468e3a0fe222.png)

Now, copy the route53 nameservers from records and put in nameservers records of freenom, and click change nameservers.

![Img-53](https://user-images.githubusercontent.com/74168188/178716887-35330c14-be1e-4af9-9295-b5740e9eb422.png)

![Img-54](https://user-images.githubusercontent.com/74168188/178717035-d955e9f1-5c2e-4ca6-9549-535be46b7778.png)

Also, we have to create a new record.

![Img-55](https://user-images.githubusercontent.com/74168188/178740756-cba9fe89-e51e-4910-a8fb-709d8201e55e.png)

![Img-56](https://user-images.githubusercontent.com/74168188/178740922-09ef362b-a0d4-4442-9d14-99c28d44d9ac.png)

Now, all records will look as shown below.

![Img-57](https://user-images.githubusercontent.com/74168188/178743523-ba88a969-ce14-4d3a-a2df-9759fa605e0c.png)

Similarly, i created another hosted zone for test1354.ml and also created record to route traffic to IP address. Then, copied the route53 nameservers from records and put in nameservers records of freenom.

After hitting, mannan18.ml or www.mannan18.ml and test1354.ml or www.test1354.ml we are getting the website with valid certificate.

![Img-58](https://user-images.githubusercontent.com/74168188/178744190-63aae10e-4dc3-42d7-a299-277c4f648e39.png)

![image](https://user-images.githubusercontent.com/74168188/179133162-678b3e4c-f0d6-468b-89ce-2e2c470b4db4.png)

So, we have successfully done the multiple hosting, SSL, and migration to Route53 part.
