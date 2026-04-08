# 01 — Hardware e Sistema Operacional

## 📋 Especificações da Máquina

| Componente | Especificação |
|---|---|
| CPU | Intel I7-3770 3.900GHz |
| RAM | 12GB |
| Armazenamento | 1TB |

---

### Por que Ubuntu Server 24.04?

- Suporte oficial do Wazuh para Ubuntu 22.04/24.04
- LTS com suporte até 2029
- Ampla documentação e comunidade ativa
- Familiaridade prévia com a distro

### Por que não usar uma VM?

A ideia do projeto é colocar a mão na massa, por isso, a decisão de usar hardware dedicado, testar em ambiente real, aprender o processo completo de instalação física.

### Por que IP estático?

O Wazuh Manager precisa de um IP fixo para que os agentes sempre saibam onde se conectar. IP dinâmico quebraria a comunicação após cada reinicialização.

---

## 💿 Instalação do Ubuntu Server

### Mídia de instalação

- ISO baixado em: [ubuntu.com/download/server](https://ubuntu.com/download/server)
- Gravado com: Balena Etcher /

### Opções selecionadas na instalação

- [x] OpenSSH Server habilitado
- [x] IP estático configurado
- [x] Sem interface gráfica

### Configuração de IP estático (Netplan)

Arquivo editado: `/etc/netplan/00-installer-config.yaml`

```yaml
network:
  version: 2
  ethernets:
    enp3s0:          # substitua pelo nome da sua interface (ip a)
      dhcp4: no
      addresses:
        - 192.168.x.x/24
      routes:
        - to: default
          via: 192.168.x.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Aplicar:
```bash
sudo netplan apply
```

---

## ✅ Verificações pós-instalação

```bash
# Confirmar IP
ip a

# Confirmar acesso SSH pelo laptop
ssh usuario@192.168.x.x

# Atualizar sistema
sudo apt update && sudo apt upgrade -y
```

---
