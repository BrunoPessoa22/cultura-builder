# Capítulo 10: Seu Primeiro Projeto com IA

> "O expert em qualquer coisa já foi um desastre ambulante que não desistiu."
> — Adaptado livremente

## O Projeto Que Muda Tudo

14 de setembro de 2024, 14h23, Guarulhos. 

Melissa Takahashi, 31 anos, assistente administrativa, olhava fixamente para a tela do computador da empresa. À sua frente: uma planilha Excel com 3.847 linhas. A tarefa parecia impossível: consolidar dados de vendas dos últimos 6 meses, cruzar com informações de estoque e gerar um relatório completo.

Tempo estimado para conclusão: 3 dias.
Tempo disponível: até às 18h.

"Foi um momento de desespero que se transformou em momento de iluminação", Melissa relembra. "Tinha duas opções: chorar ou tentar algo completamente diferente. Escolhi tentar."

Melissa abriu o ChatGPT pela primeira vez para trabalho real. Até então, havia usado apenas para curiosidades e receitas. Mas o desespero é um grande professor.

**Primeira tentativa:**
"Como fazer Excel analisar 4 mil linhas e criar relatório automaticamente?"

O ChatGPT respondeu com fórmulas complexas, mas Melissa não entendeu nada. A frustração começou a subir. O relógio continuava correndo. 14h47.

**Segunda tentativa (mais honesta):**
"Não, você não entendeu. Eu NÃO SEI fazer isso. Me ensina como se eu fosse uma criança."

O ChatGPT imediatamente mudou de abordagem. Começou a explicar passo a passo, como se estivesse ensinando uma criança: "Clique aqui. Digite exatamente isso. Aperte Enter."

**Timeline do milagre:**
- **15h23:** Melissa tinha uma macro funcionando, processando dados automaticamente
- **15h45:** Relatório completo pronto, com gráficos e insights destacados
- **16h00:** Entrega ao chefe concluída

**Resultado:** Três dias de trabalho realizados em apenas 2 horas.

"O chefe ficou impressionado, mas eu fiquei ainda mais", Melissa conta. "Não foi apenas sobre entregar um relatório. Foi sobre descobrir que eu era capaz de criar coisas que sempre achei impossíveis para alguém como eu."

Naquela noite, Melissa não conseguiu dormir. Sua mente fervilhava com possibilidades infinitas. Se conseguia automatizar relatórios complexos, o que mais seria capaz de criar?

No fim de semana seguinte, Melissa tomou uma decisão: criaria seu primeiro projeto pessoal. Não para a empresa. Para ela mesma. Para provar que realmente podia.

## O Projeto: Organizador de Vida Pessoal

Melissa sempre teve um problema crônico com finanças pessoais. Gastava sem nenhum controle, nunca sabia exatamente para onde o dinheiro ia, e invariavelmente chegava ao fim do mês no vermelho.

Já havia tentado de tudo: apps de banco complicados, planilhas tediosas, métodos de organização diversos. Nada funcionava de verdade.

Então veio o insight: "Se o ChatGPT me ajudou com aquele relatório impossível, talvez possa me ajudar a resolver esse caos financeiro também."

Sábado, 9h. Melissa abriu ChatGPT com ideia vaga: "Quero criar algo que me ajude a controlar gastos de forma que eu realmente use."

Primeiro erro: pergunta genérica demais. ChatGPT devolveu dissertação sobre educação financeira. Inútil.

Melissa lembrou da lição do relatório. Seja específica. Seja detalhada. Seja humilde.

"Quero criar uma página web simples onde eu possa adicionar gastos do dia. Precisa ser MUITO simples. Só 3 campos: o que comprei, quanto custou, categoria (comida, transporte, etc). Quero ver total do mês e quanto gastei em cada categoria. Precisa funcionar no celular. Precisa ser bonito mas simples. Você pode criar o código completo para isso?"

ChatGPT respondeu com 3 arquivos:
- index.html (estrutura)
- styles.css (visual)
- script.js (funcionamento)

Melissa copiou. Colou no Notepad. Salvou. Abriu no navegador.

Mágica. Funcionava.

Mas era feio. Cores horríveis. Fonte feia. Melissa voltou ao ChatGPT.

"Funciona perfeito! Mas está feio. Pode deixar com visual moderno, cores suaves, tipo aplicativo do Nubank?"

ChatGPT refez CSS. Melissa atualizou. Lindíssimo.

Mas tinha problema: quando fechava navegador, perdia dados. Melissa, já pegando jeito:

"Como fazer para salvar os dados mesmo quando fecho o navegador?"

ChatGPT adicionou localStorage. Melissa não fazia ideia do que era, mas funcionou. Dados persistiam.

11h30: Melissa tinha app funcionando. Mas estava só no computador dela. Como compartilhar com amigas que tinham mesmo problema?

