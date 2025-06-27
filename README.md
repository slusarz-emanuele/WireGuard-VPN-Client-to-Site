# Configurazione di una VPN Remote Access (Client-to-Site) con WireGuard

Per configurare una VPN di tipo **Remote Access VPN (client-to-site)** mediante WireGuard è necessario attenersi alla seguente guida.

---

## Occorrente

* MACCHINA SERVER
* ROUTER
* CLIENTS

---

## MACCHINA SERVER

È necessario usare un **IP statico**.
Per impostarlo è sufficiente modificarlo dal pannello di controllo del sistema operativo.

### Screenshot:

![Pannello di controllo](/control_panel_images/01.png)
![Pannello di controllo](/control_panel_images/02.png)
![Pannello di controllo](/control_panel_images/03.png)
![Pannello di controllo](/control_panel_images/04.png)
![Pannello di controllo](/control_panel_images/05.png)
![Pannello di controllo](/control_panel_images/06.png)

In questa maniera l'IP della macchina server è statico.

---

## Installazione di WireGuard sul Server

Procedere con l'installazione di **WireGuard** sulla macchina server.
L'installer è lo stesso sia per il client che per il server.

[Scarica WireGuard](https://www.wireguard.com/install/)

Proseguire con l'installazione guidata lasciando le opzioni di default.

---

## Installazione di WireGuard sul Client

Anche sulla macchina client è necessario installare WireGuard:

[Scarica WireGuard](https://www.wireguard.com/install/)

L'installazione è identica a quella vista per il server.

---

## Configurazione del Router (Port Forwarding)

Una volta completate le installazioni, è necessario modificare le impostazioni del router per abilitare il port forwarding.

Accedere al pannello del router e nella sezione **Port Forwarding** configurare quanto segue:

* IP del server VPN: quello statico configurato in precedenza
* Protocollo: UDP
* Porta esterna (del router): 51820
* Porta interna (sul server): 51820

### Esempio delle impostazioni del router (ogni modello è diverso):

![Router](/router_settings/01.png)

---

## Configurazione iniziale di WireGuard

### SERVER

Sulla schermata principale di WireGuard, creare un nuovo file di configurazione:

![WireGuard](/wireguard/01.png)

Una chiave pubblica e una chiave privata saranno generate in automatico da WireGuard:

![WireGuard](/wireguard/02.png)

### CLIENT

Eseguire gli stessi passaggi del server per la creazione di un nuovo file di configurazione.

---

## File di configurazione - SERVER

Nel file di configurazione del server, aggiungere esattamente quanto segue:

![WireGuard](/wireguard/03.png)

```ini
ListenPort = 51820 # Default port di ascolto per WireGuard
Address = 10.8.0.1/24 # IP statico del server nella VPN

[Peer] # Da configurare per ogni singolo peer, in modo che siano ammessi al collegamento
PublicKey = <CHIAVE_PUBBLICA_CLIENT>
AllowedIPs = 10.8.0.2/32 # IP designato al client
```

Cliccare su **Save** per salvare la configurazione.

Successivamente, attivare il tunnel:

![WireGuard](/wireguard/04.png)

---

### Comandi da eseguire sul server (PowerShell, con tunnel attivo)

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" -Name "IPEnableRouter" -Value 1

Set-NetIPInterface -InterfaceAlias "<NOME_TUNNEL>" -Forwarding Enabled
Set-NetIPInterface -InterfaceAlias "Ethernet" -Forwarding Enabled
netsh interface ipv4 add route 10.8.0.0/24 interface="<NOME_TUNNEL>"
```

Disattivare il tunnel e riavviare il server.

---

## File di configurazione - CLIENT

Nel file di configurazione del client, aggiungere le seguenti righe **sotto il blocco `[Interface]` con la chiave privata**:

```ini
Address = 10.8.0.2/32 # IP designato al client

[Peer]
PublicKey = movxq2wKu7X34BvgJWwEBp/9LYshCYS+YGZxpZG+tAU=
AllowedIPs = 10.8.0.0/24
Endpoint = [<IP_ROUTER>](https://whatismyipaddress.com/):51820 # Default port di ascolto per WireGuard
PersistentKeepalive = 25
```

---

## Conclusione

In questo modo tutto è stato configurato e pronto all'uso.

**DISCLAIMER:** le chiavi private e pubbliche mostrate in questo tutorial sono reali ma non sono più in uso.
# Configurazione di una VPN Remote Access (Client-to-Site) con WireGuard

Per configurare una VPN di tipo **Remote Access VPN (client-to-site)** mediante WireGuard è necessario attenersi alla seguente guida.

---

## Occorrente

* MACCHINA SERVER
* ROUTER
* CLIENTS

---

## MACCHINA SERVER

È necessario usare un **IP statico**.
Per impostarlo è sufficiente modificarlo dal pannello di controllo del sistema operativo.

### Screenshot:

![Pannello di controllo](/control_panel_images/01.png)
![Pannello di controllo](/control_panel_images/02.png)
![Pannello di controllo](/control_panel_images/03.png)
![Pannello di controllo](/control_panel_images/04.png)
![Pannello di controllo](/control_panel_images/05.png)
![Pannello di controllo](/control_panel_images/06.png)

In questa maniera l'IP della macchina server è statico.

---

## Installazione di WireGuard sul Server

Procedere con l'installazione di **WireGuard** sulla macchina server.
L'installer è lo stesso sia per il client che per il server.

[Scarica WireGuard](https://www.wireguard.com/install/)

Proseguire con l'installazione guidata lasciando le opzioni di default.

---

## Installazione di WireGuard sul Client

Anche sulla macchina client è necessario installare WireGuard:

[Scarica WireGuard](https://www.wireguard.com/install/)

L'installazione è identica a quella vista per il server.

---

## Configurazione del Router (Port Forwarding)

Una volta completate le installazioni, è necessario modificare le impostazioni del router per abilitare il port forwarding.

Accedere al pannello del router e nella sezione **Port Forwarding** configurare quanto segue:

* IP del server VPN: quello statico configurato in precedenza
* Protocollo: UDP
* Porta esterna (del router): 51820
* Porta interna (sul server): 51820

### Esempio delle impostazioni del router (ogni modello è diverso):

![Router](/router_settings/01.png)

---

## Configurazione iniziale di WireGuard

### SERVER

Sulla schermata principale di WireGuard, creare un nuovo file di configurazione:

![WireGuard](/wireguard/01.png)

Una chiave pubblica e una chiave privata saranno generate in automatico da WireGuard:

![WireGuard](/wireguard/02.png)

### CLIENT

Eseguire gli stessi passaggi del server per la creazione di un nuovo file di configurazione.

---

## File di configurazione - SERVER

Nel file di configurazione del server, aggiungere esattamente quanto segue:

![WireGuard](/wireguard/03.png)

```ini
ListenPort = 51820 # Default port di ascolto per WireGuard
Address = 10.8.0.1/24 # IP statico del server nella VPN

[Peer] # Da configurare per ogni singolo peer, in modo che siano ammessi al collegamento
PublicKey = <CHIAVE_PUBBLICA_CLIENT>
AllowedIPs = 10.8.0.2/32 # IP designato al client
```

Cliccare su **Save** per salvare la configurazione.

Successivamente, attivare il tunnel:

![WireGuard](/wireguard/04.png)

---

### Comandi da eseguire sul server (PowerShell, con tunnel attivo)

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" -Name "IPEnableRouter" -Value 1

Set-NetIPInterface -InterfaceAlias "<NOME_TUNNEL>" -Forwarding Enabled
Set-NetIPInterface -InterfaceAlias "Ethernet" -Forwarding Enabled
netsh interface ipv4 add route 10.8.0.0/24 interface="<NOME_TUNNEL>"
```

Disattivare il tunnel e riavviare il server.

---

## File di configurazione - CLIENT

Nel file di configurazione del client, aggiungere le seguenti righe **sotto il blocco `[Interface]` con la chiave privata**:

```ini
Address = 10.8.0.2/32 # IP designato al client

[Peer]
PublicKey = movxq2wKu7X34BvgJWwEBp/9LYshCYS+YGZxpZG+tAU=
AllowedIPs = 10.8.0.0/24
Endpoint = [<IP_ROUTER>](https://whatismyipaddress.com/):51820 # Default port di ascolto per WireGuard
PersistentKeepalive = 25
```

---

## Conclusione

In questo modo tutto è stato configurato e pronto all'uso.

**DISCLAIMER:** le chiavi private e pubbliche mostrate in questo tutorial sono reali ma non sono più in uso.
