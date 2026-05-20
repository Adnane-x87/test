# 🚀 Déploiement d’une application Laravel sur Ubuntu

## 📦 1. Mise à jour du système

```bash
sudo apt update
sudo apt upgrade
```

---

## 🐘 2. Installation de PHP et extensions

```bash
sudo apt install php php-cli php-mbstring php-xml php-bcmath php-curl php-mysql php-sqlite3 unzip curl
```

---

## 🎼 3. Installation de Composer

```bash
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### Vérification

```bash
composer -v
```

---

## 🗄️ 4. Installation de MySQL

```bash
sudo apt install mysql-server
```

---

## 🌱 5. Création du projet Laravel

```bash
composer create-project laravel/laravel hello_world
```

---

## 📂 6. Accéder au projet

```bash
cd hello_world
```

---

## ▶️ 7. Test du serveur Laravel

```bash
php artisan serve
```

### Accès

```text
http://127.0.0.1:8000
```

---

# 🌐 Déploiement avec Apache

## 📁 8. Déplacement du projet

```bash
sudo mv ~/hello_world /var/www/
```

---

## 🔐 9. Permissions

```bash
sudo chown -R www-data:www-data /var/www/hello_world
sudo chmod -R 755 /var/www/hello_world
sudo chmod -R 775 /var/www/hello_world/storage
sudo chmod -R 775 /var/www/hello_world/bootstrap/cache
```

---

## ⚙️ 10. Configuration Apache

### Ouvrir le fichier VirtualHost

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

### Contenu

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost

    DocumentRoot /var/www/hello_world/public

    <Directory /var/www/hello_world/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

---

## 🔧 11. Activer les modules Apache

### Rewrite module

```bash
sudo a2enmod rewrite
```

### PHP module

```bash
sudo apt install libapache2-mod-php
sudo a2enmod php8.5
```

---

## 🔄 12. Redémarrer Apache

```bash
sudo systemctl restart apache2
```

### Vérification

```bash
sudo systemctl status apache2
```

---

# 🗄️ Configuration MySQL

## 13. Créer la base de données

```bash
sudo mysql
```

### Dans MySQL

```sql
CREATE DATABASE hello_world;
EXIT;
```

---

## ⚙️ 14. Configuration du fichier .env

```bash
sudo nano .env
```

### Configuration MySQL

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=hello_world
DB_USERNAME=root
DB_PASSWORD=
```

### Sessions et cache

```env
SESSION_DRIVER=file
CACHE_STORE=file
```

---

## 🧹 15. Nettoyage du cache Laravel

```bash
rm -rf bootstrap/cache/*.php
php artisan config:clear
```

---

# 🛠️ Résolution des erreurs

## ❌ Erreur SQLite readonly

### Cause

Laravel utilisait SQLite avec des permissions insuffisantes.

### Solution

```bash
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache

sudo mkdir -p storage/framework/{sessions,views,cache}
sudo chmod -R 775 storage/framework

sudo rm -rf storage/framework/views/*
```

---

## ❌ Apache ne démarre pas

### Cause

Le port 80 était déjà utilisé par Nginx.

### Solution

```bash
sudo systemctl stop nginx
sudo systemctl restart apache2
```

---

## ❌ Apache affiche le code PHP

### Cause

Le module PHP Apache n’était pas installé.

### Solution

```bash
sudo apt install libapache2-mod-php
sudo a2enmod php8.5
sudo systemctl restart apache2
```

---

# 🌍 Accès à l’application

## Depuis Ubuntu

```text
http://localhost
```

---

## Depuis Windows (machine hôte)

### Vérifier l’adresse IP

```bash
ip a
```

### Exemple IP

```text
192.168.32.128
```

### Accès navigateur

```text
http://192.168.32.128
```

---

# 🎉 Résultat final

✅ Application Laravel fonctionnelle  
✅ Déployée sur Apache  
✅ Accessible via navigateur  
✅ Communication VM ↔ Host réussie  
✅ MySQL configuré correctement  
✅ Permissions Laravel corrigées

---

# 🧠 Conclusion

Ce lab m’a permis de :

- Installer Laravel sur Ubuntu
- Configurer Apache et PHP
- Connecter Laravel à MySQL
- Résoudre les erreurs SQLite
- Corriger les permissions Laravel
- Déployer une application web accessible sur le réseau local

---

# 🚀 Fin du Lab
