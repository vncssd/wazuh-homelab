# 🛡️ Wazuh Homelab — SIEM + IDS com Suricata

Documentação completa da configuração de um ambiente de monitoramento de segurança doméstico usando **Wazuh** e **Suricata**, instalados em hardware físico dedicado.

---

## 🎯 Objetivo

Transformar um computador sem uso em um SOC (Security Operations Center) doméstico capaz de:

- Monitorar logs de sistemas e aplicações
- Detectar intrusões e comportamentos suspeitos na rede
- Gerar alertas em tempo real via dashboard web

---

## 🖥️ Arquitetura

```
[Laptop / Agente Wazuh]
        |
        | (SSH / rede local)
        |
[Servidor — Ubuntu Server 24.04]
  ├── Wazuh Manager
  ├── Wazuh Indexer
  ├── Wazuh Dashboard
  └── Suricata (IDS)
```

---

## 📁 Estrutura do Repositório

```
wazuh-homelab/
├── README.md
├── docs/
│   ├── 01-hardware.md              # Especificações e decisões de SO
│   ├── 02-instalacao-wazuh.md      # Instalação do Wazuh (all-in-one)
│   ├── 03-suricata-config.md       # Instalação e configuração do Suricata
│   └── 04-regras-customizadas.md   # Regras personalizadas do Suricata
│   
├── configs/
│   ├── suricata/
│   │   └── local.rules.example             # Regras customizadas do Suricata
│   └── wazuh/
│       └── ossec-snippets.conf     # Trechos relevantes do ossec.conf
└── screenshots/
```

---

## 🚀 Como Reproduzir

Siga os documentos na ordem:

1. [Hardware e SO](docs/01-hardware.md)
2. [Instalação do Wazuh](docs/02-instalacao-wazuh.md)
3. [Configuração do Suricata](docs/03-suricata-config.md)
4. [Regras Customizadas](docs/04-regras-customizadas.md)

---

## 🛠️ Stack

| Ferramenta | Versão | Função |
|---|---|---|
| Ubuntu Server | 24.04 LTS | Sistema operacional do servidor |
| Wazuh | 4.x | SIEM / Manager de logs e alertas |
| Suricata | 7.x | IDS — detecção de intrusão por rede |

---

## 👤 Autor

**Vinícius Silva dos Santos**
[LinkedIn](https://linkedin.com/in/vncssd) · [GitHub](https://github.com/vncssd)
