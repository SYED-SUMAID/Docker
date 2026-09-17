# 🐳 Docker Bind Mount with Apache

A bind mount allows a directory from the **host machine** to be connected directly to a directory inside a Docker container.

This means that when we modify a file on the host, the change is immediately visible inside the container.

## 🎯 Objective

In this lab, we will:

- Create an `index.html` file on the host.
- Mount the host directory into an Apache container.
- Serve the HTML file through Apache.
- Modify the HTML file **on the host**.
- Verify that the change is immediately reflected inside the container.

---

## 1. Create a Host Directory

Create a directory on the host to store the website files.

    mkdir  ~/apache-bind-mount

---

## 2. Create the HTML File on the Host

Create a simple `index.html` file.

    echo "<h1>Hello from Docker Bind Mount</h1>" > ~/apache-bind-mount/index.html

Check the file:

    cat ~/apache-bind-mount/index.html

![alt text](<Screenshot 2026-09-17 182317.png>)

---

## 3. Run Apache with a Bind Mount

Start an Apache container and mount the host directory into Apache's web root.

    sudo docker run -d --name apache-bind-container -p 8080:80 -v ~/apache-bind-mount:/usr/local/apache2/htdocs httpd

### Command Breakdown

| Option | Meaning |
|---|---|
| `-d` | Run the container in detached mode |
| `--name apache-bind-container` | Give the container a name |
| `-p 8080:80` | Map host port `8080` to container port `80` |
| `-v` | Create a bind mount |
| `~/apache-bind-mount` | Directory on the host |
| `/usr/local/apache2/htdocs` | Apache web root inside the container |
| `httpd` | Apache Docker image |

---

## 4. Check the Running Container

    sudo docker ps

![alt text](<Screenshot (788)(1).png>)

---

## 5. Open the Website

Open the following in your browser:

    http://localhost:8080

You should see:

**Hello from Docker Bind Mount**

![alt text](<Screenshot (789)(1).png>)

---

## 6. Modify the HTML File on the Host

This is the important part of the bind mount demonstration.

Edit the HTML file **on the host machine**:

    nano ~/apache-bind-mount/index.html

Change it to something like:

    <h1>Hello from My Updated Website</h1>
    <p>This change was made on the host.</p>

Save the file:

- `Ctrl + O`
- Press `Enter`
- `Ctrl + X`

Check the updated file on the host:

    cat ~/apache-bind-mount/index.html

![alt text](<Screenshot 2026-09-17 195014(1).png>)

![alt text](<Screenshot (792)-1.png>)


---

## 7. Refresh the Website

Go back to:

    http://localhost:8080

Refresh the page.

The updated content  appeared immediately.

No container rebuild or restart is required.

![alt text](<Screenshot (790).png>)

---

## 8. Verify the Change Inside the Container

Now verify that the container can see the same file.

    sudo docker exec apache-bind-container cat /usr/local/apache2/htdocs/index.html

The output should contain the changes that were made to the file on the host.

![alt text](<Screenshot 2026-09-17 195320.png>)

---

## 🔗 How the Bind Mount Works

The host directory:

    ~/apache-bind-mount

is mounted to:

    /usr/local/apache2/htdocs

inside the container.

So the same file is accessible through both locations:

    HOST
    ~/apache-bind-mount/index.html
             |
             | Bind Mount
             ↓
    CONTAINER
    /usr/local/apache2/htdocs/index.html

When the host file changes:

    Host file changed
           ↓
    Bind mount reflects the change
           ↓
    Container sees the updated file
           ↓
    Apache serves the updated page

---


## 🔑 Key Takeaways

- A **bind mount** connects a host directory directly to a directory inside a container.
- Changes made to files on the host are immediately visible inside the container.
- The container does not need to be rebuilt or restarted.
- For Apache, `/usr/local/apache2/htdocs` is the web root.
- The `docker exec` command can be used to verify the host changes from inside the container.
- This is useful during development because website files can be edited directly on the host while Apache runs inside Docker.