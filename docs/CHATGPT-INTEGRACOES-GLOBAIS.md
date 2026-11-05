# Enginer IA — integrações reutilizáveis em qualquer chat ChatGPT

Status: plano de conexão, **não instalação concluída**. Atualizado: 2026-11-04.

## Objetivo e fronteiras

O usuário quer usar as ferramentas com sua conta ChatGPT em **conversas novas ou existentes**, não somente numa conversa, um projeto GitHub ou uma sessão Codex.

**Distinção essencial:**
1. Repositório GitHub armazena código, documentação e Skills. Não executa as aplicações automaticamente.
2. Skill `.agents/skills/` é descoberta pelo **Codex no checkout do projeto**, não é uma Skill global automaticamente instalada em todas as conversas do ChatGPT.
3. Plugin ChatGPT conectado à **conta** pode ser selecionado em conversas distintas. A seleção da ferramenta normalmente é feita para a mensagem em que será usada; reconvocar com `@` quando precisar de nova chamada.
4. Servidor MCP requer aplicação/endpoint real, TLS e autenticação. Apenas escrever arquivos de integração no GitHub não disponibiliza um MCP no ChatGPT.
5. Um aplicativo executado no Mac pode ajudar a digitar/falar em qualquer ChatGPT aberto **naquele Mac**; isso não dá ao ChatGPT web acesso direto ao localhost nem transfere a funcionalidade ao iPhone.

## Escopo aprovado

- INCLUIR: Open Higgsfield, VoiceStudio, OpenSEO, OpenWhispr.
- EXPLICAR, SEM MIGRAR: AppFlowy, alternativa a Notion (notas, páginas, bancos de dados e colaboração). O ChatGPT já possui conexão Notion; não importar ou duplicar dados sem necessidade/consentimento.
- EXCLUIR: OpenProject, Cal.diy, Chatwoot, Cap.
- Proteção: custo adicional zero, sem novos serviços tarifados, sem cruzar dados pessoais e corporativos, sem senhas/chaves em GitHub; alterações em produção só com autorização.

## Matriz de conexão

| Ferramenta | Realidade técnica | Ação verificável e gratuita | Status |
| --- | --- | --- | --- |
| OpenSEO | O serviço oficial tem plugin ChatGPT/MCP remoto em https://app.openseo.so/mcp, com OAuth. No ChatGPT web o plugin oficial é preferível, pois disponibiliza Skills e ferramentas. Uso de dados DataForSEO pode ser pago. | O usuário conecta o plugin a sua conta: https://chatgpt.com/plugins/plugin_asdk_app_6abddd2c7e4c819191f1398c4bd8e9b3 (link publicado pela documentação do projeto; disponibilidade real depende do diretório e plano). Aprovar somente login OAuth, **sem chave de API na conversa**. Testar `whoami` e `list_projects` ou 'Use OpenSEO to check my connection and list my projects. Do not run SEO research or create anything.' | AGUARDA usuário instalar/conectar; endpoint não testado na conta. |
| OpenWhispr | Aplicativo de ditado local, permite entrada por voz em qualquer janela do Mac. GitHub: https://github.com/OpenWhispr/openwhispr. Não é plugin ChatGPT nem um serviço MCP global por si. | Instalar edição oficial adequada para Apple Silicon somente no Mac com acesso local, aprovar permissões de microfone e acessibilidade se solicitadas; selecionar modelo **local**. Testar texto em navegador e ChatGPT. Não habilitar BYOK/cloud. | AGUARDA execução/validação local no Mac. |
| VoiceStudio | Software desktop/local para voz, https://github.com/debpalash/VoiceStudio. Requer modelos, espaço/CPU e permissões adequadas; alguns pesos têm licença **não comercial** mesmo com aplicativo AGPL. | Auditar o modelo exato e uso pretendido, baixar instalador oficial, validar funcionamento no Mac. Para chamá-lo como ferramenta em chat remoto, seria necessária API + gateway MCP autenticado e túnel/serviço seguro, ainda não existentes. | AGUARDA licença/execução local; sem conexão global. |
| Open Higgsfield | Aplicativo web https://github.com/wide-trace/open-higgsfield; código sem licença explícita identificável no repositório raiz na auditoria. README exige chave paga de plataforma para gerar imagens/vídeos. | Não copiar/redistribuir código sem esclarecimento de licença. Só testar serviço hospedado se houver franquia comprovadamente gratuita e com limite explícito. Não contratar créditos nem criar chave paga. Não há plugin oficial identificado para **esse clone open source**. | BLOQUEADO por licença/custo, sem conexão global. |
| AppFlowy | Alternativa open source ao Notion: https://github.com/AppFlowy-IO/AppFlowy. | Informar o que faz e comparação com Notion conectado; adiar instalação até confirmar uma necessidade não atendida. | FORA da implantação inicial. |

## Conectar OpenSEO em todas as conversas (na conta)

1. Abrir link de plugin acima no ChatGPT. Selecionar instalar e autorizar o login OpenSEO, se a opção existir. Se a tela pedir assinatura, não contratar.
2. Iniciar **nova conversa**. Escolher OpenSEO via `@` ou menu de ferramentas. Enviar mensagem somente de verificação de login/projetos, sem fazer pesquisa SEO paga.
3. Se plugin indisponível: investigar se **a conta** permite Plugin > + > Add custom MCP server. No contexto de planos individuais a opção pode faltar; não presumir autorização. Endpoint remoto do fornecedor `https://app.openseo.so/mcp`, OAuth, sem tokens no chat. Consultar a documentação OpenAI atual: https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt .
4. Se a conta não tem acesso a plugin/custom MCP, **nenhuma edição de GitHub pode contornar essa limitação**. Manter a integração pronta para quando houver superfície habilitada e usar os plugins já autorizados.
5. "Global" significa a conexão disponível na conta, **não** invocação automática em toda mensagem. A autorização do usuário e seleção da ferramenta ainda são necessárias quando a chamada for feita.

## Gates para qualquer bridge futuro

- Não hospedar um servidor novo sem motivo comprovado. Ponto de conexão remoto precisa HTTPS, login escopado, controle de acesso, isolamento dos recursos de empresa, segredos fora do GitHub e orçamento zero comprovado.
- Voz local e imagem local/externa têm privilégios diferentes. Uma bridge deve ser de menor privilégio e inicialmente read-only/teste.
- Registrar executáveis, origem e versão; nunca usar scripts remotos em `curl | sh` sem inspeção. Dispositivos pessoais requerem consentimento para permissões de microfone, disco, arquivos de voz e rede.
- Para produção/empresa: exigir revisão, backups, métricas e aprovação específica.
- Marcar cada item distintamente: documentação pronta; aplicativo instalado; serviço executando; MCP registrado; conector autenticado; **ferramenta testada em nova conversa**.

## Fontes

- OpenSEO ChatGPT: https://openseo.so/docs/chatgpt
- OpenSEO MCP: https://openseo.so/docs/mcp
- OpenAI custom MCP: https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt
- OpenWhispr: https://github.com/OpenWhispr/openwhispr
- VoiceStudio license notice: https://github.com/debpalash/VoiceStudio/blob/main/LICENSE-NOTICE.md
- Open Higgsfield: https://github.com/wide-trace/open-higgsfield
- AppFlowy: https://github.com/AppFlowy-IO/AppFlowy

Este documento não presume que o OpenSEO foi conectado, que um aplicativo roda no Mac ou que existe servidor remoto. Não modifica códigos de terceiros, plugins, cloud ou produção.
