# Playbook
***Regra de Disparo de Alerta:***
![[Laboratórios/Home Lab/Imagem/Pasted image 20260804203001.png]]

***No disparo do alerta anterior, realizar:***

- ***Triagem N1:***
	1. Identificar o usuário que executou a ação e o processo pai (Parent Process).
		
	2. Verificar se a ação foi realizada em horário comercial ou fora do expediente.
		
	3. Validar se a conta criada possui privilégios administrativos.
		
- ***Ação de Resposta N1 (Contenção):***
	1. Desabilitar a conta de usuário recém-criada via PowerShell/CMD.
		
	2. Isolar o host no Wazuh (via Active Response ou comando local).
		
- ***Ação de Escalonamento (N1 -> N2):***
	1. Se confirmado Verdadeiro Positivo (TP), abrir chamado de Nível 2 solicitando análise forense do binário executado.
# Relatório Ação N1
***Alertas:***
![[Pasted image 20260804205933.png]]
## 5W1H
1. ***Who*** -> vboxuser
2. ***What*** -> Criação de conta local + elevação para o grupo Administradores.
3. ***When*** -> 
	- ***Criação da Conta***: 2026-08-04 20:50:06.0061 UTC
	- ***Elevação da Conta***: 2026-08-04 20:50:06.081 UTC
4. ***Where*** ->
	- ***Hostname***: vboxuser
	- ***IP***: 10.0.0.6
5. ***Why*** -> Realização de uma movimentação vertical dentro dos sistemas.
	
6. ***How*** -> 
	- T1059.001 -> Powershell
## Contenção
1. ***Bloquear a Conta Maliciosa***:
```
Disable-LocalUser -Name "backdoor_user"
```
2. ***Remover a Persistência no Registro:***
```
Remove-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "MaliciousTask"
```
3. ***Isolamento de Rede:***
```
net user backdoor_user /active:no
```
![[Pasted image 20260804212437.png]]
# Documentação e Escalonamento para N2
===============================================================
RELATÓRIO DE ESCALONAMENTO DE INCIDENTE - SOC N1 
===============================================================
ID do Incidente: INC-2026-0044
Data/Hora da Detecção: 04/08/2026 - 20:50
Analista Responsável: Victor Revoredo
Severidade: Alta (High)
## 1. Resumo do Alerta:
Identificada criação não autorizada de usuário local ("backdoor_user") e adição ao grupo administradores, seguida de criação de chave de persistência no Registro do Windows.
## 2. Ativos Afetados:
- ***Hostname***: vboxuser
- ***IP***: 10.0.0.6
- Usuário comprometido/utilizado: Administrador
## 3. Evidências / IOCs:
- Event ID 4720 (User Created: backdoor_user)	![[Pasted image 20260804213601.png]]
- Event ID 4732 (Group Member Added: Administradores)
![[Pasted image 20260804213933.png]]
- Registro alterado: HKCU\Software\Microsoft\Windows\CurrentVersion\Run (Valor: MaliciousTask)
![[Pasted image 20260804214231.png]]
# 4. Ações de Contenção Aplicadas (N1):
- Conta "backdoor_user" desabilitada via PowerShell às 21:21.
- Chave de registro "MaliciousTask" removida.
