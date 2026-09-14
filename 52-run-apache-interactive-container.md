# Run Apache Interactive Terminal in Container

## 1. Run Ubuntu Container

    sudo docker run -it ubuntu /bin/bash

Starts an Ubuntu container with an interactive Bash terminal.

![alt text](<Screenshot 2026-09-14 215450.png>)

## 2. Update Package Lists

    apt update

Updates the package lists inside the container.

![alt text](<Screenshot 2026-09-14 215644.png>)

## 3. Install Apache

    apt install apache2 -y

![alt text](<Screenshot (752).png>)

## 4. Start Apache

    service apache2 start

Starts the Apache web server.

![alt text](<Screenshot 2026-09-14 220534.png>)

## 5. Check Apache Status

    service apache2 status

Displays the current Apache service status.

![alt text](<Screenshot 2026-09-14 220618(1).png>)

## 6. Check Apache Process

    ps aux | grep apache2

Shows the Apache processes running inside the container.

![alt text](<Screenshot 2026-09-14 162957.png>)

## 7. Test Apache

    curl localhost

Sends a request to Apache running inside the container and displays the response.

![alt text](<Screenshot 2026-09-14 221539(1).png>)

## 8. Exit the Container

    exit

Exits the interactive terminal.

![alt text](<Screenshot 2026-09-14 222117(1).png>)