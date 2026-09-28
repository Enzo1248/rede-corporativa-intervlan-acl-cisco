# 🌐 Arquitetura de Rede Corporativa Multicamadas com Cisco Packet Tracer

Projeto prático de infraestrutura de redes focado em segmentação de tráfego L2/L3, roteamento de borda, atribuição dinâmica de endereçamento IP, segurança por Access Control Lists (ACLs) e tradução de endereços WAN (NAT/PAT).

---

## 🛠️ Topologia e Tecnologias Utilizadas

- **Roteador de Borda (`HQ-RTR`):** Cisco 4331
- **Switch Multicamada (`HQ-SW-CORE`):** Cisco WS-C3650-24PS
- **Switches de Acesso (`HQ-SW-ACC-01` e `HQ-SW-ACC-02`):** Cisco WS-C2960-24TT
- **Simulador:** Cisco Packet Tracer

---

## 📐 Endereçamento e Sub-redes (VLSM)

Bloco base: `192.168.10.0/24`

| VLAN | Nome | Máscara / CIDR | Faixa de IPs | Gateway Padrão |
| :--- | :--- | :--- | :--- | :--- |
| **10** | ADMIN | `255.255.255.192` (/26) | `192.168.10.1` - `192.168.10.62` | `192.168.10.1` |
| **20** | FINANCE | `255.255.255.192` (/26) | `192.168.10.65` - `192.168.10.126` | `192.168.10.65` |
| **30** | SERVERS | `255.255.255.224` (/27) | `192.168.10.129` - `192.168.10.158` | `192.168.10.129` |
| **99** | GUEST | `255.255.255.224` (/27) | `192.168.10.161` - `192.168.10.190` | `192.168.10.161` |

---

## 🔒 Recursos e Políticas de Segurança Implementadas

1. **Roteamento Inter-VLAN:** Configuração do modelo *Router-on-a-Stick* via sub-interfaces 802.1Q no `HQ-RTR`.
2. **Serviço DHCP Centralizado:** Escopos DHCP atribuídos diretamente no roteador para distribuição automática nas VLANs de acesso.
3. **ACL Estendida de Segurança (`ACL_BLOCK_GUEST`):**
   - Permite tráfego das portas UDP 67/68 (DHCP) para obtenção de IP na sub-rede de visitantes.
   - Permite consultas DNS externas.
   - **Bloqueia expressamente** a comunicação da VLAN 99 (GUEST) com as redes internas e o servidor corporativo (`SRV-CORP`).
4. **NAT/PAT Overload:** Mapeamento do bloco interno `192.168.10.0/24` para o IP público da interface Serial WAN (`200.100.10.2`) garantindo acesso aos serviços da Internet.

---

## 🧪 Validação dos Testes de Conectividade

- [x] Atribuição dinâmica de IP via DHCP validada no `PC-ADMIN`, `PC-FINANCE` e `PC-GUEST`.
- [x] Comunicação liberada entre `PC-ADMIN` / `PC-FINANCE` e o Servidor Corporativo (`192.168.10.130`).
- [x] Bloqueio verificado com sucesso a partir do `PC-GUEST` em direção ao servidor (`Destination host unreachable`).
- [x] Resolução DNS e resposta ao ping público (`8.8.8.8`) operacionais através de tradução NAT/PAT.

---

## 📂 Como Utilizar este Repositório

1. Faça o download do arquivo `.pkt` localizado neste repositório.
2. Abra o arquivo no **Cisco Packet Tracer** (versão 8.x recomendada).
3. Utilize os comandos da CLI no `HQ-RTR` e nos switches para visualizar as configurações aplicadas:
   ```text
   show ip interface brief
   show ip route
   show ip nat translations
   show access-lists
