# 03 — Instalação e Configuração do Suricata

O Suricata é instalado no servidor e integrado ao Wazuh via arquivo de log `eve.json`.

---

## 📥 Instalação

```bash
sudo apt install suricata -y
```

Verificar versão instalada:

```bash
suricata --version
```

---

## 🔍 Identificar a interface de rede

```bash
ip a
```

Anote o nome da interface que recebe o tráfego da rede local — geralmente `eth0`, `enp3s0`, `ens18`, etc.

---

## ⚙️ Editar `suricata.yaml`

```bash
sudo nano /etc/suricata/suricata.yaml
```

### 1. Definir a interface de captura

```yaml
af-packet:
  - interface: xxxxx   # substitua pelo nome da sua interface
```

### 2. Definir a rede local (HOME_NET)

```yaml
vars:
  address-groups:
    HOME_NET: "[192.167.1.0/24]"   # substitua pela sua faixa de rede
    EXTERNAL_NET: "!$HOME_NET"
```

> ⚠️ `HOME_NET` correto é essencial para que as regras funcionem.
> Tráfego interno não classificado como HOME_NET pode gerar falsos positivos ou alertas ignorados.

---

## 📦 Atualizar regras (Emerging Threats)

```bash
sudo suricata-update
```

As regras são salvas em `/var/lib/suricata/rules/`.

Verificar regras carregadas:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

---

## ▶️ Iniciar o Suricata

```bash
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```

Esperado: `active (running)`

---

## 📄 Verificar logs

O Suricata gera alertas em formato JSON:

```bash
sudo tail -f /var/log/suricata/eve.json | jq '.event_type'
```

Se `jq` não estiver instalado:

```bash
sudo apt install jq -y
```

---

## 🔗 Integrar Suricata com Wazuh

No arquivo `/var/ossec/etc/ossec.conf` **do servidor**, adicione dentro de `<ossec_config>`:

```xml
<!-- Integração com Suricata -->
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

Reiniciar o Wazuh Manager:

```bash
sudo systemctl restart wazuh-manager
```

---

## ✅ Verificar integração no Dashboard

1. Acesse **Threat Hunting → Events**
2. Filtre por `rule.groups: suricata`
3. Gere tráfego de teste — ex: `ping` para o servidor
4. Alertas do Suricata devem aparecer em segundos

---

## 📸 Screenshots

_Adicione prints do dashboard mostrando alertas vindos do Suricata._
