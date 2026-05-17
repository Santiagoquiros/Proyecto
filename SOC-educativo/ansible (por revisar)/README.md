# Automatización con Ansible

Este directorio contiene playbooks Ansible para automatizar la instalación y configuración del SOC.

## Playbooks Disponibles

### playbook_install_wazuh.yml
Instala y configura Wazuh Manager en un servidor Ubuntu/Debian.

```bash
ansible-playbook -i inventory.ini playbook_install_wazuh.yml
```

### playbook_install_elk.yml
Instala y configura ELK Stack (Elasticsearch, Logstash, Kibana).

```bash
ansible-playbook -i inventory.ini playbook_install_elk.yml
```

### playbook_install_agent.yml
Instala agentes Wazuh en servidores remotos.

```bash
ansible-playbook -i inventory.ini playbook_install_agent.yml -e "wazuh_manager_ip=192.168.1.100"
```

## Estructura

```
ansible/
├── playbook_install_wazuh.yml
├── playbook_install_elk.yml
├── playbook_install_agent.yml
├── roles/
│   ├── wazuh-manager/
│   ├── elk-stack/
│   └── wazuh-agent/
└── inventory.ini
```

## Requisitos

- Ansible 2.9+
- SSH acceso a servidores destino
- Contraseña de administrador o SSH keys configuradas

## Uso

1. Editar `inventory.ini` con IPs de servidores
2. Ejecutar playbook correspondiente
3. Verificar instalación con comandos de validación

Ver documentación en `/docs` para más detalles.
