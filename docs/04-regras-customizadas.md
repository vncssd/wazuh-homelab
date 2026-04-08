# 04 — Regras Customizadas do Suricata

Regras criadas especificamente para este ambiente, além das regras padrão do Emerging Threats.

---

## 📁 Arquivo de regras locais

```bash
sudo nano /etc/suricata/rules/local.rules
```

Após editar sempre reinicie o Suricata:

```bash
sudo systemctl restart suricata
```

---

## 📐 Anatomia de uma regra Suricata

```
[ação] [protocolo] [src_ip] [src_port] -> [dst_ip] [dst_port] (opções)
```

| Campo | Valores comuns |
|---|---|
| Ação | `alert`, `drop`, `pass`, `log` |
| Protocolo | `tcp`, `udp`, `icmp`, `http`, `dns`, `tls` |
| IPs | `any`, `$HOME_NET`, `$EXTERNAL_NET`, IP específico |
| Portas | `any`, `80`, `!443`, `[80,443]` |

Cada regra precisa de um `sid` único e um `rev` (revisão).

---

## 🧪 Regras Criadas

### Regra 1 — Detectar ICMP (ping)

```
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping detectado"; sid:1000001; rev:1;)
```

**Objetivo:** confirmar que o Suricata está funcionando e integrado ao Wazuh.

**Teste:**
```bash
ping 192.168.x.x   # do laptop para o servidor
```

---

### Regra 2 — Detectar Port Scan (TCP SYN em múltiplas portas)

```
alert tcp $EXTERNAL_NET any -> $HOME_NET any (msg:"Possivel port scan detectado"; flags:S; threshold: type both, track by_src, count 20, seconds 5; sid:1000002; rev:1;)
```

**Objetivo:** detectar varreduras de portas feitas por ferramentas como nmap.

**Teste:**
```bash
nmap -sS 192.168.x.x   # do laptop (nmap precisa estar instalado)
```

---

### Regra 3 — Detectar tentativa de acesso a porta SSH de IP externo

```
alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"Tentativa de conexao SSH de rede externa"; sid:1000003; rev:1;)
```

**Objetivo:** monitorar tentativas de acesso SSH vindas de fora da rede local.

---

## 📋 Tabela de SIDs utilizados

| SID | Descrição | Status |
|---|---|---|
| 1000001 | ICMP Ping | ✅ Testado |
| 1000002 | Port Scan TCP | ✅ Testado |
| 1000003 | Falha de autenticação SSH | ✅ Testado |

> Use SIDs acima de 1.000.000 para regras locais — evita conflito com regras oficiais.

---

## 🔎 Verificar se as regras foram carregadas

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v 2>&1 | grep "local.rules"
```

Deve mostrar o número de regras carregadas do arquivo.
