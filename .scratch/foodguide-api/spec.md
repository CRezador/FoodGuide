Status: ready-for-agent

# Spec: API do FoodGuide

## Problema

Caio está fazendo o desafio de estudo do primeiro módulo da formação: uma API de receitas em Laravel, com contas de usuário, CRUD de Receitas com regras de permissão e dois Relatórios semanais por e-mail. Hoje o repositório tem só o starter kit Laravel 13 + Inertia/Vue. Não existem rotas de API, o Sanctum não está instalado e não há domínio de receitas. A entrega é a URL do código no GitHub, avaliada contra os requisitos do enunciado e as dicas (Sanctum, `apiResource`, Form Requests, API Resources, Notifications, Policies, Providers para regras de senha).

## Solução

Uma API REST em JSON, autenticada por token do Sanctum, na qual um Usuário:

- cria a conta, entra, sai e gerencia a própria conta;
- publica Receitas (públicas ou privadas) com Ingredientes estruturados e Modo de preparo em passos;
- navega pelas Receitas públicas de outros Usuários, com paginação e filtro por Categoria;
- curte Receitas, e cada abertura de Receita gera uma Visualização;
- recebe toda segunda-feira o Relatório Geral e, se teve atividade, o Relatório do Autor.

As telas Inertia ficam para depois (ADR 0001). O vocabulário segue o `GLOSSARY.md`.

## Histórias de usuário

### Conta

1. Como visitante, quero me cadastrar com nome, e-mail e senha, para ter uma conta no FoodGuide.
2. Como visitante, quero receber um token já no cadastro, para começar a usar a API sem um login extra.
3. Como visitante, quero que o cadastro rejeite um e-mail já usado, para que cada conta seja única.
4. Como visitante, quero que o cadastro rejeite senhas fracas (menos de 8 caracteres, sem letras ou sem números), para que minha conta fique protegida.
5. Como visitante, quero que o cadastro exija confirmação da senha, para não errar a digitação.
6. Como Usuário, quero entrar com e-mail e senha e receber um token, para acessar as rotas protegidas.
7. Como Usuário, quero receber um erro claro quando a senha ou o e-mail estiverem errados, sem descobrir qual dos dois errou.
8. Como Usuário, quero entrar de vários dispositivos ao mesmo tempo, cada um com o próprio token.
9. Como Usuário, quero sair revogando só o token do dispositivo atual, para continuar logado nos outros.
10. Como Usuário, quero ver os dados da minha conta (id, nome, e-mail, datas), para conferir o que está cadastrado.
11. Como Usuário, quero editar meu nome e e-mail, para manter a conta atualizada.
12. Como Usuário, quero que a troca de e-mail rejeite um e-mail já usado por outra conta.
13. Como Usuário, quero trocar a senha informando a atual, para que alguém com meu token não consiga trocá-la sozinho.
14. Como Usuário, quero excluir minha conta informando a senha atual, para não perder tudo por engano.
15. Como Usuário, quero que excluir a conta apague minhas Receitas, Curtidas, Visualizações e tokens, para não deixar rastros.
16. Como Usuário, quero que minha senha nunca apareça em nenhuma resposta e seja guardada criptografada.
17. Como visitante, quero receber 401 em qualquer rota protegida sem token válido.

### Receitas

