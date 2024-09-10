# NestJS Project

This guide provides a comprehensive step-by-step process for setting up a NestJS project, integrating it with GitHub, and deploying it to an AWS EC2 instance. The project follows best practices and aims to ensure a scalable, maintainable, and secure backend application.

## Table of Contents
1. [Project Setup](#1-project-setup)
2. [GitHub Integration](#2-github-integration)
3. [AWS EC2 Deployment](#3-aws-ec2-deployment)
4. [System Updates and Install Dependencies](#4-system-updates-and-install-dependencies)
5. [Clone the Repository and Install Dependencies](#5-clone-the-repository-and-install-dependencies)
6. [Environment Setup](#6-environment-setup)
7. [Install and Configure Nginx](#7-install-and-configure-nginx)
8. [Install and Configure PM2](#8-install-and-configure-pm2)
9. [Final Steps and Testing](#9-final-steps-and-testing)
10. [Conclusion](#9-conclusion)

## 1. Project Setup

Ensure that you have Node.js installed and the NestJS CLI globally available. Follow the commands below to create a new NestJS project:

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
$ cd project-name
```

Create environment variables .env and .env.example

## 2. GitHub Integration

Version control is essential for project management. Follow the steps below to initialize a Git repository and push your project to GitHub:

```bash
$ git init
$ git remote add origin https://github.com/username/project-name.git
$ git add .
$ git commit -m "Initial commit"
$ git branch -M main
$ git push -u origin main
```

## 3. AWS EC2 Deployment

To deploy your NestJS application, set up an AWS EC2 instance with the following steps:

Launch a New EC2 Instance:

Name the instance appropriately.
* Select Ubuntu as the operating system (Amazon Machine Image).
* Choose the t2.micro instance type for free-tier usage.
* Create or select an SSH key pair for secure access.
* Configure security groups: Ensure that the security group allows incoming SSH (port 22) and HTTP/HTTPS (ports 80/443) traffic.
* Connect to the EC2 Instance using the SSH key:

## 4. System Updates and Install Dependencies

After connecting to the EC2 instance, it’s essential to update the system packages. You can automate this process by creating and running a script directly on your EC2 instance.

### Steps:

* Connect to your EC2 instance via SSH:

```bash
$ ssh -i "your-key.pem" ubuntu@public_ip_address
```

* Once connected, create a new script file using a text editor (e.g., nano):

```bash
$ nano install_dependencies.sh
```

Paste the following content into the editor and save the file (Ctrl+O, then Enter, followed by Ctrl+X to exit):

```bash
#!/bin/bash

# Cambiar a usuario root
if [ "$(whoami)" != "root" ]; then
  echo "Por favor ejecuta este script como root."
  exit 1
fi

# Actualizar y actualizar la lista de paquetes
echo "Actualizando lista de paquetes..."
apt-get update && apt-get upgrade -y

# Instalar curl
echo "Instalando curl..."
apt-get install -y curl

# Agregar el repositorio de Node.js e instalar Node.js
echo "Agregando repositorio de Node.js e instalando Node.js..."
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt-get install -y nodejs

# Agregar la clave GPG de Yarn y el repositorio
echo "Agregando clave GPG y repositorio de Yarn..."
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Actualizar la lista de paquetes e instalar Yarn
echo "Instalando Yarn..."
apt-get update
apt-get install -y yarn

# Verificar las versiones de Node.js y Yarn
echo "Verificando instalación..."
node --version
yarn --version

echo "Instalación completada."
```

* Make the script executable:

```bash
chmod +x install_dependencies.sh
```

* Run the script with sudo to install the necessary dependencies:

```bash
sudo ./install_dependencies.sh
```

## 5. Clone the Repository and Install Dependencies

Now that the system dependencies are installed, the next step is to clone your GitHub repository and prepare your NestJS application for execution.

```bash
$ git clone https://github.com/username/project-name.git
$ cd project-name
$ yarn install
```

## 6. Environment Setup

You will need to set up environment variables using a .env file. If you have a .env.example file, you can copy it to create your .env file:

```bash
$ ls -la
$ cd project-name
$ cp .env.example .env
$ nano .env
```

Inside the file, adjust the values to match your configuration. After editing, save the file by pressing Ctrl + O, then Enter, and exit with Ctrl + X.

## 7. Install and Configure Nginx

To ensure that your NestJS application runs smoothly and can handle production traffic, install and configure Nginx as a reverse proxy along with PM2 for process management.

```bash
$ sudo apt install nginx
$ sudo ufw app list
$ sudo ufw allow 'Nginx HTTP'
$ sudo systemctl start nginx
$ systemctl status nginx
$ sudo systemctl restart nginx
```

## 8. Install and Configure PM2

Install PM2 globally using Yarn:

```bash
$ yarn global add pm2
$ pm2 start dist/main.js --name application_name
```

## 9. Final Steps and Testing

Open your browser or use Postman to test your application by sending a request to your EC2 instance

```bash
$ http://public_ip_address
```

## 10. Conclusion

You have successfully set up, deployed, and tested a scalable NestJS application on an AWS EC2 instance. By following best practices like version control, process management, and using Nginx as a reverse proxy, your application is now ready for production.

Feel free to further improve and expand your project by adding more features or integrating additional services.

## Contact
For any questions or contributions, feel free to reach out:

* **Author**: Luis Carrasco
* **Website**: [my_site.com](https://my_site.com)