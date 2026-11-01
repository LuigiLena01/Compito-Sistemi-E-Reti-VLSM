# Prova di Sistemi e reti: VLAN, VLSM e ACL

Prova del quinto anno svolta in **Cisco Packet Tracer**: la rete del campus di un'università con quattro reparti separati in VLAN, indirizzi assegnati con VLSM e una ACL estesa che protegge la rete della Segreteria dagli studenti, senza isolarli dal resto della rete e da Internet.

Scheda completa nel portfolio: <https://portfoliogigi.com/progetti/prova-sistemi-vlan-acl/>

## Contenuto

| File | Descrizione |
|---|---|
| `VLSM.pkt` | Progetto Cisco Packet Tracer (router, switch e quattro PC) |
| `VLSM.pdf` | Relazione: calcoli VLSM, tabella di indirizzamento e spiegazione dell'ACL |

## Piano di indirizzamento

| VLAN | Reparto | Host richiesti | Sottorete | Gateway |
|---|---|---|---|---|
| 10 | Studenti | 900 | `172.16.0.0/22` | `172.16.3.254` |
| 20 | Laboratori | 200 | `172.16.4.0/24` | `172.16.4.254` |
| 30 | Docenti | 120 | `172.16.5.0/25` | `172.16.5.124` |
| 40 | Segreteria | 50 | `172.16.5.128/26` | `172.16.5.190` |

## Configurazione

- **Switch:** una porta di accesso per VLAN e un trunk 802.1Q verso il router.
- **Router (router-on-a-stick):** una sottointerfaccia `dot1Q` per VLAN con l'indirizzo del gateway.
- **ACL estesa 101**, in ingresso sulla sottointerfaccia della VLAN Studenti:

```
access-list 101 deny ip 172.16.0.0 0.0.3.255 172.16.5.128 0.0.0.63
access-list 101 permit ip any any
```

## Come aprirlo

Serve **Cisco Packet Tracer**. Apri `VLSM.pkt` con *File → Open*.

## Limiti noti

- L'ACL blocca anche il traffico di ritorno verso la Segreteria (per esempio le risposte ai ping); servirebbe `established` per il TCP o un firewall con controllo dello stato.
- Nel file i PC non hanno ancora un indirizzo IP, quindi l'ACL non si può verificare con un ping senza configurarli.

---

Autore: Luigi Lena · [portfoliogigi.com](https://portfoliogigi.com)
