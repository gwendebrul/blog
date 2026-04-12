# SSH Tunnels

![Header image](images/header.jpg)

## Prologue

At home, I have set up a **Mac mini** as a **file HTTP server**, which also hosts **gitweb** and **developer documentation** that I can access (at least within my home network) if needed even if the internet goes down.

Now, as long as the internet is working normally, I also want access from outside my home. I did some research, and **Tailscale** is the easiest solution. However, it’s an external service, and I prefer to keep everything under my own control, plus the learning experience matters too ;-)

Eventually, I ended up with **SSH tunnels**, and they work as I hoped. It’s a bit more cumbersome than **Tailscale**, but I retain full control.

And the biggest pro is you don't need to adjust your firewall at home.

## What do you need

To access your **local home server** from outside your network, you need an intermediate **computer** or **server** that you can access from anywhere. In my case, I rent hosting from a Dutch company, and that server acts as a gateway.

You also need a **device** to log into the gateway **server** from a remote location. This can be a **laptop**, **smartphone**, or **tablet**. In my case, I use an **iPad** with the **Termius** app.

Additionally, your local **computer/server** must be able to establish an SSH connection to the gateway **server**, preferably using **SSH keys**.

Finally, you also need **web hosting** with **SSH access**, otherwise this setup won’t work.

## Creating a reverse tunnel

On your local **computer** or **server**, you need to create a **reverse tunnel** to your public **server** (the gateway). You can do this with the following command:

```
ssh -R localhost:8888:localhost:8080 John@example.com
```

Here, the first `localhost:8888` is the port on your gateway **server**. The second `localhost:8080` is the port on your local **computer/server**. Using `localhost` allows you to simply type `localhost:8888` in the browser on your **iPad**, and you’ll see your local **website** in my case, the portal.

From there, I can view the **file HTTP server** and **developer documentation** as if I were at home.

The last part is the gateway **server** address and username, which is used to log in to the gateway.

## Creating a local tunnel

Next, on a **computer** you use outside your home, you need to create a **local tunnel** to the gateway **server**. This is done with a similar command:

```
ssh -L localhost:8888:localhost:8888 john@example.com
```

Note the `-L`, which indicates a local tunnel. If everything is configured correctly, you can simply type `localhost:8888` in your browser and access the website on your local **computer/server**.

## Explanation about ports

On my local **computer/server**, I chose port 8080 for the web server. Normally, if you want to use port 80 on your gateway **server**, you need to modify the **sshd_config** file.

Specifically, you must set **GatewayPorts** to **yes** instead of **no**. Since I cannot change this setting on my hosting provider, I opted to use a different port.

## Termius app on iPadOS

On my **iPad**, I use the **Termius** app to set up port forwarding.

First, you need to create an **SSH key** and add the public key to the **authorized_keys** file on the gateway **server**. Once that’s done, you can create the connection.

![Termius Key creation](images/termius-1.jpg)
![Termius Key creation 2](images/termius-1b.jpg)

Under “Hosts,” create a connection to the gateway **server**. Fill in all the necessary details to establish an SSH connection. Instead of a password, select the **SSH key** you created. Also enter the IP address or hostname and choose SSH as the connection type.

![Termius Host setup](images/termius-2.jpg)

Once configured, test whether you can log in to the gateway **server** via SSH from your **iPad**. If it works, you’ll see a **command prompt** from your **server**.

Now, you just need to configure **port forwarding** in **Termius**. Enter the correct details:

* **Bind address**: `localhost`
* **Port**: `8888`
* **Intermediate host**: your username and server address

```
john@example.com:22
```

![Termius Port Forwarding setup](images/termius-3.jpg)

If everything is set correctly, click the newly created **local forwarding** entry, then open your browser and go to:

```
http://localhost:8888
```

And voilà—you now have access to a website on your local **computer/server** from outside your home.

## Epilogue

You can also use this setup for **remote desktop** (VNC). Just configure the correct port and use a **VNC** app on your **iPad** to control your local **computer**.

If, like me, you use a local **Mac computer** and a **Mac laptop** outside your home, you don’t need any extra app, this functionality is built into **macOS**. You just need to set up the correct **SSH tunnels**.
