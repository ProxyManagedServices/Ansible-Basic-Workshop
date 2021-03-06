# Lab 7: Extra - Linux Server

In dit lab gaan we een andere Linux server configureren. Alle ansible acties voer je uit op de du-ans-XXa, en de linux server die we gaan configureren is de du-ans-XXb. 

Linux is het Operating System waarvoor verreweg de meeste Ansible modules te vinden zijn. Voor vrijwel elke uitdaging is wel een Ansible module (of role) te vinden. In dit lab gaan we in 3 stappen een webserver configureren én zelfs de web content installeren. 

## Task 7.1: Inventory aanpassen

Voer deze task uit op je Client die inmiddels ansible server is: du-ans-XXa. We gaan acties uitvoeren op en andere client genaamd du-ans-XXb

* Log in op je Client.

  ``$ ssh -l userXX <hostname>`` 

* Als het goed is log je direct in (zonder wachtwoord):

  ``` 
  [user01@du-ans-09a ~]$ 
  ```

* Maak een inventory file:

  ``$ vi inventory``

* Vul de inventory file met (vervang ``<hostname>`` door de hostname van je B client, bijvoorbeeld ansible_host=du-ans-09b.westeurope.cloudapp.azure.com):

  ```
  [linux]
  linux-XXb ansible_host=<hostname>
  ```
  
* Maak een ansible.cfg aan:

  ``$ vi ansible.cfg``

* Vul de ansible.cfg met:

  ```
  [defaults]
  inventory = ~/inventory
  remote_user = userXX
  
  host_key_checking = False
  ```

## Task 7.2: Verbinding testen

Om zeker te zijn dat de inventory file ``inventory`` en de config file ``ansible.cfg`` correct zijn, voeren we een test uit met de ``adhoc`` module ``ping``. Ansible vraagt om een wachtwoord. Gebruik hiervoor het wachtwoord van je user account ``userXX``.

* Test de verbinding met de module ``ping``

  ``$ ansible -m ping linux --ask-pass``
  
  ```
  linux-09b | SUCCESS => {
    "changed": false,
    "ping": "pong"
  }
  ```
  
## Task 7.3: Apache installeren

De eerste stap is de webserver software installeren. Na installatie moet de webserver natuurlijk gestart worden. Beide acties voeren we uit in het onderstaande playbook.

* Maak het playbook ``linux.yml``:

  ```
  ---
  - hosts: linux
    become: true
    become_method: sudo

    tasks:
      - name: Install apache packages
        yum:
          name: httpd

      - name: ensure httpd is running
        service:
          name: httpd 
          state: started
  ```

* Voer het playbook uit:

  ``$ ansible-playbook linux.yml --ask-pass``
  
## Task 7.4: Firewall configureren

Om de webserver goed te laten werken, dient poort 80 (http) open gezet te worden. Hiervoor gebruiken we de Ansible module ``firewalld``. Om er zeker van te zijn dat de nieuwe regel ingelezen wordt, herstarten we ``firewalld`` na de wijziging. 
 
* Pas het playbook ``linux.yml`` aan:
 
  ```
      - name: Ensure port 80 is open for http access
        firewalld:
          service: http
          permanent: true
          state: enabled

      - name: Ensure firewalld service is restarted after the firewall changes
        service: 
          name: firewalld 
          state: restarted
   ```

* Voer het playbook uit:

  ``$ ansible-playbook linux.yml -k``
   
## Task 7.5: Installeer content voor de webserver

Met Ansible kun je eenvoudig content kopieën van je Ansible Engine naar de webserver. In dit voorbeeld installeren we een index.html, welke je daarna via de browser op kunt vragen.

* Maak de directory ``files`` aan:
  
  ``$ mkdir files``
  
* Maak een HTML file aan met content (in de directory files):

  ``$ vi files/index.html``
  
  ```
  <html>
    <body>
      <h1>Hello world</h1>
    </body>
  </html>
  ```
  
* Pas het playbook ``linux.yml`` aan:

  ```
      - name: Ensure content is installed in the webserver
        copy:
          content: "Hello World! This is the Ansible Basic Workshop Extra Lab on the {{ ansible_facts.hostname }} Linux Server\n"
          dest: /var/www/html/index.html
          owner: apache
          group: apache
          mode: 0644
  ```
    
* Voer het playbook uit:

  ``$ ansible-playbook linux.yml -k``
  
* Open in je browser de url ``http://<hostname>.westeurope.cloudapp.azure.com`` (vervang ``<hostname>`` door het de hostname van de Linux server).