18. Como Autor, quero criar uma Receita com título, descrição, Ingredientes, Modo de preparo, tempo de preparo em minutos, rendimento em porções, Categoria e URL de imagem opcional.
19. Como Autor, quero informar cada Ingrediente com nome, quantidade opcional e unidade, para registrar coisas como "sal a gosto".
20. Como Autor, quero escolher a unidade de uma lista fixa (g, kg, ml, l, xícara, colher de sopa, colher de chá, unidade, pitada, a gosto).
21. Como Autor, quero que os Ingredientes e os passos do Modo de preparo mantenham a ordem em que os enviei.
22. Como Autor, quero escolher a Categoria de uma lista fixa (Café da manhã, Entrada, Prato principal, Acompanhamento, Sopa, Salada, Sobremesa, Lanche, Bebida).
23. Como Autor, quero que minha Receita nasça pública, mas poder marcá-la como privada.
24. Como Autor, quero receber erros de validação claros quando faltar campo obrigatório, quando um valor for inválido (rendimento menor que 1, tempo não inteiro, URL inválida, Categoria ou unidade fora da lista) ou quando a lista de Ingredientes ou de passos vier vazia.
25. Como Autor, quero ser registrado automaticamente como Autor da Receita, sem poder criá-la em nome de outra pessoa.
26. Como Usuário, quero listar as Receitas públicas paginadas (15 por página), das mais novas para as mais antigas.
27. Como Usuário, quero filtrar a listagem por Categoria.
28. Como Usuário, quero que a listagem traga, em cada Receita, o Autor (id e nome), a Categoria (valor e rótulo), a contagem de Curtidas e se eu já curti.
29. Como Usuário, quero que a listagem nunca mostre Receitas privadas, nem as minhas.
30. Como Autor, quero listar todas as minhas Receitas, públicas e privadas, paginadas.
31. Como Usuário, quero ver uma Receita pelo id com todos os detalhes: Ingredientes, passos, Autor, contagem de Curtidas e se eu curti.
32. Como Usuário, quero receber 404 ao abrir a Receita privada de outra pessoa, como se ela não existisse.
33. Como Autor, quero ver minhas próprias Receitas privadas.
34. Como Autor, quero editar qualquer campo da minha Receita, inclusive a Visibilidade.
35. Como Autor, quero que, ao enviar a lista de Ingredientes ou de passos na edição, ela substitua a lista inteira.
36. Como Autor, quero editar só alguns campos sem reenviar os outros.
37. Como Usuário, quero receber 403 ao tentar editar ou excluir a Receita pública de outra pessoa.
38. Como Usuário, quero receber 404 ao tentar editar ou excluir a Receita privada de outra pessoa.
39. Como Autor, quero excluir minha Receita, levando junto Ingredientes, Curtidas e Visualizações dela.
40. Como Usuário, quero ver as datas de criação e de atualização de cada Receita.

### Curtidas

41. Como Usuário, quero curtir uma Receita pública de outra pessoa.
42. Como Usuário, quero que curtir de novo uma Receita já curtida não duplique a Curtida.
43. Como Usuário, quero desfazer minha Curtida.
44. Como Autor, quero ser impedido de curtir minha própria Receita, para não inflar meus números.
45. Como Usuário, quero receber 404 ao tentar curtir a Receita privada de outra pessoa.

### Visualizações

46. Como Autor, quero que cada abertura da minha Receita por outro Usuário gere uma Visualização, para saber o que faz sucesso.
47. Como Autor, quero que um mesmo Usuário abrindo a mesma Receita várias vezes no mesmo dia conte como uma só Visualização.
48. Como Autor, quero que o mesmo Usuário volte a contar em outro dia.
49. Como Autor, quero que minhas próprias aberturas não contem como Visualização.
50. Como Autor, quero que o "dia" siga o horário de Brasília.

### Relatórios

51. Como Usuário, quero receber toda segunda-feira às 8h (Brasília) o Relatório Geral, com as 10 Receitas públicas mais visualizadas na Semana de referência.
52. Como Usuário, quero que o Relatório Geral mostre, para cada Receita, o título, o Autor e o número de Visualizações na semana.
53. Como Usuário, quero não receber o Relatório Geral numa semana sem nenhuma Visualização.
54. Como Usuário, quero que o Relatório Geral nunca mostre Receitas privadas, nem as que viraram privadas ou foram excluídas antes do envio.
55. Como Autor, quero receber toda segunda-feira às 8h o Relatório do Autor, com as minhas 5 Receitas mais visualizadas e as 5 mais curtidas na Semana de referência.
56. Como Autor, quero que o Relatório do Autor inclua minhas Receitas privadas, já que é só para mim.
57. Como Autor, quero não receber o Relatório do Autor numa semana em que nenhuma Receita minha teve Visualização ou Curtida.
58. Como Autor, quero que as Curtidas do ranking sejam só as recebidas na Semana de referência.
59. Como leitor de um Relatório, quero que, no empate, a Receita mais recente apareça primeiro.
60. Como leitor de um Relatório, quero que só entrem no ranking Receitas com pelo menos uma Visualização (ou Curtida, no ranking de Curtidas) na semana.
61. Como Usuário, quero que a Semana de referência seja a segunda a domingo anterior ao envio.

