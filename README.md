# Domaci streznik
Moj homelab
Kratek pregled tega, kar poganjam doma v prostem času – strežnik, nekaj storitev in domače omrežje.

Zakaj
Rad eksperimentiram s self-hostingom – namesto da za vsako stvar plačujem naročnino v oblaku, raje gostim čim več sam, se ob tem nekaj naučim o Linuxu, omrežjih in kontejnerjih, ter obdržim nadzor nad svojimi podatki.

Strojna oprema
En doma stoječi strežnik (računalnik), ki teče 24/7.
Nekaj TB diskovnega prostora.
Poraba je okrog 40 W v času mirovanja.
Kot router uporabljam starejši računalnik, na katerem teče pfSense. Na njem imam nameščen tudi pfBlocker, ki blokira večino oglasov in nevarnih domen.
Kaj teče na strežniku
Na strežnik sem naložil Proxmox, v katerem imam VM-je, znotraj njih pa storitve tečejo v kontejnerjih, ločeno drug od druge.

Nameščeno imam:

Immich in Nextcloud – za shranjevanje slik in dokumentov.
Nginx Proxy Manager – z avtomatskimi SSL certifikati, ki skrbi, da posamezne storitve niso neposredno izpostavljene navzven.
Omrežje (na grobo)
Domače omrežje je razdeljeno na več VLAN-ov – ločeno omrežje za "obiskovalce/IoT", ločeno za osebne naprave in ločeno za strežniško/homelab opremo, da morebiten problem na eni napravi ne ogrozi ostalih.
Ves promet med VLAN-i gre skozi firewall na routerju, ki po privzeti nastavitvi vse blokira in dovoljuje samo eksplicitno odprta pravila.
Dostop od zunaj gre večinoma prek VPN, ne prek neposredno odprtih vrat na strežnik.
Storitve, ki so dostopne navzven, gredo vse skozi reverse proxy s SSL.
Brezžični dostop pokriva ena dostopna točka, ki oddaja dva ločena WiFi omrežja – eno za goste, eno za družinske naprave – vsak vezan na svoj VLAN.

## Diagram omrežja
``` mermaid
flowchart TB
    WAN((Internet))
    WAN --- FW

    subgraph FW["pfSense (router + firewall + pfBlocker)"]
        direction TB
        VPN[VPN dostop]
    end

    FW ---|trunk| SW["Managed switch"]

    SW --- AP["Dostopna točka (2x SSID)"]
    SW --- SRV["Proxmox strežnik"]

    subgraph V1["VLAN: Gostje"]
        AP1[Gost WiFi]
    end
    subgraph V2["VLAN: Družina"]
        AP2[Družinski WiFi]
    end
    subgraph V3["VLAN: Homelab / strežniki"]
        NPM["Nginx Proxy Manager"]
        IMM["Immich"]
        NC["Nextcloud"]
    end

    AP --- AP1
    AP --- AP2
    SRV --- NPM
    SRV --- IMM
    SRV --- NC
```
Kaj se učim ob tem
Osnove Linux administracije in Docker Compose.
Segmentacijo omrežja in firewall pravila.
Reverse proxy konfiguracijo in upravljanje SSL certifikatov.
Kaj je za izboljšati
Redne avtomatske posodobitve kontejnerjev.
Avtomatski backup-i.
