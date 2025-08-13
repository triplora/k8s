# k8s
Documentos, boas práticas e arquivos configs 

Kubernetes – Conceitos

1. Componentes do Kubernetes

    Plano de Controle (Control Plane): API Server (porta de entrada), etcd (banco de dados do estado do cluster), Controller Manager (mantém estado desejado), Scheduler (aloca pods).

    Componentes de Nó: Kubelet (agente que executa workloads), Kube-proxy (rede), runtime de contêiner.

    Add-ons: DNS, controladores de Ingress, servidor de métricas.

2. Diferença entre DaemonSet e ReplicaSet

    DaemonSet: Garante 1 pod por nó (ou por seletor), usado para agentes de monitoramento, logs, rede.

    ReplicaSet: Garante um número fixo de pods idênticos para workloads sem estado (stateless), usado para escalabilidade.

3. Como solucionar problemas de pods que não funcionam

    kubectl get pods -A → ver estado geral.

    kubectl describe pod → verificar Events para erros de imagem, probes, recursos.

    kubectl logs → analisar erros de aplicação.

    Testar conectividade (kubectl exec) e verificar NetworkPolicies e PVC/PV se houver problemas de rede/armazenamento.

4. Pod perde conexão HTTPS – o que fazer?

    Conferir mapeamento de Service e Endpoints.

    Testar dentro do pod (curl -vk).

    Verificar Ingress/Load Balancer, certificados, firewall, regras de VPC no GCP.

Networking – Conceitos

5. Diferença entre Netplan e configuração manual

    Netplan: Arquivo YAML persistente, aplicado com netplan apply.

    Manual: Comandos ip ou ifconfig, aplicam na hora mas não persistem sem salvar em arquivos de configuração.

6. Diferença entre Roteador e Switch

    Roteador: Conecta redes diferentes, usa protocolos de roteamento (OSPF, BGP).

    Switch: Conecta dispositivos na mesma rede (LAN), usa endereços MAC, suporta VLANs.

7. Subnet e CIDR

    Subnet: Segmento lógico de rede IP.

    CIDR: Notação IP + máscara (ex: 10.0.0.0/24).

8. Familiaridade com iSCSI e Catalyst

    iSCSI para armazenamento em bloco via IP (descoberta, autenticação CHAP, multipath).

    Catalyst para configuração VLAN, trunking, spanning tree, VRFs, VXLAN/eVPN, configuração L2/L3.

Armazenamento no GKE via CSI Drivers

Persistent Disks (PD)

    Discos persistentes, zonais ou regionais, SSD ou HDD, para workloads que precisam de armazenamento em bloco.

    Integrados ao Kubernetes via CSI driver; PVC cria e anexa o disco automaticamente.

Filestore

    Serviço gerenciado de NFS para acesso ReadWriteMany por múltiplos pods.

    Ideal para workloads que precisam compartilhar arquivos.

Boas práticas

    Regional PD para alta disponibilidade.

    RWO para um único pod, RWX para múltiplos.

    Ajustar tamanho para IOPS necessários.

IAM com Workload Identity

    Mapeia uma Service Account do Kubernetes para uma Service Account do GCP.

    Elimina uso de chaves estáticas, usando credenciais temporárias seguras.

    Permite seguir princípio de menor privilégio, atribuindo apenas as permissões necessárias.

Boas práticas

    Uma GSA por aplicação.

    Evitar papéis amplos como Editor, usar papéis pré-definidos mais restritivos.

    Auditar permissões regularmente.