## Decisões de implementação

### Arquitetura e plataforma

- **API primeiro (ADR 0001).** Todas as rotas ficam no arquivo de rotas de API, com o prefixo `/api`, e respondem JSON. O starter kit Inertia fica sem uso.
- **Sanctum por token.** Instalar `laravel/sanctum`, já aprovado durante o grilling. O model User usa tokens de API pessoais.
- **Fuso horário.** A aplicação passa a usar `America/Sao_Paulo`, valendo para o "dia" da Visualização, a Semana de referência e o horário do agendamento.
- **Regras de senha.** `Password::defaults()` definido no AppServiceProvider: mínimo de 8 caracteres, com letras e números. Cadastro e troca de senha usam essas regras. A senha usa o cast `hashed` do model.
- **Validação** fica em Form Requests, **formatação das respostas** em API Resources e **permissões** em Policies, conforme as dicas do desafio.

### Contrato da API

Todas as rotas exigem token, exceto cadastro e login.

| Método | Rota | O que faz |
| --- | --- | --- |
| POST | `/api/register` | Cria o Usuário e devolve o Usuário e um token (201). |
| POST | `/api/login` | Devolve o Usuário e um novo token. Credenciais inválidas dão 422 com mensagem genérica. |
| POST | `/api/logout` | Revoga o token atual (204). |
| GET | `/api/user` | Dados da própria conta. |
| PUT/PATCH | `/api/user` | Edita nome e e-mail. Senha nova só junto de `current_password` e confirmação. |
| DELETE | `/api/user` | Exige `password` atual. Apaga a conta em cascata (204). |
| GET | `/api/user/recipes` | Receitas do Autor autenticado, públicas e privadas, paginadas. |
| GET | `/api/recipes` | Receitas públicas, mais novas primeiro, 15 por página, `?category=` opcional. |
| POST | `/api/recipes` | Cria uma Receita do Usuário autenticado (201). |
| GET | `/api/recipes/{recipe}` | Detalhe da Receita. Registra uma Visualização. |
| PUT/PATCH | `/api/recipes/{recipe}` | Edita a Receita (só o Autor). |
| DELETE | `/api/recipes/{recipe}` | Exclui a Receita (só o Autor) (204). |
| POST | `/api/recipes/{recipe}/like` | Curte. É idempotente. |
| DELETE | `/api/recipes/{recipe}/like` | Desfaz a Curtida. É idempotente (204). |

- As rotas de Receita usam `apiResource`.
- **Respostas de erro.** Uma Receita privada acessada por quem não é o Autor responde **404** em qualquer rota (ver, editar, excluir, curtir). Editar ou excluir a Receita pública de outro Usuário responde **403**. Curtir a própria Receita responde **403**.
- **Formato da Receita na resposta:**
  - campos básicos: `id`, `title`, `description`, `preparation_time_minutes`, `servings`, `image_url`, `is_public`, `created_at`, `updated_at`;
  - `ingredients`, como lista ordenada de `{ name, quantity, unit: { value, label } }`;
  - `steps`, como lista ordenada de strings;
  - `category`, como `{ value, label }`;
  - `author`, como `{ id, name }`;
  - `likes_count` e `liked_by_me`.

  A listagem carrega Autor e contagens sem N+1.
- **Formato do Usuário na resposta:** `id`, `name`, `email`, `created_at`, `updated_at`. Nunca inclui a senha nem tokens.

### Esquema de dados

