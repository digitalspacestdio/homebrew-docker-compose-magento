# Magento 2 Docker Environment

## Optional Pre-requirements (MacOs only)
Configure and start nfs
```bash
echo "$HOME -alldirs -mapall=$UID:20 localhost" | sudo tee -a /etc/exports
echo "nfs.server.mount.require_resv_port = 0" | sudo tee -a /etc/nfs.conf
```

Start the NFS server
```bash
sudo nfsd restart
```

## Usage

Export your composer auth tokens
If you use github only
```bash
export COMPOSER_AUTH='{
    "http-basic": {
        "repo.magento.com": {
            "username": "xxxxxxxxxxxx",
            "password": "yyyyyyyyyyyy"
        }
    },
    "github-oauth": {
        "github.com": "xxxxxxxxxxxx"
    }
}'
````

To use specific php version just export environment variable:
```bash
export PHP_VERSION=7.4
```
> following versions are supported: 7.2, 7.3, 7.4, 8.0

Clone this repo
```bash
git clone git@github.com:digitalspacestdio/docker-env-magento2.git
```

Go to the working dir
```bash
cd docker-env-magento2
```

Clone your code to the `www` folder or create the new project:
```bash
./docker-compose-wrapper run --rm cli composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=^2 /var/www
```

Install dependencies (if needed)
```bash
./docker-compose-wrapper run --rm cli composer install -o --no-interaction
```

Deploy sample data (if needed)
```bash
./docker-compose-wrapper run --rm cli bin/magento sampledata:deploy
```

Install the application
```bash
./docker-compose-wrapper run --rm cli bin/magento setup:install --backend-frontname="admin" --key="admin" --session-save="files" --db-host="database:3306" --db-name="magento2" --db-user="magento2" --db-password="magento2" --base-url="http://localhost:30280/" --base-url-secure="https://localhost:30280/" --admin-user="admin" --admin-password='$ecretPassw0rd' --admin-email="johndoe@example.com" --admin-firstname="John" --admin-lastname="Doe" --key="26765209cb05b93729898c892d18a8dd" --search-engine=elasticsearch7  --elasticsearch-host=elasticsearch --elasticsearch-port=9200
```

Disable 2FA module (if needed)
```bash
./docker-compose-wrapper run --rm cli bin/magento module:disable Magento_TwoFactorAuth
```

Start the stack
```bash
./docker-compose-wrapper up
```

Also you can start the stack in the background mode
```bash
./docker-compose-wrapper up -d
```

> Application should be available by following link: http://localhost:30280/

Stop the stack
```bash
./docker-compose-wrapper down
```

Destroy the whole data
```bash
./docker-compose-wrapper down -v
```
