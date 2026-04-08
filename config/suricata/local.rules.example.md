# Regras customizadas do Suricata — Wazuh Homelab
# Arquivo: /etc/suricata/rules/local.rules
#
# Convenção de SIDs:
#   1000001–1000099  → detecção básica / validação
#   1000100–1000199  → monitoramento de rede
#   1000200–1000299  → detecção de ataques
#
# Após qualquer alteração: sudo systemctl restart suricata
# Validar sintaxe:        sudo suricata -T -c /etc/suricata/suricata.yaml -v

# ---------------------------------------------------------------------------
# DETECÇÃO BÁSICA — validação do ambiente
# ---------------------------------------------------------------------------

alert icmp any any -> $HOME_NET any (msg:"ICMP Ping detectado"; sid:1000001; rev:1;)

# ---------------------------------------------------------------------------
# MONITORAMENTO DE REDE
# ---------------------------------------------------------------------------

alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"Tentativa de conexao SSH de rede externa"; sid:1000101; rev:1;)

alert dns any any -> any any (msg:"Consulta DNS detectada"; sid:1000102; rev:1;)

# ---------------------------------------------------------------------------
# DETECÇÃO DE ATAQUES
# ---------------------------------------------------------------------------

alert tcp $EXTERNAL_NET any -> $HOME_NET any (msg:"Possivel port scan detectado"; flags:S; threshold: type both, track by_src, count 20, seconds 5; sid:1000201; rev:1;)

# ---------------------------------------------------------------------------
# ESPAÇO PARA NOVAS REGRAS
# ---------------------------------------------------------------------------

# alert [protocolo] [src] [src_port] -> [dst] [dst_port] (msg:"Descrição"; sid:1000XXX; rev:1;)