"Como coloco isso na internet para outras pessoas usarem?"

ChatGPT guiou para GitHub Pages. Grátis. Simples. Em 20 minutos, app estava online.

URL: melissa-financas.github.io

Melissa compartilhou no grupo do WhatsApp das amigas. Resposta imediata:

"MEU DEUS, VOCÊ FEZ ISSO?"
"Quanto cobrou?"
"Faz um pra mim?"
"Você virou programadora?"

"Momento mais surreal da minha vida", Melissa emociona. "Eu, assistente administrativa, sendo tratada como desenvolvedora. Eu, que mal sabia ligar computador direito, com 'app' online."

## Anatomia do Primeiro Projeto

Vamos dissecar projeto da Melissa para entender elementos fundamentais de todo primeiro projeto:

### 1. Problema Pessoal Real

Melissa não tentou criar próximo Facebook. Não tentou revolucionar indústria. Não tentou impressionar ninguém. Resolveu problema próprio, real, imediato.

"Primeiro projeto precisa ser egoísta", defende Lucas Ribeiro, mentor de builders iniciantes. "Resolva SEU problema. Se você tem problema, milhões têm. Mas comece com você."

Ricardo Almeida, contador, confirma: "Primeiro projeto: calculadora de quanto imposto pago em compras online. Fiz para mim. Hoje 50 mil pessoas usam."

Marina Santos, professora: "Primeiro projeto: gerador de atividades para minha turma. Hoje vendo para 200 escolas."

### 2. Escopo Brutalmente Limitado

Projeto da Melissa fazia UMA coisa: registrar gastos. Não tinha login. Não tinha relatórios complexos. Não tinha integração bancária. Não tinha nada além do essencial.

"Feature creep mata primeiro projeto", alerta Fernando Dias, developer veterano. "Cada feature que você adiciona é chance de travar, desistir, nunca lançar."

Regra de ouro: se não cabe em uma frase, é complexo demais para primeiro projeto.

Bom: "App que registra gastos"
Ruim: "Sistema completo de gestão financeira pessoal com IA, blockchain e realidade aumentada"

### 3. Feedback Loop Imediato

Melissa viu resultado em minutos. Digitou código, abriu navegador, funcionou. Dopamina instantânea. Motivação para continuar.

"Primeiro projeto que demora mais de um dia para ver funcionando tem 90% chance de ser abandonado", estatística brutal de Ana Costa, que estudou 1000 builders iniciantes.

Escolha tecnologias com feedback imediato:
- HTML/CSS/JS: abre no navegador, vê resultado
- Python com Streamlit: roda comando, app abre
- No-code tools: arrasta, solta, funciona

Evite para primeiro projeto:
- Apps mobile nativos (complexidade de publicação)
- Sistemas distribuídos (debug impossível)
- Qualquer coisa que precisa de servidor

### 4. IA Como Copiloto, Não Piloto

Melissa não pediu para ChatGPT "fazer tudo". Ela dirigiu. IA executou. Melissa decidiu features. IA implementou. Melissa escolheu visual. IA codificou.

"Erro fatal é achar que IA vai criar produto sozinha", explica Patricia Wong, product manager. "IA é ferramenta. Incrivelmente poderosa, mas ferramenta. Você precisa ter visão, fazer escolhas, dar direção."

Como Melissa usou ChatGPT:
- Perguntas específicas, não genéricas
- Iteração constante (não aceitou primeira resposta)
- Feedback claro ("está feio", "perco dados")
- Ownership do resultado (MEU app, não app do ChatGPT)

### 5. Deploy Imediato

Melissa não esperou app estar "pronto" para colocar online. Assim que funcionou minimamente, deployou. Compartilhou. Recebeu feedback. Melhorou.

"Perfeccionismo é desculpa para não lançar", provoca João Medeiros, criador de 50+ projetos. "Lança merda. Recebe crítica. Melhora. Repete. É único caminho."

GitHub Pages foi escolha genial da Melissa (guiada pelo ChatGPT):
- Grátis para sempre
- Deploy em minutos
- URL própria
- Zero configuração

## Seu Primeiro Projeto: Vamos Construir Juntos

Chega de teoria. Vamos construir. Agora. Juntos. Passo a passo.

**Projeto: Gerador de Desculpas Criativas**

Por quê esse projeto?
- Simples mas útil
- Divertido (reduz pressão)
- Compartilhável (gera engajamento)
- Extensível (pode melhorar infinitamente)

### Passo 1: Abra ChatGPT

Cole exatamente este prompt:

