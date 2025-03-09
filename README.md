# This will deploy a PHP website on Kubernetes cluster. This will use Apache as a web server and Mysql for database.

## 1. php-config file will be created to store configuration data for php.ini. php.ini: This is the PHP configuration file that contains various settings to control the behavior of PHP. In this case, it contains the setting variables_order.
'kubectl apply -f php-config.yaml'

### variables_order = "EGPCS": This is a configuration directive for PHP that defines the order in which PHP registers the environment variables (such as $_GET, $_POST, $_COOKIE, $_FILES, and $_SERVER).
Explanation of "EGPCS":
This value is a string of characters representing the order in which different PHP superglobals are populated with data:

E = Environment variables ($_ENV)
G = GET variables ($_GET)
P = POST variables ($_POST)
C = COOKIE variables ($_COOKIE)
S = SERVER variables ($_SERVER)

## 2. Apply secret.yaml This secret stores MySQL credentials (values are base64-encoded).
'kubectl apply -f secret.yaml'

### Kubernetes Secrets 
Kubernetes Secrets are used to store and manage sensitive data such as passwords, API keys, certificates, and other sensitive information. They allow you to securely store and access sensitive information that should not be stored in plain text within your Kubernetes resources.

## 3. Deployment (lamp-wp.yaml). This creates the lamp-wp deployment with two containers:
httpd-php-container (PHP + Apache)
mysql-container (MySQL)

'kubectl apply -f deployment.yaml'

### In this deployment env values are used and will use the secret values. By using env it can be mentioned to each specific values for example, MYSQL_PASSWORD. And /app is mounted as emptyDir so that we can change config in the pod instead of loggin in te container directly. /app is Apache document root. Also php-config configmap is mounted as volume in this deployment.

## 4. This exposes Apache on using LoadBalancer external IP

'kubectl apply -f lamp-service.yaml'

## 5. This allows internal communication to MySQL.

'kubectl apply -f mysql-service.yaml'

## You can check the below command if the httpd container is accessing the mysql server.

'kubectl exec -it lamp-wp-57865dd75b-dm7wx -c httpd-php-container -- curl http://localhost:80'

## You can also check from the browser using the external-IP of lamp-service.

`http://<External-IP>/index.php`
