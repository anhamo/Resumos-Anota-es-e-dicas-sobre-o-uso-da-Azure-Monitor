# Resumos-Anota-es-e-dicas-sobre-o-uso-da-Azure-Monitor
Este repositório contém o resumo das lições aprendidas durante o a formação  AZ-104: Microsoft Azure Administrator

Agente do Azure Monitor
O AMA (Agente do Azure Monitor) coleta dados de monitoramento do sistema operacional convidado do Azure e das máquinas virtuais híbridas e os entrega ao Azure Monitor, para uso por recursos, insights e outros serviços, como o Microsoft Sentinel e o Microsoft Defender para Nuvem. O Agente do Azure Monitor substitui todos os agentes de monitoramento herdados do Azure Monitor, incluindo:
•	Agente do Log Analytics: envia dados a um workspace do Log Analytics e dá suporte a soluções de monitoramento. Isso é totalmente consolidado no agente do Azure Monitor.
•	Agente o Telegraf: Envia dados para Métricas do Azure Monitor (somente Linux). Somente plug-ins do Telegraf básicos têm suporte hoje no agente do Azure Monitor.
•	Extensão de Diagnóstico: envia dados para Métricas do Azure Monitor (somente Windows), Hubs de Eventos do Azure e Armazenamento do Azure. Isso ainda não está consolidado.
Instalar o Agente do Azure Monitor
O Agente do Azure Monitor é implementado como uma extensão de VM do Azure. Os pré-requisitos a seguir devem ser atendidos antes da instalação do Agente do Azure Monitor.
•	Permissões: para métodos diferentes do portal do Azure, você precisa ter as seguintes atribuições de função para instalar o agente:
o	Colaborador de Máquina Virtual
o	Administrador de recursos de Azure Connected Machine
o	Qualquer função que inclua a ação Microsoft.Resources/deployments/* (por exemplo, Colaborador do Log Analytics.)
•	Fora do Azure: Para instalar o agente em servidores físicos e máquinas virtuais hospedadas fora do Azure (ou seja, localmente) ou em outras nuvens, você deve instalar o agente do Azure Arc Connected Machine.
•	Autenticação: a identidade gerenciada precisa ser habilitada nas máquinas virtuais do Azure. Há suporte para identidades gerenciadas atribuídas pelo sistema e pelo usuário.
•	Rede: se você usar firewalls de rede, a marca de serviço do Azure Resource Manager precisará estar habilitada na rede virtual da máquina virtual. A máquina virtual também precisa ter acesso aos seguintes endpoints HTTPS:
o	global.handler.control.monitor.azure.com
o	<virtual-machine-region-name>.handler.control.monitor.azure.com (exemplo: westus.handler.control.monitor.azure.com)
o	<log-analytics-workspace-id>.ods.opinsights.azure.com (exemplo: 12345a01-b1cd-1234-e1f2-1234567g8h99.ods.opinsights.azure.com)
É importante observar que, uma vez instalado o agente, os Agentes do Azure Monitor não funcionam sem estarem associados às regras de coleta de dados. Elas podem ser criados manualmente ou usando os Insights da VM.
Você pode instalar o Agente do Azure Monitor em máquinas virtuais do Azure usando os comandos az vm extension set –name AzureMonitorWindowsAgent ou az vm extension set AzureMonitorLinuxAgent.
Por exemplo, para instalar o Agente do Azure Monitor usando a identidade gerenciada atribuída pelo sistema em uma VM de IaaS do Windows Server, use o comando:
az vm extension set --name AzureMonitorWindowsAgent --publisher Microsoft.Azure.Monitor --ids <vm-resource-id> --enable-auto-upgrade true
Para instalar o Agente do Azure Monitor usando a identidade gerenciada atribuída pelo sistema em uma VM de IaaS do Linux, use o comando:
az vm extension set --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --ids <vm-resource-id> --enable-auto-upgrade true
A criação de uma regra de coleta de dados por meio do portal do Azure implanta automaticamente o Agente do Azure Monitor em uma VM de IaaS se ele ainda não estiver implantado.
A Microsoft recomenda que você habilite a Atualização Automática de Extensão ao implantar o Agente do Azure Monitor. Você pode habilitar o Azure Policy para implantar automaticamente o Agente do Azure Monitor e associá-lo a uma regra de coleta de dados sempre que implantar uma nova VM de IaaS.
Monitorar o desempenho com insights de VM
Os insights da VM fornecem um método rápido e fácil para começar a monitorar as cargas de trabalho do cliente em suas máquinas virtuais e conjuntos de dimensionamento de máquinas virtuais. Ele exibe um inventário de suas VMs existentes e fornece uma experiência guiada para habilitar o monitoramento base para elas. Ele também monitora o desempenho e a integridade das máquinas virtuais e dos conjuntos de dimensionamento de máquinas virtuais, coletando dados em seus respectivos processos em execução e dependências de outros recursos. Os insights de VM monitoram os principais indicadores de desempenho do sistema operacional relacionados ao processador, à memória, ao adaptador de rede e à utilização do disco.
Os insights da VM fornecem um conjunto de pastas de trabalho predefinidas que permitem exibir a tendência de dados de desempenho coletados ao longo do tempo. Você pode exibir esses dados em uma VM individual diretamente na máquina virtual ou pode usar o Azure Monitor para entregar uma exibição agregada de várias VMs.
Os insights de VM incluem um conjunto de gráficos de desempenho destinados a vários indicadores chave de desempenho para ajudar você a determinar o desempenho de uma máquina virtual. Os gráficos mostram a utilização de recursos em um período de tempo. Você pode usá-los para identificar gargalos e anomalias. Você também pode mudar para uma perspectiva que lista cada máquina para visualizar a utilização de recursos com base na métrica selecionada.
Os insights da VM fornecem os seguintes benefícios além de outros recursos para monitorar VMs no Azure Monitor:
•	Integração simplificada do agente do Azure Monitor e do agente de Dependência para que você possa monitorar as cargas de trabalho e o sistema operacional convidado de uma máquina virtual.
•	Regras de coleta de dados predefinidas que coletam o conjunto mais comum de dados de desempenho.
•	Gráficos de tendências de desempenho e pastas de trabalho predefinidos para que você possa analisar as principais métricas de desempenho do sistema operacional convidado da máquina virtual.
•	Mapa de dependências, que exibe os processos executados em cada máquina virtual e os componentes interconectados com outros computadores e fontes externas.
 
Habilitar insights da VM
Para habilitar o VM Insights, selecione Insights no menu da máquina virtual no portal do Azure. Se os insights de VM não estiverem habilitados, você verá uma breve descrição e uma opção para habilitá-los.
Selecione Habilitar para abrir o painel de configuração monitoramento. Deixe a opção padrão do agente do Azure Monitor.
A habilitação de insights de VM cria uma regra de coleta de dados padrão que não inclui coleta de processos e dependências. Para habilitar essa coleta, selecione Criar novo para criar uma nova regra de coleta de dados.
Você será solicitado a fornecer um nome de regra de coleta de dados e, em seguida, selecionar Habilitar processos e dependências (Mapa). Você não pode desabilitar a coleta de desempenho do convidado porque ela é necessária para insights de VM.
Você pode usar o workspace padrão do Log Analytics para a assinatura, a menos que tenha outro workspace que deseja usar.
1.	Selecione Criar para criar a nova regra de coleta de dados.
2.	Selecione Configurar para iniciar a configuração de insights de VM.
Exibir o desempenho da VM
Quando a implantação for concluída, você verá exibições na guia Desempenho em insights de VM com dados de desempenho para o computador. Esses dados mostram os valores das principais métricas de convidados ao longo do tempo.
Por padrão, os gráficos mostram as últimas 24 horas. Usando o seletor TimeRange, você pode consultar intervalos de tempo históricos de até 30 dias para mostrar o desempenho anterior.
Os seguintes gráficos de utilização de capacidade estão disponíveis:
•	% de utilização de CPU: os padrões mostram o percentil médio e o 95º superior.
•	Memória disponível: os padrões mostram o percentil médio e o 5º e o 10º percentil.
•	% de espaço em disco lógico usado: os padrões mostram o percentil médio e o 95º percentil.
•	IOPS de disco lógico: os padrões mostram o percentil médio e o 95º percentil.
•	MB/s do disco lógico: os padrões mostram o percentil médio e o 95º percentil.
•	% máximo de disco lógico usado: os padrões mostram o percentil médio e o 95º percentil.
•	Taxa de bytes enviados: os padrões mostram os bytes médios enviados.
•	Taxa de bytes recebidos: os padrões mostram a média de bytes recebidos.
 
Exibir dependências
Nos insights da VM, você pode exibir componentes de aplicativo descobertos Windows VMs (máquinas virtuais) Linux que são executados no Azure ou em seu ambiente. Você pode exibir um mapa diretamente de uma VM. Você também pode exibir um mapa do Azure Monitor para ver os componentes entre os grupos de VMs. Esta unidade ajuda você a entender esses dois métodos de exibição e como usar o recurso Mapa.
O recurso Mapa visualiza as dependências de VM descobrindo os processos em execução que têm:
•	Conexões de rede ativas entre servidores.
•	Latência de conexão de entrada e de saída.
•	Portas em qualquer arquitetura conectada a TCP em um intervalo de tempo especificado.
Para acessar o mapa de insights de VM diretamente de uma VM:
1.	No Portal do Azure, selecione Máquinas Virtuais.
2.	Na lista, selecione uma VM. Na seção Monitoramento, selecione Insights.
3.	Selecione a guia Mapa.
O mapa visualiza as dependências da VM descobrindo a execução de processos e grupos de processos que têm conexões de rede ativas em um intervalo de tempo especificado.
Por padrão, o mapa mostra os últimos 30 minutos. Se você quiser ver como as dependências foram examinadas no passado, consulte intervalos de tempo históricos de até uma hora. Para executar a consulta, use o seletor TimeRange no canto superior esquerdo. Você pode executar uma consulta, por exemplo, durante um incidente ou para ver o status antes de uma alteração.
 
No Azure Monitor, o recurso Mapa fornece uma exibição global das suas VMs e das respectivas dependências. Para acessar o recurso Mapa no Azure Monitor:
1.	No portal do Azure, selecione Monitor.
2.	Na seção Insights, selecione Máquinas Virtuais.
3.	Selecione a guia Mapa.
Se você tiver mais de um workspace do Log Analytics, escolha o workspace que está habilitado com a solução e que tem VMs subordinadas.
O seletor Grupo retornará assinaturas, grupos de recursos, grupos de computadores e conjunto de dimensionamento de máquinas virtuais dos computadores relacionados ao workspace selecionado. A seleção só se aplica ao recurso Mapa e não é transferida para Desempenho nem Integridade.
________________________________________
Configurar regras de coleta de dados para logs de VM IaaS
Se você habilitar insights de VM, o agente do Azure Monitor será instalado e começará a enviar um conjunto predefinido de dados de desempenho para os Logs do Azure Monitor. Você pode criar regras de coleta de dados adicionais para coletar eventos e outros dados de desempenho.
As DCRs (regras de coleta de dados) definem o processo de coleta de dados no Azure Monitor. As DCRs especificam quais dados devem ser coletados, como transformar esses dados e para onde enviar esses dados. Algumas DCRs serão criados e gerenciados pelo Azure Monitor para coletar um conjunto específico de dados para habilitar insights e visualizações. Você também pode criar suas DCRs para definir o conjunto de dados necessário para outros cenários.
Você pode definir uma regra de coleta de dados para enviar dados de vários computadores para vários workspaces do Log Analytics, incluindo workspaces em uma região ou locatário diferente. Criar a regra de coleta de dados na mesma região do workspace do Log Analytics. Você pode enviar dados de eventos e o Syslog do Windows somente para os Logs do Azure Monitor. Você pode enviar contadores de desempenho para métricas de Azure monitor e registros do Azure Monitor.
Coletar eventos e logs de desempenho
Para coletar eventos e dados de desempenho, execute as seguintes etapas:
1.	No portal do Azure, navegue até o Azure Monitor.
2.	No menu Monitor, selecione Regras de Coleta de Dados.
3.	Selecione Criar para criar uma nova regra de coleta de dados e associações.
4.	Insira um nome de regra e especifique uma assinatura, grupo de recursos, região e tipo de plataforma:
•	A região especifica onde o DCR será criado. As máquinas virtuais e as associações podem estar em qualquer assinatura ou grupo de recursos no locatário.
•	O Tipo de Plataforma especifica o tipo de recursos aos quais essa regra pode se aplicar. A opção Personalizado permite os tipos Windows e Linux.
5.	Na guia Recursos :
•	Selecione + Adicionar recursos e associe recursos à regra de coleta de dados. Os recursos podem ser máquinas virtuais, Conjuntos de Dimensionamento de Máquinas Virtuais e Azure Arc para servidores.
•	Selecione Habilitar Pontos de Coleta de Dados.
•	Selecione um ponto de extremidade de coleta de dados para cada um dos recursos e associe à regra de coleta de dados.
6.	Na guia Coletar e entregar , selecione Adicionar fonte de dados para adicionar uma fonte de dados e definir um destino.
7.	Selecione um tipo de fonte de dados .
8.	Selecione quais dados você deseja coletar. Para contadores de desempenho, selecione em um conjunto predefinido de objetos e a taxa de amostragem deles. Para eventos, você pode selecionar um conjunto de registros e níveis de gravidade.
9.	Selecione Personalizado para coletar logs e contadores de desempenho que não têm suporte atualmente em fontes de dados ou filtrar eventos usando consultas XPath. Você pode especificar um XPath para coletar qualquer valor específico.
10.	Na guia Destino , adicione um ou mais destinos para a fonte de dados. Você pode selecionar vários destinos do mesmo tipo ou de tipos diferentes. Por exemplo, você pode selecionar vários workspaces do Log Analytics, que também são conhecidos como multihoming. Você pode enviar eventos do Windows e fontes de dados Syslog somente para os Logs do Azure Monitor. Você pode enviar contadores de desempenho para métricas de Azure monitor e registros do Azure Monitor.
11.	Selecione Adicionar fonte de dados e, em seguida, selecione Examinar + criar para examinar os detalhes da regra de coleta de dados e a associação com o conjunto de máquinas virtuais.
Coletar logs do IIS
Para criar a regra de coleta de dados para coletar logs do IIS de uma VM do Windows Server no portal do Azure, execute as etapas descritas na seção coletar eventos e desempenho, exceto ao escolher sua fonte de dados, especifique logs do IIS.
 
Coletar dados do Syslog
O Syslog é um protocolo de registro de eventos em log comum para o Linux. Você pode usar o daemon do Syslog integrado a dispositivos Linux para coletar eventos locais dos tipos especificados. Em seguida, você pode fazer com que ele envie esses eventos para um workspace do Log Analytics. Os aplicativos enviam mensagens que podem ser armazenadas no computador local ou entregues a um coletor de Syslog.
Quando o agente do Azure Monitor para Linux é instalado, ele configura o daemon do Syslog local para encaminhar mensagens para o agente quando a coleção Syslog estiver habilitada na DCR (regra de coleta de dados). Em seguida, o Agente do Azure Monitor envia as mensagens para um workspace do Azure Monitor ou Log Analytics em que um registro de Syslog correspondente é criado em uma tabela Syslog.
As seguintes instalações têm suporte com o coletor do Syslog:
•	autenticação
•	authpriv (autenticação e privacidade)
•	cron
•	daemon
•	marca
•	kerning
•	lpr
•	correio
•	notícias
•	syslog
•	usuário
•	uucp
•	local0-local7
O Agente do Azure Monitor para Linux coleta apenas eventos com as instalações e as severidades especificadas na configuração. Você pode configurar o Syslog por meio do Portal do Azure ou gerenciando arquivos de configuração em seus agentes do Linux.
Configure a coleção Syslog no menu Regras de Coleta de Dados do Azure Monitor. Essa configuração é entregue ao arquivo de configuração em cada agente do Linux. Siga o procedimento descrito anteriormente na unidade para eventos e logs de desempenho, mas ao selecionar Adicionar fonte de dados, verifique se, para o tipo de fonte de dados, selecione o syslog do Linux.
Você pode coletar eventos de Syslog com um nível de log diferente para cada instalação. Por padrão, todos os tipos de instalação do Syslog são coletados. Se você não quiser coletar, por exemplo, eventos do tipo auth, selecione NONE na caixa de listagem nível de log mínimo para auth e salve as alterações. Se você precisar alterar o nível de log padrão para eventos do Syslog e coletar apenas eventos com um nível de log começando em NOTICE ou uma prioridade mais alta, selecione LOG_NOTICE na caixa de listagem de nível de log Mínimo .

