# Arquitetura do laboratório SOC com Wazuh

## Objetivo

Monitorar eventos de falha de autenticação do windows com Wazuh, investigar as avidências e documentar os resultados

## Status

Arquitetura planejada. Instalção e comunicação entre os componentes ainda serão validadas

## Componentes

### Windows - Computador monitorado e hospedeiro

O computador físico com Windows terá duans funções:

- Executar a máquina virtual da central Wazuh
- Ser o computador monitorado durante os testes

O Wazuh Agent será instalado com uma conta de testes com foco no evento 4625, relacionado a falhas de logon.

### VM linux - central Wazuh

Uma máquina virtual linux executará os três componentes:

- Wazuh Server: recebe e enalisa os eventos enviados pelo agente
- Wazuh Indexer: armazena e indexa os dados para consulta
- Wazuh Dashboard: oferece a interface de consulta pelo navegador

Sistema operacional e versão: a definir
Programa de virtualização: VMWare
Recursos de CPU, RAM e Disco: a definir

### Ferramentas já disponiveis

- Wireshark no Windows: disponível para análise de tráfego, caso seja necessário

## Diagrama
    
    Computador Físico com Windows
    |
    |-- Conta de teste
    |   '-- Gera falhas de autenticação
    |
    |-- Wazuh Agent
    |   '-- Coleta os eventos e envia ao Server na VM
    |
    |-- Navegador
    |   '-- Acessa o Dashboard na VM
    |
    |-- Máquina virtual Linux
        |-- Wazuh Server
        |-- Wazuh Indexer
        |-- Wazuh Dashboard

## Comunicação de rede

O windows e a VM linux precisarão se comunicar pela rede virtual no programa de virtualização.

Modo de rede: a definir
Endereço IP da VM Linux: a definir

O agente será configurado para enviar os eventos ao endereço da VM. O navegador do Windows acessará o dashboard nessa VM

## Fluxo dos eventos

1. Uma tentativa de login falha no Windows
2.  Com a auditoria configurada, o Windows registra o evento
3. O agente coleta o evento e o envia ao Wazuh Server
4. O server analisa o evento conforme as regras configuradas
5. Os alertas gerados são indexados e podem ser consultados pelo dashboard

O recebimento de um evento e a geração de um alerta serão validados separadamente. Nem todo evento gera um alerta

## validação prevista

- Confirmar que o agente está conectado ao Server
- Gerar uma falha controlada com a conta de teste
- Localizar o evento 4625
- Verificar o recebimento do evento pelo Wazuh
- Verificar se houve alerta e identificar a regra responsável

## Limitações

O Windows monitorado também hospeda a VM Wazuh; dessa forma, os dois ambientes compartilham os recursos do computador físico

A primeira versão terá apenas um computador monitorado e testes manuais de falha de autenticação