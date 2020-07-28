# acme.sh


#### Halaman Github
[acme.sh](https://github.com/acmesh-official/acme.sh)

---

#### Instalasi
`curl https://get.acme.sh | sh`

atau 

`wget -O -  https://get.acme.sh | sh`

---

#### Mendapatkan sertifikat
satu domain

`acme.sh --issue -d example.com -w /var/www/html`

multi domain

`acme.sh --issue -d example.com -d www.example.com -d cp.example.com -w /home/wwwroot/example.com`

---

#### Upgrade

manual

`acme.sh --upgrade`

otomatis

`acme.sh --upgrade --auto-upgrade`

menonaktifkan autoupdate

`acme.sh --upgrade --auto-upgrade 0`

---