- **recipes**: Autor (FK para users, cascata), título, descrição, passos (JSON, lista ordenada de strings), tempo de preparo em minutos (inteiro), porções (inteiro ≥ 1), Categoria (string guardando o valor do enum), URL da imagem (nullable), `is_public` (padrão true) e timestamps. Tem índice para a listagem pública (visibilidade + data de criação) e para o filtro por Categoria.
- **recipe_ingredients**: Receita (FK, cascata), posição (inteiro, define a ordem), nome, quantidade (decimal, nullable) e unidade (string guardando o valor do enum). São linhas por Receita, sem catálogo compartilhado.
- **recipe_likes**: Usuário (FK, cascata), Receita (FK, cascata) e timestamps. Tem índice único em (Usuário, Receita). A data de criação é o que o ranking semanal de Curtidas usa.
- **recipe_views**: Usuário (FK, cascata), Receita (FK, cascata), data da visualização (date, no fuso de Brasília) e timestamps. O índice único em (Usuário, Receita, data) garante "uma por dia". A gravação é um insert que ignora duplicata.

### Enums

- **Categoria**: chaves TitleCase em inglês e rótulos em português: `Breakfast` (Café da manhã), `Appetizer` (Entrada), `MainCourse` (Prato principal), `SideDish` (Acompanhamento), `Soup` (Sopa), `Salad` (Salada), `Dessert` (Sobremesa), `Snack` (Lanche), `Drink` (Bebida). O enum expõe um método de rótulo.
- **Unidade**: `Gram` (g), `Kilogram` (kg), `Milliliter` (ml), `Liter` (l), `Cup` (xícara), `Tablespoon` (colher de sopa), `Teaspoon` (colher de chá), `Unit` (unidade), `Pinch` (pitada), `ToTaste` (a gosto).

### Validação da Receita

- **Na criação**, são obrigatórios: título (string, máx. 255), descrição, `preparation_time_minutes` (inteiro ≥ 1), `servings` (inteiro ≥ 1), `category` (valor do enum), `ingredients` (array com pelo menos um item) e `steps` (array com pelo menos um item, cada um uma string não vazia).
- **Cada Ingrediente**: `name` obrigatório, `quantity` opcional (numérico > 0) e `unit` obrigatória (valor do enum).
- **Campos opcionais**: `image_url` (URL válida) e `is_public` (boolean, padrão true).
- **Na edição**, as mesmas regras com `sometimes`. Se `ingredients` ou `steps` vierem, substituem a lista inteira.
- **O Autor nunca vem do corpo da requisição.** É sempre o Usuário autenticado.

### Policy de Receita

- `view`: a Receita é pública, ou o Usuário é o Autor.
- `update` e `delete`: só o Autor.
- `like`: a Receita é pública e o Usuário não é o Autor.
- **404 vs. 403.** Quando a Receita é privada e o Usuário não é o Autor, toda ação nega como 404 (`denyAsNotFound`). Os demais casos negados respondem 403.

### Visualização

- Registrada pelo próprio fluxo de `GET /api/recipes/{recipe}`, depois da autorização.
- Não registra quando o Usuário é o Autor.
- A unicidade por (Usuário, Receita, dia) é garantida pelo índice único. Uma tentativa duplicada é ignorada sem erro.

### Relatórios

- **Duas Notifications por e-mail**, uma para o Relatório Geral e outra para o Relatório do Autor (no código, nomes em inglês, por exemplo `WeeklyTopRecipesReport` e `WeeklyAuthorReport`). Ambas implementam `ShouldQueue` e são montadas com `MailMessage`. O mailer continua `log` em desenvolvimento. Trocar para SMTP é só configuração.
- **Dois comandos Artisan**, um por Relatório. Cada comando:
  - calcula a Semana de referência a partir de "agora": a segunda 00:00 até o domingo 23:59:59 da semana anterior, no fuso de Brasília;
  - consulta os rankings;
  - dispara as Notifications.

  Os comandos ficam agendados para toda segunda às 08:00 em `routes/console.php`.
