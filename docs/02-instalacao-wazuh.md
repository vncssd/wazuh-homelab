# 02 — Instalação do Wazuh (All-in-One)

Instalação do Wazuh Manager, Indexer e Dashboard na mesma máquina usando o script oficial encontrado no site da ferramenta.

---

## ⚙️ Configuração do `config.yml`

> ⚠️ **Ponto crítico:** este arquivo define onde cada componente roda.
> Usar `127.0.0.1` aqui impede que agentes externos se conectem.
> **Use sempre o IP real da máquina.**

```yaml
nodes:
  indexer:
    - name: node-1
      ip: "192.168.x.x"   # IP estático do servidor

  server:
    - name: wazuh-1
      ip: "192.168.x.x"   # mesmo IP

  dashboard:
    - name: dashboard
      ip: "192.168.x.x"   # mesmo IP
```

---

## 🌐 Acessar o Dashboard

No navegador do laptop:

```
https://192.168.x.x/"PORTA DEFINIDA DURANTE A INSTALAÇÃO(PADRÃO 443)
```
## 🔌 Instalar o Agente Wazuh no Laptop

### Via Dashboard (recomendado)

1. Vá em **Agents → Deploy new agent**
2. Selecione o SO do laptop
3. Informe o IP do manager: `192.168.x.x`
4. Copie e execute o comando gerado

### Verificar status do agente:

```bash
sudo systemctl status wazuh-agent
```

Esperado: `active (running)`

---

## 📋 Configurar Logs no Agente (`ossec.conf`)

Arquivo: `/var/ossec/etc/ossec.conf` (no laptop)

```xml
<ossec_config>

  <!-- Logs gerais do sistema -->
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/syslog</location>
  </localfile>

  <!-- Tentativas de autenticação (SSH, sudo) -->
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/auth.log</location>
  </localfile>

  <!-- Adicione outros logs conforme necessidade -->

</ossec_config>
```

> ⚠️ **Erro comum:** caminho de log incorreto ou arquivo inexistente.
> Verifique antes de reiniciar: `ls /var/log/auth.log`

Após editar:

```bash
sudo systemctl restart wazuh-agent
```

---

## ✅ Verificar Eventos no Dashboard

1. Acesse **Threat Hunting → Events**
2. Filtre pelo nome do seu agente
3. Aguarde 1–2 minutos — eventos devem aparecer

---

## 📸 Screenshots

