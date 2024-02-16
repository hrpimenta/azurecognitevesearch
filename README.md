# Explorando um Índice de Pesquisa Azure AI (UI)

Imagine que você trabalhe para a Fourth Coffee, uma cadeia nacional de café. Você foi solicitado a ajudar a construir uma solução de mineração de conhecimento que facilite a busca por insights sobre as experiências dos clientes. Você decide construir um índice de pesquisa Azure AI usando dados extraídos de avaliações de clientes.

Neste laboratório você irá:

Criar recursos do Azure
Extrair dados de uma fonte de dados
Enriquecer dados com habilidades de IA
Usar o indexador do Azure no portal do Azure
Consultar seu índice de pesquisa
Revisar resultados salvos em um Knowledge Store
Recursos do Azure necessários
A solução que você criará para a Fourth Coffee requer os seguintes recursos em sua assinatura do Azure:

Um recurso Azure AI Search, que gerenciará a indexação e a consulta.
Um recurso Azure AI services, que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.

Observação: Seus recursos Azure AI Search e Azure AI services devem estar na mesma localização!

Uma conta de armazenamento com contêineres de blob, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.
Criar um recurso Azure AI Search
Faça login no portal do Azure.

Clique no botão + Criar um recurso, pesquise por Azure AI Search e crie um recurso Azure AI Search com as seguintes configurações:

Assinatura: Sua assinatura do Azure.
Grupo de recursos: Selecione ou crie um grupo de recursos com um nome exclusivo.
Nome do serviço: Um nome exclusivo.
Localização: Escolha qualquer região disponível.
Nível de preço: Básico
Selecione Revisar + criar e, depois de ver a resposta Validação bem-sucedida, selecione Criar.

Após a conclusão da implantação, selecione Ir para recurso. Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

Criar um recurso Azure AI services
Você precisará provisionar um recurso Azure AI services que esteja na mesma localização que seu recurso Azure AI Search. Sua solução de pesquisa usará este recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

Volte para a página inicial do portal do Azure. Clique no botão +Criar um recurso e pesquise por Azure AI services. Selecione criar um plano de serviços Azure AI. Você será levado a uma página para criar um recurso Azure AI services. Configure-o com as seguintes configurações:

Assinatura: Sua assinatura do Azure.
Grupo de recursos: O mesmo grupo de recursos que seu recurso Azure AI Search.
Região: A mesma localização que seu recurso Azure AI Search.
Nome: Um nome exclusivo.
Nível de preço: Standard S0
Ao marcar esta caixa, eu reconheço que li e entendi todos os termos abaixo: Selecionado
Selecione Revisar + criar. Após ver a resposta Validação aprovada, selecione Criar.

Aguarde a conclusão da implantação e, em seguida, visualize os detalhes da implantação.
Criar uma conta de armazenamento
Volte para a página inicial do portal do Azure e, em seguida, selecione o botão + Criar um recurso.

Procure por conta de armazenamento e crie um recurso de conta de armazenamento com as seguintes configurações:

Assinatura: Sua assinatura do Azure.
Grupo de recursos: O mesmo grupo de recursos que seus recursos Azure AI Search e Azure AI services.
Nome da conta de armazenamento: Um nome exclusivo.
Localização: Escolha qualquer local disponível.
Desempenho: Padrão
Redundância: Armazenamento redundante localmente (LRS)
Clique em Revisar e, em seguida, clique em Criar. Aguarde a conclusão da implantação e, em seguida, vá para o recurso implantado.

Na conta de armazenamento do Azure que você criou, no painel de menu à esquerda, selecione Configuração (em Configurações).
Altere a configuração para Permitir acesso anônimo a Blob para Habilitado e, em seguida, selecione Salvar.
Enviar documentos para o Armazenamento do Azure
No painel de menu à esquerda, selecione Contêineres.

Captura de tela que mostra a página de visão geral do blob de armazenamento.

Selecione + Contêiner. Uma painel no seu lado direito é aberto.

Insira as seguintes configurações e clique em Criar:
Nome: coffee-reviews
Nível de acesso público: Contêiner (acesso de leitura anônimo para contêineres e blobs)
Avançado: sem alterações.
Em uma nova aba do navegador, baixe as análises de café compactadas de https://aka.ms/mslearn-coffee-reviews e extraia os arquivos para a pasta de análises.

No portal do Azure, selecione seu contêiner de análises de café. No contêiner, selecione Carregar.

Captura de tela que mostra o contêiner de armazenamento.

Na janela Carregar blob, selecione Selecionar um arquivo.

Na janela do Explorer, selecione todos os arquivos na pasta de análises, selecione Abrir e, em seguida, selecione Carregar.

Captura de tela que mostra os arquivos carregados no contêiner Azure.

Após o término do upload, você pode fechar o painel de upload de blob. Seus documentos agora estão em seu contêiner de armazenamento de análises de café.
Indexe os documentos
Depois de ter os documentos no armazenamento, você pode usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um Assistente de importação de dados. Com este assistente, você pode criar automaticamente um índice e indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice de pesquisa Azure AI.

No portal do Azure, acesse seu recurso Azure AI Search. Na página de Visão geral, selecione Importar dados.

Captura de tela que mostra o assistente de importação de dados.