- **Ranking do Relatório Geral**: Receitas **públicas no momento do envio**, contando as Visualizações cuja data cai na Semana de referência. Mostra as top 10, em ordem decrescente de Visualizações; no empate, vence a Receita criada mais recentemente. Só entram Receitas com pelo menos uma Visualização. Se o ranking sair vazio, nada é enviado. Se não, o mesmo ranking vai para **todos** os Usuários.
- **Ranking do Relatório do Autor**: para cada Autor, as top 5 Receitas dele por Visualizações na semana e as top 5 por Curtidas criadas na semana. Inclui Receitas privadas, usa o mesmo desempate e só entram Receitas com contagem ≥ 1. Só recebem o Relatório os Autores com pelo menos uma Visualização ou Curtida na semana. Se um dos dois rankings vier vazio, o e-mail mostra uma linha avisando que não houve atividade naquela seção.
- **Separação entre consulta e envio.** O cálculo dos rankings fica num lugar separado da Notification (uma classe ou um escopo de consulta), para que a Notification só formate. Os testes continuam entrando pelo comando.

## Decisões de teste

- **Bons testes verificam comportamento externo:** status HTTP, JSON da resposta, estado do banco e Notifications enviadas. Nunca chamam Policy, Form Request, Resource ou Model diretamente. Uma refatoração interna não deve quebrar nenhum teste.
- **Ponto de teste 1: a API HTTP.** Testes de Feature em Pest com `postJson`, `getJson` etc. e `Sanctum::actingAs` para autenticar. Cobrem:
  - cadastro, login e logout, incluindo o logout revogar só o token atual;
  - conta (ver, editar, trocar senha, excluir em cascata);
  - CRUD de Receitas com validação;
  - Visibilidade (404/403);
  - paginação e filtro;
  - substituição de listas na edição;
  - Curtidas (idempotência, própria Receita, Receita privada);
  - Visualizações (deduplicação por dia com `travelTo`, Autor não conta).
- **Ponto de teste 2: os comandos dos Relatórios.** Os testes criam Receitas, Visualizações e Curtidas em datas controladas (`travelTo`), rodam o comando com `artisan()` e usam `Notification::fake()`. Eles verificam:
  - quem recebeu e quem não recebeu;
  - que Receitas aparecem e em que ordem, inspecionando os dados da Notification;
  - os limites da Semana de referência (domingo 23:59 entra, segunda 00:00 seguinte não);
  - o desempate;
  - a exclusão de Receitas privadas no Relatório Geral e a inclusão no Relatório do Autor;
  - o não envio quando não há atividade.

  Um teste à parte confere que os dois comandos estão agendados para segunda às 08:00.
- **Factories** para User, Recipe (com estado `private`), RecipeIngredient, RecipeLike e RecipeView. Os testes usam as factories e seus estados em vez de montar registros na mão.
- **Prior art:** ainda não existe teste de domínio, só os `ExampleTest` do starter kit. O `tests/Pest.php` já aplica `RefreshDatabase` aos testes de Feature, e o tratamento de exceções já devolve JSON para `api/*`.

## Fora do escopo

- Telas Inertia/Vue (extensão futura, ADR 0001).
- Envio real de e-mail (SMTP ou Mailpit): o mailer fica `log`.
- Acesso de visitantes sem login às Receitas.
- Perfis públicos de outros Usuários e rotas `/users/{id}`.
- Papel de administrador e moderação.
- Catálogo compartilhado de ingredientes e busca por ingrediente.
- CRUD de Categorias (a lista é fixa).
- Upload de imagem: só URL.
- Busca textual, ordenações alternativas e comentários.
- Recuperação de senha e verificação de e-mail.
- Soft delete de contas ou Receitas.
- Rate limiting além do padrão do framework.
- Coleção do Postman (o Caio cria por conta própria).

## Notas

- O glossário (`GLOSSARY.md`) define Usuário, Autor, Receita, Visibilidade, Ingrediente, Modo de preparo, Categoria, Curtida, Visualização, Semana de referência, Relatório, Relatório Geral e Relatório do Autor. Use esses termos nos tickets, testes e mensagens.
- O enunciado sugere o LaraDumps para depuração. É uma ferramenta de desenvolvimento, opcional, e não faz parte da spec: se for instalada, precisa de aprovação, como toda dependência.
- Instalar o Sanctum é a única dependência nova prevista.
