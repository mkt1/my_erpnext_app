## My Erpnext App

My Erpnext App as ERPNext plugin

### License

gpl-3.0

## Some notes how to admin ERPNext

### Startup with simple multi container setup

Get it:

    git clone https://github.com/frappe/frappe_docker.git
    cd frappe_docker

Startup ERPNext:

    docker compose -f pwd.yml up -d

Login to https://localhost:8080 :

    # user: Administrator ; password: admin

### Startup with devcontainer

The repository directory ```<foo>/frappe_docker```and ```<foo>/my_erpnext_app``` need to be in the same directory!

    cd <foo>/frappe_docker
    ../my_erpnext_app/tools/frappe_docker-cleanrepository.sh
    ../my_erpnext_app/tools/frappe_docker-prepare-devcontainer.sh

Now open ```frappe_docker``` folder in VS Code and reopen in devcontainer.
To reinstall and start the bench, call and follow instructions. DB Password is ```123```

    ./frappe_docker-reinstall.sh

Go to http://d-code.localhost:8000

### Install/update the plugin in ERPNext

Connect to the container where ERPNext (the bench) is running:

    docker exec -it frappe_docker-frontend-1 /bin/bash
    cd /home/frappe/frappe-bench

<table><tr>
<th>New</th>
<th>Get</th>
<th>Update</th>
</tr><tr>
<td>

    bench new-app my_erpnext_app

</td><td>

    bench get-app my_erpnext_app https://github.com/mkt1/my_erpnext_app.git

</td><td>

    cd my_erpnext_app
    git pull

</td></tr></table>

The directory ```frappe-bench/apps/my_erpnext_app``` contains all files of the application.



Now install and restart:

    # Not necessary for update
    bench install-app my_erpnext_app
    
    # Migrate and update and restart ERPNext
    bench migrate
    bench clear-cache
    bench restart

To uninstall:

    bench uninstall-app my_erpnext_app
    bench remove-app my_erpnext_app

### Docker ps aliases for convenience

    alias dpsi='docker ps --format "table ☞   {{.ID}}\t{{.Image}}\t{{.Status}}\t{{.Names}}" | (read -r; printf "%s\n" "$REPLY"; sort -k 2 )'
    alias dpsn='docker ps --format "table ☞   {{.ID}}\t{{.Image}}\t{{.Status}}\t{{.Names}}" | (read -r; printf "%s\n" "$REPLY"; sort -k 4 )'
    alias dils='docker image ls --format "table ☞   {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.CreatedSince}}\t{{.Size}}"'
    alias dvls='docker volume ls --format "table ☞   {{.Name}}"'
    alias dall='echo "Containers:" ; dpsn ; echo "Images:" ; dils ; echo "Volumes:" ; dvls'

