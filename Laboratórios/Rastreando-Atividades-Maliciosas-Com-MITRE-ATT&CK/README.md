***Laboratório***: Análise de Incidente com MITRE ATT&CK (Hacker Hive).
***Analista***: Victor Revoredo.
***Ferramentas/Frameworks***: MITRE ATT&CK Navigator, PDF do Cenário.
# Cenário 
Durante o monitoramento de eventos no SOC, foi identificado que um endpoint executou o seguinte comando PowerShell fora do horário comercial: 
```
powershell.exe -nop -w hidden -EncodedCommand ... 
```

***Detalhes da ocorrência***: 
- Execução fora do horário comercial 
- Usuário sem privilégios administrativos 
- Comando PowerShell em modo oculto e codificado 
# Mapeamento MITRE ATT&CK

| Tática    | Técnica                                       | ID MITRE  | Ferramentas                                        | [$^5$] |
| --------- | --------------------------------------------- | --------- | -------------------------------------------------- | ------ |
| Execution | Command and Scripting Interpreter: PowerShell | T1059.001 | [[#Empire\|Empire]], [[#PowerSploit\|PowerSploit]] |        |

# Relatório
## Análise da Anomalia
Durante a triagem realizada às 09:15, identificou-se um evento crítico ocorrido na madrugada (01:52). O alerta foi disparado devido à execução de PowerShell em uma estação de trabalho comum (sem perfil administrativo), fora do horário comercial. A combinação do horário (madrugada), com o uso de uma ferramenta de administração (PowerShell), por um usuário sem privilégios, e utilizando de comandos para esconder sua execução (como `hidden`, `-EncodedCommand`), são indicadores de alta confiança que **se trata de um comportamento malicioso**$^1$, ligado a técnica **T1059.001 (Command and Scripting Interpreter: PowerShell)**$^2$.
## Nivelamento de Risco
A Técnica T1059.001 é muito utilizada para **realizar downloads e executar softwares maliciosos (malwares)**$^3$, sendo uma forma do atacante realizar vários tipos de processos que possam não somente comprometer a estrutura funcional da empresa, como também realizar a coleta de dados confidenciais. Sendo assim um **processo que se faz necessário a análise investigativa**$^4$!
## Próximos Passos$^6$
Considerando os fatos passados, se faz necessário para a mitigação e análise do ataque as seguintes ações:

- Coleta de eventos correlacionados (ex.: outros logs de PowerShell, alertas de EDR);
- Analise das conexões de rede do host para identificação de comunicações suspeitas;
- Realizar dump de memória para identificação de possíveis payloads carregados;
- Utilizar antivírus/antimalware para execução de quarentenas automáticas quando detectado qualquer tipo de arquivo suspeito.
...
## Ferramentas de Teste Ofensivo
### Empire
Estrutura de administração remota e pós-exploração de código aberto e multiplataforma que está disponível publicamente no GitHub.

***Principais Técnicas Utilizadas***:
- Escalonamento de Privilégios
- Roubo de dados 
- Coleta de dados em transporte (Man-In-The-Middle)
- Exfiltração Automatizada
...
### PowerSploit
Framework de segurança de código aberto, ofensivo, composto de PowerShell módulos e scripts que executam uma ampla gama de tarefas relacionadas a testes de penetração, como execução de código, persistência, ignorar antivírus, reconhecimento e exfiltração.

***Principais Técnicas Utilizadas***:
- Captura de áudio
- Criar e modificar processo do sistema
- Execução de spywares (Como Keyloggers, Screenloggers...)
- Enumeração de contas
...