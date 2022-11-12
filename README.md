# Shoppy HTB


Primero que nada nos damos cuenta que tiene 3 puerto abiertos

```
nmap -sSV --open -vvv IP
nmap -sC -p 22,80,9093 IP
```

Esto re direcciona a shoppy.htb lo cual nos da indicios de que podria utilizar vhosting

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


























