Na página Conectar-se aos seus dados, na lista Fonte de dados, selecione Armazenamento de Blob do Azure. Preencha os detalhes do armazenamento de dados com os seguintes valores:
Fonte de dados: Armazenamento de Blob do Azure
Nome da fonte de dados: dados do cliente do café
Dados a serem extraídos: Conteúdo e metadados
Modo de análise: Padrão
String de conexão: *Selecione Escolher uma conexão existente. Selecione sua conta de armazenamento, selecione o contêiner de análises de café e, em seguida, clique em Selecionar.
Autenticação de identidade gerenciada: Nenhum
Nome do contêiner: este configuração é preenchida automaticamente após você escolher uma conexão existente.
Pasta de blob: Deixe em branco.
Descrição: Avaliações para lojas da Fourth Coffee.
Selecione Avançar: Adicionar habilidades cognitivas (Opcional).

Na seção Anexar Serviços Cognitivos, selecione seu recurso Azure AI services.

Na seção Adicionar enriquecimentos:
Altere o nome do conjunto de habilidades para conjunto de habilidades de café.
Selecione a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
Observação: É importante selecionar Habilitar OCR para ver todas as opções de campo enriquecido.

Garanta que o campo de dados de origem esteja definido como merged_content.
Altere o nível de granularidade do enriquecimento para Páginas (pedaços de 5000 caracteres).
Não selecione Habilitar enriquecimento incremental.
Selecione os seguintes campos enriquecidos:

Habilidade cognitiva	Parâmetro	Nome do campo
Extrair nomes de locais	 	locais
Extrair frases-chave	 	frasesChave
Detectar sentimento	 	sentimento
Gerar tags de imagens	 	tagsImagem
Gerar legendas de imagens	 	legendaImagem
Sob Salvar enriquecimentos em um Knowledge Store, selecione:
Projeções de imagem Azure
Documentos
Páginas
Frases-chave
Entidades
Detalhes da imagem
Referências de imagem
Observação: Se aparecer um aviso solicitando uma String de Conexão da Conta de Armazenamento.

Captura de tela que mostra o aviso da tela de conexão da conta de armazenamento com 'Escolher uma conexão existente' selecionado.

Selecione Escolher uma conexão existente. Escolha a conta de armazenamento que você criou anteriormente.
Clique em + Contêiner para criar um novo contêiner chamado knowledge-store com o nível de privacidade definido como Privado e selecione Criar.
Selecione o contêiner knowledge-store e, em seguida, clique em Selecionar na parte inferior da tela.
Selecione Projeções de blob Azure: Documento. Uma configuração para Nome do contêiner com o contêiner knowledge-store auto-populado é exibida. Não altere o nome do contêiner.

Selecione Avançar: Personalizar índice de destino. Altere o nome do índice para índice de café.

Garanta que a Chave esteja definida como metadata_storage_path. Deixe o nome do Sugestor em branco e o modo de pesquisa autopreenchido.

Revise as configurações padrão dos campos de índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

Captura de tela que mostra o painel de índice personalizado com o nome do índice inserido e 'Filtrável' selecionado para um campo de índice padrão.

Selecione Avançar: Criar um indexador.

Altere o nome do Indexador para indexador de café.

Deixe o Agendamento definido como Uma vez.

Expanda as opções Avançadas. Garanta que a opção Base-64 Encode Keys esteja selecionada, pois a codificação de chaves pode tornar o índice mais eficiente.

Selecione Enviar para criar o recurso de origem de dados, conjunto de habilidades, índice e indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
Extrai os campos de metadados do documento e conteúdo da fonte de dados.
Executa o conjunto de habilidades de habilidades cognitivas para gerar mais campos enriquecidos.
Mapeia os campos extraídos para o índice.
Volte para a página do seu recurso Azure AI Search. No painel esquerdo, em Gerenciamento de Pesquisa, selecione Indexadores. Selecione o indexador coffee-indexer recém-criado. Espere um minuto e selecione &orarr; Atualizar até que o Status indique sucesso.

Selecione o nome do indexador para ver mais detalhes.

Captura de tela que mostra o indexador coffee-indexer Indexador criado com sucesso.

Consulte o índice
Use o Explorer de Pesquisa para escrever e testar consultas. O Explorer de Pesquisa é uma ferramenta integrada ao portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Explorer de Pesquisa para escrever consultas e revisar resultados em JSON.

Na página Visão geral do seu serviço de pesquisa, selecione Explorador de Pesquisa na parte superior da tela.

Captura de tela de como encontrar o Explorer de Pesquisa.

Observe como o índice selecionado é o café-index que você criou. Abaixo do índice selecionado, altere a visualização para a visualização JSON.

Captura de tela do Explorer de Pesquisa.

No campo do editor de consulta JSON, copie e cole:

Código
{
    "pesquisa": "*",
    "contagem": true
}
Selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo seus resultados de pesquisa.

Agora vamos filtrar por localização. No campo do editor de consulta JSON, copie e cole:
Código
{
 "pesquisa": "locais:'Chicago'",
 "contagem": true
}
Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com uma localização de Chicago. Você deve ver 3 no campo @odata.count.

Agora vamos filtrar por sentimento. No campo do editor de consulta JSON, copie e cole:
Código
{
 "pesquisa": "sentimento:'negativo'",
 "contagem": true
}
Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra as avaliações com um sentimento negativo. Você deve ver 1 no