```
Crie um gerador de desculpas criativas em HTML, CSS e JavaScript.
Requisitos:
- Interface limpa e moderna
- Botão grande "Gerar Desculpa"
- Mostra desculpa aleatória quando clica
- Mínimo 30 desculpas criativas e engraçadas
- Deve ter categorias: trabalho, relacionamento, social
- Visual responsivo para celular
- Cores vibrantes mas profissionais
- Botão para copiar desculpa

Crie os 3 arquivos completos: HTML, CSS e JavaScript
```

### Passo 2: Crie os Arquivos

ChatGPT vai gerar 3 blocos de código. 

1. Crie pasta chamada "desculpas"
2. Dentro, crie 3 arquivos:
   - index.html (copie primeiro bloco)
   - styles.css (copie segundo bloco)
   - script.js (copie terceiro bloco)

### Passo 3: Teste Localmente

1. Abra index.html no navegador (duplo clique)
2. Teste o botão
3. Veja desculpas aparecendo

Funciona? Ótimo! Não funciona? Volte ao ChatGPT:

"O código não está funcionando. O erro é: [descreva o que acontece]. Como corrigir?"

### Passo 4: Personalize

Volte ao ChatGPT com pedidos específicos:

"Adicione 20 desculpas mais específicas para quem trabalha com tecnologia"

"Mude as cores para tons de azul e roxo"

"Adicione animação quando nova desculpa aparece"

"Crie botão para compartilhar no WhatsApp"

### Passo 5: Deploy no GitHub Pages

1. Crie conta no GitHub (github.com)
2. Crie novo repositório: "gerador-desculpas"
3. Upload os 3 arquivos
4. Settings → Pages → Source → Deploy from branch
5. Aguarde 2 minutos
6. Seu app está em: seunome.github.io/gerador-desculpas

### Passo 6: Compartilhe

Mande link para 5 pessoas. Agora. Não amanhã. Agora.

"Fiz meu primeiro projeto com IA. O que acham?"

Prepare-se para duas reações:
1. "VOCÊ fez isso? Incrível!"
2. "Legal, mas seria melhor se..."

Ambas são vitórias. Primeira valida capacidade. Segunda dá direção para melhorar.

## Os Primeiros 7 Dias Após o Primeiro Projeto

Dia 1 após lançar: Euforia. Você é developer! Builder! Criador!

Dia 2: Ideias explodem. "Posso criar QUALQUER COISA!"

Dia 3: Paralisia. Tantas opções que não sabe o que criar depois.

Dia 4: Dúvida. "Foi sorte? Consigo fazer de novo?"

Dia 5: Procrastinação. "Preciso aprender mais antes do próximo."

Dia 6: Scrolling infinito de tutoriais.

Dia 7: Decisão. Ou você cria segundo projeto ou volta a ser consumidor.

"Semana após primeiro projeto é crucial", analisa Dra. Helena Peixoto, que estudou jornada de 500 builders. "70% não criam segundo projeto. Dos 30% que criam, 90% viram builders consistentes."

Como garantir que você está nos 30%:

**Compromisso Público**: Poste nas redes: "Criei primeiro projeto! Próximo sai sexta!"

**Momentum Mantido**: Segundo projeto deve ser mais simples que primeiro. Não mais complexo.

**Variação, Não Revolução**: Pegue primeiro projeto, mude tema. Gerador de desculpas → Gerador de elogios.

**Time Box Radical**: Segundo projeto em 2 horas. Não 2 dias. 2 horas.

## Estudos de Caso: Primeiros Projetos Que Viraram Algo Maior

### Caso 1: A Calculadora que Virou Startup

Bruno Ferreira, engenheiro civil desempregado, criou calculadora de materiais de construção. Primeiro projeto. Só calculava quantidade de tijolos para uma parede.

"Era ridiculamente simples", Bruno ri. "Mas pedreiro amigo usou e adorou. Pediu para adicionar cimento. Adicionei. Pediu areia. Adicionei."

6 meses depois: 10 mil usuários ativos.
1 ano depois: Empresa de construção ofereceu R$ 200 mil pelo app.
Hoje: Bruno tem startup de tech para construção civil, 20 funcionários.

"Tudo começou com calculadora de tijolo de 50 linhas de código."

### Caso 2: O Bot que Salvou Casamento

Carla Mendonça criou bot que mandava mensagem romântica diária para marido. Primeiro projeto. ChatGPT gerava mensagens, Zapier automatizava envio.

"Era brincadeira. Marido trabalhava muito, esquecia de pequenos gestos. Bot lembrava por ele."

Marido adorou. Contou para amigos. Amigos quiseram. Carla criou versão paga. R$ 9,90/mês.

Hoje: 5 mil assinantes. R$ 50 mil/mês. Salvou dezenas de relacionamentos (segundo depoimentos).

"Nunca imaginei que primeiro projeto brincalhão viraria negócio sobre amor."

### Caso 3: A Planilha que Virou SaaS

