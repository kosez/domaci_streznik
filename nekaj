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
