🇺🇸 [English](README.md) | 🇧🇷 [Português](README.pt-BR.md)

# VPN-configuration-EC2

Setting up a free VPN (Pritunl) on an Ubuntu EC2 instance.

## Step 1: Install the VPN (Pritunl)

- With the EC2 instance created and running Ubuntu 20.04 or 22.04, use the root user to run the commands from the Pritunl documentation.
- Go to https://pritunl.com/, scroll down, and find your Ubuntu version. Run all the commands through to the end, then use `systemctl status pritunl` to check that the service is running.

## Step 2: Configure the VPN

- In the EC2 console, go to your VPN instance's security group and open ports 80 and 443 to public access (`0.0.0.0/0`).
- Copy your instance's public IPv4 address and paste it into your browser to access it. On first access, a command will be shown that lets you retrieve the VPN admin username and password.
- Copy that command, run it on your EC2 instance, then enter the username and password in the Pritunl login screen.
- In the Pritunl dashboard, go to **Servers** > **Add Server** > enter your server name, clear the **DNS Server** field > click **Advanced** and uncheck all checked boxes. Click **Add**.
- A panel with some server information will open. Below it there will be a `0.0.0.0/0` route; click **Remove Route**.
- Next to the panel there is a **Port** field with a UDP port number. Copy that number, go to your VPN EC2 instance's security group, create an inbound rule of type Custom UDP, paste the port number, and allow `0.0.0.0/0` (public access).
- Back in the Pritunl dashboard, open the **Users** tab and click **Add Organization**. Name the organization and click **Add**.
- Return to the **Servers** tab and, in your server's panel, click **Attach Organization** and select the organization created in the previous step.
- Click **Add Route** and enter your AWS VPC's CIDR block. You can find it under VPC > Your VPCs > select the VPC you are using and check its **IPv4 CIDR**. Copy the IP range and go back to the Pritunl dashboard to add it under **Add Route**.
- Click **Start Server**, and the VPN will be running on your network.

## Step 3: Create a User and Connect with the Client

- In the Pritunl dashboard, go to **Users** and click **Add User**.
- Name the user, select the organization you created, and set a PIN (numbers only). Click **Add**.
- Select the user you created and click **Click to Download Profile**. A `.tar` file will be downloaded to your computer.
- Download the Pritunl client from https://client.pritunl.com/ and install it.
- With the client open, click **Import** > **Import Profile** > **Browse** and select the user's `.tar` file you downloaded.
- Click **Connect** and enter the PIN.
- You are now connected to the VPN and can reach servers in the VPC network through their private IPs.