Eduardo Nakamura, vendedor, criou planilha automatizada para calcular comissões. Primeiro projeto. Google Sheets + Apps Script (com ajuda do ChatGPT).

"Empresa calculava comissões errado todo mês. Criei planilha para conferir. Descobri que perdi R$ 30 mil em 2 anos."

Eduardo mostrou para outros vendedores. Todos tinham mesmo problema. Eduardo transformou planilha em webapp. Cobrou R$ 50/mês.

Hoje: 500 clientes. R$ 25 mil/mês recorrente. Eduardo largou emprego de vendedor.

"Primeiro projeto resolveu MEU problema. Descobri que milhares tinham mesmo problema."

## Os Anti-Padrões: O Que NÃO Fazer no Primeiro Projeto

### Anti-Padrão 1: A Síndrome do Próximo Facebook

"Vou criar rede social que vai destronar Instagram!"

Não. Você não vai. E tentar isso como primeiro projeto é suicídio criativo.

Pedro Alves tentou. Gastou 3 meses. Nunca lançou. Desistiu de programar. "Me senti incompetente. Hoje sei que projeto era impossível para iniciante."

### Anti-Padrão 2: Tutorial Hell

Marina passou 6 meses fazendo cursos antes de criar primeiro projeto. Quando finalmente tentou, travou. Conhecimento teórico não se traduziu em habilidade prática.

"Assistir tutorial é como assistir vídeo de natação. Você não aprende a nadar. Só aprende nadando."

### Anti-Padrão 3: Stack Overflow Syndrome

Carlos escolheu stack "moderna": React + TypeScript + GraphQL + Docker + Kubernetes. Para fazer lista de tarefas.

"Gastei mais tempo configurando ambiente que criando. Desisti na terceira vez que Docker quebrou."

Primeiro projeto = tecnologia mais simples possível.

### Anti-Padrão 4: Perfeccionismo Paralisante

Ana refez primeiro projeto 15 vezes. Nunca estava bom suficiente. Nunca lançou. Código perfeito que ninguém usa vale menos que código ruim que alguém usa.

### Anti-Padrão 5: Modo Solitário

Roberto criou primeiro projeto em segredo. Não mostrou para ninguém. Não pediu feedback. Quando finalmente revelou, descobriu que ninguém precisava.

"Construí solução elaborada para problema que não existia."

## O Framework PRIMEIRO

Após analisar centenas de primeiros projetos bem-sucedidos, identificamos padrão. Framework PRIMEIRO:

**P**roblema pessoal
**R**ápido de construir (< 1 dia)
**I**nterface simples
**M**ínimo viável (1 feature core)
**E**xecutável localmente
**I**terativo (pode melhorar depois)
**R**esultado compartilhável
**O**riginal para você (não precisa ser único no mundo)

Melissa acertou todos sem saber. Por isso funcionou.

## Transformando Medo em Combustível

"E se meu código for horrível?"
Vai ser. E daí? ChatGPT não julga. GitHub não ri. Deploy não critica.

"E se ninguém usar?"
Provável. Mas você usou. Você criou. Você provou que pode. Isso vale mais que mil usuários.

"E se já existe algo melhor?"
Sempre existe. Google é melhor que seu buscador. Mas seu buscador é INFINITAMENTE melhor que buscador que você não criou.

"E se eu travar?"
Vai travar. Todo mundo trava. Diferença entre builder e não-builder: builder destrava pedindo ajuda (para IA, comunidade, Google) e continua.

Patricia Yamada, hoje CTO de startup, lembra primeiro projeto: "Timer pomodoro. Bugado. Feio. Inútil (existiam 500 melhores). Mas MEU. Provei que podia criar. Resto é história."

## Seu Compromisso Começa Agora

Pare de ler. Sério. Pare.

Abra ChatGPT. Agora.

Digite: "Me ajude a criar meu primeiro projeto. Tenho problema: [descreva problema real seu]. Como criar solução simples em HTML/CSS/JS?"

Siga instruções. Quebre. Conserte. Deploy. Compartilhe.

Não amanhã. Não "quando tiver tempo". Não "depois de estudar mais".

AGORA.

Melissa criou primeiro projeto em 2 horas no fim de semana. Você tem 2 horas. Todo mundo tem 2 horas. Questão é se você vai usar essas 2 horas para criar ou para consumir.

Relógio está correndo. ChatGPT está esperando. GitHub está pronto. Vercel está disponível.

Único ingrediente faltando é sua decisão de começar.

Próximo capítulo é sobre criar 10 projetos em 30 dias. Mas só faz sentido ler se você criou o primeiro.

Então: o que você está esperando?

Seu primeiro projeto aguarda.

---

*[Continua no Capítulo 11: 10 Projetos em 30 Dias - O Desafio que Transforma Amadores em Builders]*